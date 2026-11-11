# Rúbrica y calificación en Moodle con Claude Code

> 🎬 **Video:** próximamente · ☕ **Categoría:** Evaluación · 🛠️ **Requiere:** [Claude Code](https://claude.com/claude-code) (terminal o Claude Desktop), `pdftotext` y un Moodle con calificación por rúbrica

## Para qué sirve

Crea en tu aula de Moodle la **tarea y la rúbrica** de una actividad de tu materia y, cuando
llegan las entregas, te ayuda a **calificarlas con esa rúbrica**. Claude lee la actividad en tu
planificación, las diapositivas que cita y tu sílabo, y propone una rúbrica con niveles que se
pueden comprobar en la entrega. Al calificar, propone un nivel por criterio con **la frase de la
entrega que lo justifica** y un comentario de retroalimentación.

Se detiene dos veces: **tú apruebas la rúbrica** antes de publicarla y **tú decides la nota**
antes de guardarla. Claude propone; tú calificas.

No es un prompt para el chat: se ejecuta en una sesión de **Claude Code** (en la terminal o en
la aplicación de escritorio [Claude Desktop](https://claude.com/download)), que trabaja sobre tu
aula de Moodle. Si solo quieres la rúbrica para pegarla en un documento, usa la
[Rúbrica analítica](rubrica-analitica.md), que funciona en cualquier chat.

## Antes de empezar

1. **Instala `pdftotext`** (paquete `poppler-utils` en Linux, `brew install poppler` en macOS):
   Claude lo usa para leer las entregas en PDF.
2. **Comprueba que tu Moodle permite calificar con rúbrica.** Al crear una tarea, en
   «Calificación» → «Método de calificación» debe aparecer «Rúbrica». Necesitas permiso de
   profesor con edición en el curso.
3. **Prepara la carpeta `insumos/`** con el sílabo, las presentaciones de la materia, tu
   planificación (si la tienes) y el archivo `credenciales` (`urlmoodle`, `email` y `cookie`),
   igual que en el
   [prompt del aula invertida](../planificacion/aula-invertida-moodle.md#2-configura-el-archivo-credenciales).

> ⚠️ **La cookie da acceso a tu cuenta de Moodle mientras la sesión esté abierta.** No la
> compartas, no la subas a GitHub y no la muestres en pantalla.

> ⚠️ **Claude leerá entregas de tus estudiantes, que son datos personales.** Sigue la política de
> tu institución sobre el uso de IA con trabajos de estudiantes. Si no la hay o no lo permite,
> pide entregas sin nombres o haz primero un piloto con entregas de prueba, como en el video.

Abre una terminal en la carpeta que contiene `insumos/` e inicia `claude` (o abre esa carpeta en
Claude Desktop), rellena los cinco datos y pega el prompt.

## Prompt

```text
Crea en Moodle la tarea y la rúbrica de una actividad de mi materia y, cuando haya entregas,
ayúdame a calificarlas con esa rúbrica. Tú propones; la nota la decido yo.

Datos:
- Actividad: [descripción de la actividad, o «el trabajo aplicado de la semana N de mi planificación»]
- Sección de Moodle: [nombre o número de la sección, y la semana o el momento donde va la tarea]
- Criterios: [número de criterios; si no lo digo, 5]
- Niveles: [número de niveles por criterio; si no lo digo, 4]
- Puntaje: [nota máxima de la tarea; si no lo digo, 10]

El sílabo, las presentaciones, la planificación y el acceso a Moodle están en `insumos/`. Nunca
imprimas, copies ni hagas commit del contenido del archivo `credenciales` (claves `urlmoodle:`,
`email:` y `cookie:`, el valor de `MoodleSession`), ni el `sesskey`. Si esta carpeta es un
repositorio git, comprueba que `insumos/` esté en `.gitignore`. Escribe en `scripts/` los
programas que necesites; si ya hay un cliente de Moodle ahí, reutilízalo. Habla con Moodle con
la cookie, como lo haría el navegador.

## Paso 1 — Revisa Moodle y lee la actividad

- Comprueba que la cookie sigue valiendo: si Moodle redirige al inicio de sesión, avísame para
  que la renueve y detente. No la busques en otro lado.
- Lee la actividad: si te doy la semana, búscala en la planificación. Anota su producto (qué
  entrega el grupo y en qué partes), las diapositivas que cita y los resultados de aprendizaje
  del sílabo que trabaja. Abre esas diapositivas: los niveles se apoyan en lo que dicen.
- Muéstrame en pocas líneas qué entendiste: producto, partes, diapositivas y resultados.

## Paso 2 — Enunciado y rúbrica (aquí te detienes)

Propón:

- El enunciado de la tarea: qué se entrega (un archivo PDF por grupo, con los nombres de los
  integrantes; lo sube uno de ellos), las partes del producto y las fechas (abre hoy y vence
  en 7 días).
- La rúbrica, en una tabla de criterios × niveles:
  - un criterio por cada parte del producto y uno por el uso de la evidencia (salidas,
    capturas o cálculos propios y las diapositivas que lo respaldan), hasta el número pedido;
  - cada nivel describe algo que se puede ver en la entrega. Prohibido «bueno», «regular»,
    «adecuado», «suficiente», «excelente» o «deficiente» sin decir qué se ve;
  - los niveles son acumulativos: cada uno incluye todo lo del anterior y añade algo. Comprueba que
    cualquier entrega posible cabe en algún nivel;
  - el nivel más bajo de cada criterio vale 0, y la suma de los niveles más altos es
    exactamente el puntaje. Reparte los puntos según el peso de cada parte y dilo.

Detente y espera mi revisión. Si cambio algo, rehace la tabla antes de seguir.

## Paso 3 — Crea la tarea y carga la rúbrica

Con mi visto bueno:

- Crea la tarea en la sección pedida, justo después de la actividad o del momento que te
  indiqué: entrega en archivo (PDF, un archivo), comentarios de retroalimentación activados,
  método de calificación = rúbrica, nota máxima = puntaje, sin flujo de calificación, sin
  notificaciones. Abre hoy a las 00:00 y vence en 7 días a las 23:59 (la fecha límite para calificar, una semana después del vencimiento: Moodle la exige posterior); si la semana de la
  actividad ya pasó, igual usa esas fechas: el estudiante tiene que poder entregar.
- Carga la rúbrica y déjala preparada («Guardar rúbrica y dejarla preparada»).
- Abre la tarea como profesor y comprueba que la rúbrica aparece con todos sus criterios y
  niveles.
- Termina el paso con el enlace a la tarea y: «Avísame cuando haya entregas».

## Paso 4 — Propón la calificación (aquí te detienes otra vez)

Cuando te lo pida:

- Busca las entregas enviadas, descarga cada archivo y léelo con `pdftotext`.
- Si una entrega no es PDF o no tiene texto legible (por ejemplo, una foto escaneada: `pdftotext` devuelve casi nada), no
  propongas niveles para ella: dímelo y sigue con las demás. Nunca adivines.
- Por cada entrega, una tabla: criterio · nivel propuesto · puntos · cita textual de la
  entrega que lo justifica · una línea de por qué. Si la entrega trae cifras o resultados que puedes recalcular, recalcúlalos antes de proponer el nivel. Si el nivel depende de algo que falta, dilo
  («no menciona ninguna diapositiva»). Si la entrega no cabe en ningún nivel (cumple algo de uno superior pero
  le falta algo de uno inferior), no elijas uno: dímelo, porque la rúbrica tiene un error.
- Redacta un comentario de retroalimentación de 2 a 4 frases: primero lo que el grupo logró,
  luego qué mejorar y cómo. Sin la nota.
- Muestra la nota que saldría (suma de puntos).

Detente: la nota la decido yo. Puedo confirmar o cambiar niveles y el comentario.

## Paso 5 — Guarda y muestra el resultado

- Guarda la calificación con mis cambios (niveles, comentario y, solo si las aprobé, observaciones
  por criterio), sin notificar al estudiante. El calificador de Moodle guarda con el servicio
  `mod_assign_submit_grading_form` y el formulario del panel de calificación.
- Relee la nota en Moodle y comprueba que es la suma de los niveles elegidos.
- Muestra la tabla final (criterio · nivel · puntos), la nota y el enlace a la entrega para
  verla como la verá el estudiante.

Trabaja paso a paso y verifica cada uno contra la fuente real (la planificación, la entrega, el
aula) antes de seguir. Solo te detienes en los pasos 2 y 4, si falta algo que no puedes resolver
o si la cookie caducó.
```

## Campos a rellenar

| Campo | Ejemplo |
|---|---|
| `[Actividad]` | el trabajo aplicado de la semana 3 de mi planificación |
| `[Sección de Moodle]` | Unidad 300, semana 3, momento «En clase» |
| `[Criterios]` | 5 |
| `[Niveles]` | 4 |
| `[Puntaje]` | 10 |

## Variaciones

- **Solo la rúbrica:** pide «detente después del paso 3»; calificas tú en Moodle con ella.
- **Entrega individual:** cambia «un archivo PDF por grupo» por «un archivo por estudiante».
- **Otros formatos:** si las entregas son `.docx`, pide que las convierta a texto antes de leerlas
  (por ejemplo, con `pandoc`).
- **Varias entregas:** en el paso 4, pide la tabla de todas las entregas juntas para comparar
  niveles entre grupos antes de decidir.

## Consejos

- Revisa cada nivel de la rúbrica preguntándote «¿lo puedo señalar en la hoja?». Si no, pide
  que lo reescriba: un nivel observable es lo que permite a Claude citar la evidencia.
- En el paso 4, lee la cita de cada criterio: si la cita no justifica el nivel, cámbialo.
- Si después de calificar notas que una entrega no cabía en ningún nivel, el error es de la rúbrica: pide
  que corrija ese criterio en Moodle (sin crear otra rúbrica) y que revise las notas ya guardadas.
- Prueba la tarea con una cuenta de estudiante antes de abrirla al grupo: debe poder subir el
  archivo y, después de calificar, ver los niveles, el comentario y la nota.
