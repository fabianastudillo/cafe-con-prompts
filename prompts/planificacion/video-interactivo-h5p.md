# Video interactivo con preguntas (H5P) en Moodle con Claude Code

> 🎬 **Video:** próximamente · ☕ **Categoría:** Planificación · 🛠️ **Requiere:** [Claude Code](https://claude.com/claude-code), [yt-dlp](https://github.com/yt-dlp/yt-dlp) y un Moodle con H5P Interactive Video

## Para qué sirve

Convierte un video de YouTube en una **actividad interactiva de Moodle**: un video interactivo H5P
que se detiene en el minuto justo y le hace una pregunta al estudiante, con retroalimentación y
nota en el libro de calificaciones. Es ideal para el momento **antes de clase** del aula invertida
([ver el prompt del aula invertida](aula-invertida-moodle.md)).

Tú das el enlace del video. Claude lee sus subtítulos (no descarga el video), arma una línea de
tiempo de los conceptos, la compara con tu sílabo y propone preguntas en el segundo exacto, cada
una con la frase del video que la justifica. **Tú revisas las preguntas** antes de que publique.

No es un prompt para el chat: se ejecuta en una sesión de **Claude Code**, que trabaja sobre tu
aula de Moodle.

## Antes de empezar

1. **Instala `yt-dlp`** (por ejemplo, `pipx install yt-dlp`). Si YouTube lo bloquea («Sign in to
   confirm you're not a bot»), el prompt usa la sesión de YouTube de tu navegador Chrome con
   `--cookies-from-browser chrome`: ten YouTube abierto con tu cuenta.
2. **Elige un video con subtítulos** (los automáticos sirven) que permita insertarse en otras
   páginas. Si es tuyo, mejor: no hay dudas de derechos.
3. **Comprueba que tu Moodle tiene H5P Interactive Video.** En el banco de contenido del curso, el
   botón «Añadir» debe ofrecer «Interactive Video». El prompt lo comprueba igual en el paso 1 y no
   necesita permisos de administrador si la librería ya está instalada.
4. **Prepara la carpeta `insumos/`** con el sílabo de la materia y el archivo `credenciales`
   (`urlmoodle`, `email` y `cookie`), igual que en el
   [prompt del aula invertida](aula-invertida-moodle.md#2-configura-el-archivo-credenciales).

> ⚠️ **La cookie da acceso a tu cuenta de Moodle mientras la sesión esté abierta.** No la
> compartas, no la subas a GitHub y no la muestres en pantalla.

Abre una terminal en la carpeta que contiene `insumos/`, inicia `claude`, rellena los cinco datos
y pega el prompt.

## Prompt

```text
Convierte un video de YouTube en una actividad interactiva de Moodle: un video interactivo H5P
que se detiene en el minuto justo y le hace una pregunta al estudiante.

Datos:
- Video: [URL del video de YouTube]
- Unidad: [unidad o tema del sílabo que cubre el video]
- Sección de Moodle: [nombre o número de la sección donde va la actividad]
- Preguntas: [número de preguntas; si no lo digo, 5 o 6]
- Calificación: [sí o no; si no lo digo, sí: la nota va al libro de calificaciones]

El sílabo y el acceso a Moodle están en `insumos/`. Nunca imprimas, copies ni hagas commit
del contenido del archivo `credenciales` (claves `urlmoodle:`, `email:` y `cookie:`, el
valor de `MoodleSession`). Si esta carpeta es un repositorio git, comprueba que `insumos/`
esté en `.gitignore`. Escribe en `scripts/` los programas que necesites; si ya hay un
cliente de Moodle ahí, reutilízalo.

## Paso 1 — Revisa Moodle

- Comprueba que la cookie sigue valiendo: si Moodle redirige al inicio de sesión, avísame
  para que la renueve y detente. No la busques en otro lado.
- Averigua qué librerías H5P tiene instaladas el sitio y en qué versión (H5P.InteractiveVideo
  y los tipos de pregunta que vayas a usar: H5P.MultiChoice, H5P.TrueFalse, H5P.Summary). El
  editor del banco de contenido las expone (`/contentbank/edit.php?contextid=<contexto del
  curso>&plugin=h5p&library=H5P.InteractiveVideo-<versión>` y su `ajax.php?action=libraries`).
  Nunca intentes instalar ni actualizar librerías: el paquete se arma para lo que hay. Si
  falta H5P.InteractiveVideo, dímelo y detente.
- Muéstrame las versiones que encontraste. No imprimas `sesskey` ni tokens.

## Paso 2 — Lee el video, sin descargarlo

Usa `yt-dlp` con `--no-playlist --skip-download`: solo metadatos y subtítulos, nunca el video.

- Anota título, canal, duración, si permite inserción (`playable_in_embed`) y la licencia.
- Descarga los subtítulos en el idioma del video en formato `vtt`. Prefiere los manuales a
  los automáticos.
- Si YouTube responde «Sign in to confirm you're not a bot» o faltan subtítulos por el
  «PO token», reintenta con `--cookies-from-browser chrome`. No imprimas ninguna cookie.
- Si el video no permite inserción o no tiene subtítulos, detente y dímelo: puedo darte
  otro video o una transcripción con tiempos. No adivines el contenido.

## Paso 3 — La línea de tiempo

Con los subtítulos, arma una línea de tiempo: en qué segundo empieza y termina la
explicación de cada concepto. Compárala con los resultados de aprendizaje de la unidad en
el sílabo y muéstrame:

- la línea de tiempo (mm:ss → concepto);
- qué resultados cubre el video, cuáles no y si dice algo que contradiga el sílabo o que sea
  un error conceptual.

## Paso 4 — Las preguntas (aquí te detienes)

Propón las preguntas en una tabla. Cada una lleva:

- el segundo en que aparece: **después** de la frase que la responde, nunca antes;
- el tipo: opción múltiple (una correcta y tres distractores plausibles) o verdadero/falso;
- la cita textual del subtítulo que la justifica. Si los subtítulos traen un error de
  transcripción evidente (un término técnico mal escrito), corrígelo en la pregunta y dilo;
- la retroalimentación de cada opción: por qué es correcta o incorrecta, sin regañar.

Reparte las preguntas a lo largo del video y no preguntes nada que el video no diga.
Detente y espera mi revisión. Si cambio algo, rehace la tabla antes de seguir.

## Paso 5 — Arma el paquete y publícalo

Con mi visto bueno:

- Arma un `.h5p` de solo contenido (`h5p.json` y `content/content.json`, sin carpetas de
  librerías), con `mainLibrary` H5P.InteractiveVideo y `preloadedDependencies` con las
  versiones del paso 1. El video va por su URL de YouTube (`mime: video/YouTube`).
- Cada pregunta con `pause: true` y `adaptivity.requireCompletion: true`: el video se
  detiene y no sigue hasta responder. Al final, un resumen.
- Todos los textos de la interfaz en español (`l10n` del video interactivo y textos de cada
  pregunta: «Comprobar», «Reintentar», «Mostrar solución», «Continuar»…). Sin ellos, los
  botones salen en inglés.
- Publícalo como actividad H5P (`mod_h5pactivity`) en la sección pedida, con el seguimiento
  de intentos activado y, si hay calificación, con la nota al libro de calificaciones (mejor
  intento). Nombre: «Video: <título corto>». Antes de subir el paquete, vacía el área de
  borrador del formulario: Moodle añade el archivo nuevo junto al viejo.
- Ábrela como profesor y comprueba que se despliega sin errores (Moodle muestra «Missing
  required library» u otro aviso si algo no cuadra) y que el video y las preguntas están.

## Paso 6 — Informe

Termina con:

- el enlace a la actividad;
- la tabla final de preguntas (segundo, tipo, pregunta, respuesta correcta);
- cómo probarla como estudiante: el profesor la ve en «modo de vista previa», que no
  registra intentos; hay que entrar con una cuenta de estudiante del curso.

Trabaja paso a paso y verifica cada uno contra la fuente real (los subtítulos, el aula) antes
de seguir. Solo te detienes en el paso 4, si falta algo que no puedes resolver o si la cookie
caducó.

```

## Campos a rellenar

| Campo | Ejemplo |
|---|---|
| `[URL del video de YouTube]` | `https://www.youtube.com/watch?v=-ign3aPxxtM` |
| `[unidad o tema del sílabo]` | Unidad 300: Criptografía de clave secreta y de clave pública |
| `[sección de Moodle]` | Unidad 300 |
| `[número de preguntas]` | 6 |
| `[sí o no]` | sí |

## Variaciones

- **Sin nota:** responde «no» en Calificación para una actividad formativa.
- **Sin bloquear el avance:** pide «que el video no se detenga obligatoriamente» si prefieres
  preguntas opcionales.
- **Pregunta abierta al final:** pide «añade al final una pregunta abierta para traer a clase».
- **Video sin subtítulos:** entrega una transcripción con tiempos (por ejemplo, la de «Mostrar
  transcripción» de YouTube) en `insumos/` y dile a Claude que la use.

## Consejos

- Revisa cada pregunta contra su cita: si la cita no la responde, pide que la cambie.
- Mira que el segundo de cada pregunta quede **después** de la explicación.
- Prueba la actividad con una cuenta de estudiante: como profesor la ves en vista previa y no se
  registran intentos.
