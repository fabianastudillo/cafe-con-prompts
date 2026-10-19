# Aula invertida en Moodle con Claude Code

> 🎬 **Video:** próximamente · ☕ **Categoría:** Planificación · 🛠️ **Requiere:** [Claude Code](https://claude.com/claude-code)

## Para qué sirve

Lleva una materia completa a **aprendizaje invertido** y la **publica en tu aula de Moodle**:
planificación semana a semana, guías de estudio, banco de preguntas, cuestionarios de entrada,
tareas con rúbrica a partir del manual de prácticas, etiquetas de antes / en clase / laboratorio /
cierre con fechas reales y la imagen del curso.

No es un prompt para el chat: se ejecuta en una sesión de **Claude Code**, que lee tus archivos,
escribe scripts y trabaja sobre tu aula de Moodle.

Está escrito a partir de dos materias reales de la Universidad de Cuenca (Redes Inalámbricas y
Seguridad en Redes) en Moodle 5.0.

## Antes de empezar

### 1. Crea la carpeta `insumos/`

En una carpeta para la materia (por ejemplo `SeguridadEnRedes/`), crea `insumos/` y deja ahí todo
el material. Los nombres de archivo dan igual: Claude identifica cada uno por su contenido.

| Insumo | ¿Obligatorio? |
|---|---|
| Sílabo oficial (unidades, horas, resultados de aprendizaje) | Sí |
| Presentaciones del curso (PPTX, PDF, HTML, Beamer…) | Sí |
| Calendario académico del período | Sí |
| Horario de la materia (día, hora y duración de cada clase) | Sí |
| Archivo `credenciales` (ver abajo) | Sí |
| Manual de prácticas (LaTeX o PDF) | Para crear las tareas con rúbrica |
| Libro de marca institucional (colores, tipografías, logotipo) | Para la identidad visual |
| Documentación ABET | Solo si la carrera está acreditada |

### 2. Configura el archivo `credenciales`

Crea `insumos/credenciales`, un archivo de texto con tres líneas:

```text
urlmoodle: https://TU-MOODLE/course/view.php?id=NUMERO-DEL-CURSO
email: tu.correo@institucion.edu
cookie: VALOR-DE-LA-COOKIE-MoodleSession
```

- **`urlmoodle`**: abre tu curso en Moodle y copia la dirección de la barra del navegador.
- **`cookie`**: con la sesión iniciada en Moodle, abre las herramientas de desarrollador del
  navegador (F12) → *Aplicación* (o *Almacenamiento*) → *Cookies* → tu sitio de Moodle, y copia el
  **valor** de `MoodleSession`. Funciona aunque tu institución use inicio de sesión único
  (Microsoft, Google), donde no hay usuario y contraseña de Moodle.

> ⚠️ **La cookie da acceso a tu cuenta de Moodle mientras la sesión esté abierta.** No la
> compartas, no la subas a GitHub y no la muestres en pantalla. Si la expones por error, cierra la
> sesión en Moodle y la cookie deja de valer. Caduca sola: si Claude te avisa de que Moodle
> redirige al inicio de sesión, copia una nueva.

### 3. Usa un curso vacío

Haz la primera prueba en un curso **vacío o de prueba**, no en uno con estudiantes y entregas.

### 4. Ejecuta el prompt

Abre una terminal en la carpeta de la materia (la que contiene `insumos/`), inicia `claude`, y
pega el prompt. **No hay nada que rellenar.** El proceso es largo: Claude trabaja fase por fase,
verifica cada una contra el aula y solo se detiene para preguntarte si hay ambigüedades.

## Prompt

```text
Vas a llevar una materia a aprendizaje invertido y a publicarla en Moodle. Todo el
material está en `insumos/`. Trabaja fase por fase: no pases a la siguiente sin haber
verificado la anterior contra la fuente real (el PDF, el sitio, el aula virtual).

## Fase 0 — Inventario de `insumos/`

Lo primero, antes de abrir nada: si esta carpeta es un repositorio git, comprueba que
`insumos/` esté en `.gitignore` y, si no lo está, añádelo. Ahí viven credenciales.
Nunca imprimas, copies ni hagas commit del contenido del archivo `credenciales`.

Recorre `insumos/` (incluidas subcarpetas y archivos comprimidos) e identifica cada
archivo por su contenido, no por su nombre. Busca esto:

| Insumo | Cómo reconocerlo | Fases que lo necesitan |
|---|---|---|
| Sílabo oficial | Unidades, horas ACD/APE/AA, resultados de aprendizaje, código de la materia | Todas |
| Presentaciones del curso | Diapositivas por tema (PDF, PPTX, HTML, LaTeX/Beamer…) | 1, 2, 3 |
| Calendario académico del período | Fechas de inicio y fin, feriados, receso, exámenes | 1, 6 |
| Horario docente | Día, hora y duración de cada franja de la materia (puede venir dentro del sílabo o en un archivo aparte) | 1, 3, 6 |
| Manual de prácticas | `.tex` con capítulos de prácticas, o el PDF del manual | 5 |
| Credenciales de Moodle | Archivo `credenciales` con tres claves: `urlmoodle:` (URL del curso, `…/course/view.php?id=N`: de ahí salen el sitio y el ID), `email:` y `cookie:` (valor de `MoodleSession`) | 4–7 |
| Libro de marca institucional | Manual de identidad visual: colores, tipografías, logotipo y sus usos | 2, 5, 6, 7 |
| Documentación ABET (opcional) | Student Outcomes, indicadores, descriptores de niveles | 8 |

Del sílabo saca el nombre y el código de la materia. Usa una carpeta `<curso>/`
(el código en minúsculas) para todo lo que generes; en adelante, `<curso>/` se refiere
a ella.

Antes de pasar a la fase 1, muéstrame el inventario: insumo → archivo detectado →
estado (encontrado / falta / ambiguo). Después:

- Si falta algún insumo, avísame en el chat en ese momento, con un mensaje aparte del
  inventario. Para cada uno que falte, di:
  - qué insumo es,
  - qué fases quedan bloqueadas por su ausencia,
  - qué archivo debo añadir a `insumos/`.

  Luego continúa con las fases que no dependen de él. Sin manual de prácticas no hay
  fase 5; sin documentación ABET o si la carrera no está acreditada, la fase 8 se omite.
  No inventes el insumo que falta. Si lo añado más tarde, vuelve a la fase que quedó
  pendiente.
- Si hay ambigüedad (dos calendarios, dos versiones del sílabo), pregúntame cuál vale
  antes de seguir; es lo único por lo que debes detenerte.
- Si la cookie ha caducado (Moodle redirige al login), avísame cuando llegues a la
  fase 4 para que la renueve; no la busques en otro lado.

El sílabo no se toca. Se respetan las unidades, el reparto de horas y los entregables
declarados. El aula invertida cambia cuándo ocurre cada cosa, no qué se enseña. Si al
leerlo encuentras una inconsistencia (por ejemplo, que las horas por unidad no sumen el
total declarado), no la corrijas en silencio: documéntala como aviso en la
planificación y sigue.

## Fase 1 — Planificación semana a semana

Cruza el calendario académico con el horario y produce el calendario real del período:
semanas lectivas, feriados, receso y ventanas de exámenes. Las semanas que pierden una
franja por feriado o por fin de período no se ignoran: cada una necesita una línea que
diga cómo se resuelve (trabajo autónomo guiado, recuperación en la semana siguiente,
etc.).

Escribe `<curso>/planificacion-aprendizaje-invertido.md` con, para cada semana:

- ANTES (AA) — qué presentación se estudia y qué cuestionario se responde.
- DURANTE (ACD) — bloques con minutos: puesta a punto · instrucción entre pares con
  ConcepTests (escribe las preguntas, son la materia prima de la fase 6) · trabajo
  aplicado · puesta en común · cierre.
- DESPUÉS (APE) — la práctica de laboratorio de esa semana.

Añade el mapa del semestre, el calendario en tabla, las rúbricas breves y la propuesta
de distribución de evidencias (sin fijar porcentajes si el sílabo no los fija).

## Fase 2 — Guías de estudio

Una por unidad, en `<curso>/guias/`. Cada guía dice qué mirar en cada presentación, qué
conceptos deben quedar firmes, qué preguntas traer a clase y cuánto tiempo dedicar.
Conviértelas a HTML para pegarlas en Moodle, con la identidad del libro de marca de
`insumos/`: sus colores, sus tipografías y su uso del logotipo, sin aproximaciones.
Usa esa misma identidad en todo HTML que publiques (enunciados, rúbricas, etiquetas).
Moodle conserva los estilos en línea pero descarta los bloques `<style>`, así que todo
el CSS va inline.

## Fase 3 — Banco de preguntas

Extrae las autoevaluaciones que ya viven dentro de las presentaciones. Cuenta cuántas
salen y compáralo con las que hay: si el extractor solo reconoce un formato de marcado,
se perderán en silencio las de los demás. Escribe a mano, en un JSON aparte, las
preguntas de las presentaciones que no traían ninguna.

Emite un `.gift` por sesión con su `$CATEGORY:`. Cada cuestionario de entrada: 10
preguntas al azar del banco de su unidad, 2 intentos, cuenta el mejor, una pregunta por
página, retroalimentación inmediata, abre el viernes anterior a las 18:00 y cierra dos
horas antes de la clase. Pesa poco en la nota: diagnostica, no sanciona.

## Fase 4 — Publicación en Moodle

La definición del aula vive en un YAML por curso y se aplica con una orden idempotente:
crea lo que falta, actualiza lo que cambió y no toca lo demás. Escribe tú los scripts
que hagan falta en `scripts/`. El sitio y el ID del curso salen de `urlmoodle` en el
archivo de credenciales. Estas son las trampas que cuestan horas si no se saben
(Moodle 5.0.x):

- Autenticación: si el sitio usa ADFS/OIDC no hay login por usuario y contraseña. Usa
  la cookie `MoodleSession` del archivo `credenciales`, leyéndola en tiempo de
  ejecución. Nunca imprimas su valor.
- Formularios: lee primero el formulario y conserva sus campos ocultos (`sesskey`,
  `_qf__*`, `availabilityconditionsjson`, `completionunlocked`). Descarta todos los
  inputs de tipo `submit`, `button`, `reset` e `image`: si envías uno de los botones
  "no-submit" (del estilo «Añadir 3 campos más de retroalimentación»), Moodle recarga el
  formulario y descarta el guardado sin devolver ningún error.
- La cabecera `Referer` debe apuntar al propio formulario.
- `assignsubmission_file_filetypes` vacío devuelve HTTP 404: envíalo solo si tiene
  valor.
- `gradingduedate` no puede ser anterior a `duedate`.
- Actualizar un recurso de archivo añade el nuevo junto al viejo: vacía el área de
  borrador (`draftfiles_ajax.php?action=delete`) antes de subir.
- El banco de preguntas ya no cuelga del curso: es una actividad `qbank`. Créalo o
  localízalo en `/question/banks.php?courseid=<id>&createdefault=1` e importa el GIFT en
  `/question/bank/importquestions/import.php?cmid=<cmid del banco>`.
- Las preguntas al azar de un cuestionario se añaden por AJAX
  (`mod_quiz_add_random_questions`).
- Editar una pregunta crea una versión nueva: vuelve a leer los identificadores después
  de editar, o verificarás sobre los viejos.
- Si algo falla, lee el HTML de respuesta: Moodle devuelve 200 con el error dentro.

Verifica siempre contra el aula: cuenta actividades, preguntas por categoría y
preguntas efectivas dentro de cada cuestionario.

## Fase 5 — Prácticas y rúbricas desde el manual

El manual es la fuente; no escribas enunciados a mano en Moodle. Genera, por práctica,
el enunciado HTML de la tarea y su rúbrica de calificación.

- Si el manual está en LaTeX, parsea el `.tex`. Notas: usa `xltabular` para tablas
  largas con columna `X` (`longtable` no la admite y falla con Extra alignment tab), y
  deja el material final —bibliografía, `\backmatter`— justo antes de `\end{document}`
  o los capítulos dejarán de numerarse.
- Si el manual está en PDF, extrae el texto conservando la estructura de cada práctica
  y comprueba que no se pierdan tablas ni pasos numerados.

De cada práctica salen: cabecera, objetivos, materiales, pasos del procedimiento,
entregables y rúbrica.

Revisa antes que el manual cubra todas las unidades del sílabo y que la unidad declarada
en cada práctica coincida con la del sílabo: es el error más común al heredar un manual
de un período anterior. Si falta una unidad, avísame y propón el capítulo con la misma
estructura y el mismo tono que los demás. Trabaja sobre una copia en `<curso>/`; no
modifiques el original de `insumos/`.

La rúbrica se carga como calificación avanzada de la tarea (`/grade/grading/manage.php`
→ `setmethod=rubric` → `/grade/grading/form/rubric/edit.php?areaid=N`), sobre 100
puntos, que Moodle escala a la nota.

## Fase 6 — Etiquetas: los cuatro momentos de cada unidad

Dentro de una sección, las actividades salen en una lista plana y el estudiante no sabe
si una presentación se mira antes o después de clase. Divide cada unidad con etiquetas
que respondan tres preguntas: ¿cuándo lo hago?, ¿cuánto me toma?, ¿qué tengo que poder
hacer al terminar?

| Etiqueta | Componente | Qué agrupa debajo |
|---|---|---|
| ① ANTES · Llego sabiendo de qué hablamos | AA | Guía de estudio, presentaciones, cuestionario de entrada |
| ② EN CLASE · Resolvemos juntos lo difícil | ACD | Guía de la unidad |
| ③ EN EL LABORATORIO · Lo hago con mis manos | APE | Tareas de entrega con rúbrica |
| ④ ¿YA PUEDO…? | cierre | Nada: las preguntas van dentro de la etiqueta |

Cada etiqueta lleva la fecha real de esa semana, calculada del calendario y del
horario, no una fórmula genérica. Las preguntas del bloque ④ son los ConcepTests que
escribiste en la fase 1, dos por semana. Añade una etiqueta «Empieza aquí» en la
bienvenida y una por parcial al cierre de su última unidad.

El orden de los momentos fuerza el orden de las actividades: las presentaciones suben
al ① y las entregas bajan al ③.

- Las etiquetas no tienen nombre propio: Moodle lo recorta de los primeros ~50
  caracteres del contenido sin etiquetas HTML al crearlas, y no lo recalcula al
  editarlas. Deja un espacio entre bloques desde el principio (si no, el título se pega
  a la línea siguiente) e identifícalas por un prefijo estable, no por el nombre.
- Reordenar dentro de una sección se hace por AJAX con
  `core_courseformat_update_course`, `action: cm_move`, `targetsectionid` y
  `targetcmid`, que inserta antes del objetivo. Para imponer un orden completo, recorre
  la lista al revés: la última al final y cada anterior delante de la ya colocada.

## Fase 7 — La imagen del curso

El aula llega con el marcador de posición del tema, que no dice nada de la materia.
Genera una imagen de cabecera propia con la misma identidad que ya usan las páginas
publicadas dentro: el mismo degradado de fondo, la misma franja de acento y la misma
paleta, tomadas del libro de marca. Si el aula y su banner no se parecen, el banner
parece pegado.

- Sin título. El tema superpone el nombre del curso sobre la imagen; cualquier texto
  grande choca con él. Deja como mucho una línea institucional pequeña en una esquina
  libre.
- El motivo, hacia el centro. La tarjeta de «Mis cursos» recorta por los lados: un
  motivo pegado al borde desaparece ahí. 1600×500 funciona bien si lo importante cae en
  la franja central.
- Que hable de la materia, no de «tecnología» en abstracto: una torre radiando con el
  espectro al lado dice redes inalámbricas; un grafo de nodos, un perímetro y una
  intrusión detenida en él dice seguridad en redes.
- Dibújalo como SVG por programa y conviértelo a PNG, con la semilla de cualquier
  posición aleatoria fija para que el resultado sea reproducible.
- Cuidado con el contraste sobre fondo oscuro: un rojo profundo casi no se ve sobre azul
  marino. Úsalo en la franja de acento y resuelve los elementos destacados con los tonos
  claros de la paleta.

Se sube en los ajustes del curso (`/course/edit.php?id=<id>`), campo
`overviewfiles_filemanager`, con el botón `saveanddisplay`. Tiene la misma trampa que
los recursos de archivo: vacía el área de borrador antes de subir.

## Fase 8 — Medición ABET

Solo si la carrera está acreditada y hay documentación ABET en `insumos/`. Averigua en
ella qué Student Outcome mide esta materia, sus indicadores de desempeño y los
descriptores oficiales de los cinco niveles (0-59,99 / 60-69,99 / 70-79,99 /
80-89,99 / 90-100).

- La actividad de medición debe pesar al menos el 30 % de la valoración.
- Cada indicador se implementa como criterio propio de la rúbrica de Moodle, con sus
  cinco niveles. Así el libro de calificaciones ya contiene los resultados por
  estudiante y por indicador.
- Redacta los cinco niveles con el escenario concreto de esa práctica, no con el
  descriptor genérico.
- Plantea las preguntas sobre un dilema realista de la materia, y pide de forma
  explícita el marco legal aplicable y el análisis de impacto económico, social y
  ambiental con contramedidas fundamentadas en estándares.

## Ortografía, al final

Revisa con un diccionario español (hunspell es_EC/es_ES) y usa uno de inglés para
filtrar los falsos positivos técnicos. Nunca apliques una corrección automática dentro
de una URL, un identificador de archivo, una entidad HTML (`&iacute;`) o un escape
(`ó`), ni sobre nombres propios. Comprueba al terminar que no desapareció ninguna
palabra acentuada.

## Cómo quiero que trabajes

- Verifica cada fase contra la fuente real y dime los números: actividades creadas,
  preguntas importadas, páginas del PDF, errores de compilación.
- Si algo no se puede hacer, dilo y termina todo lo demás; no lo escondas en un resumen
  optimista.
- Al cerrar, vuelve a listarme en el chat los insumos que sigan faltando y las fases
  que quedaron pendientes por ellos.
- Antes de borrar o reemplazar algo en el aula, comprueba que no tenga entregas de
  estudiantes y dímelo.
- `insumos/` es de solo lectura: todo lo que generes va a `<curso>/` o a `scripts/`.
- Todo lo generado sale de una fuente del repositorio (sílabo, planificación, manual,
  presentaciones). No inventes contenido de la materia.
```

## Qué esperar

- **Primero, el inventario:** Claude te muestra qué encontró en `insumos/` y te avisa de lo que
  falta y qué fases se bloquean. Es un buen momento para corregir antes de seguir.
- **Después, fase por fase:** con los números de cada verificación (actividades creadas, preguntas
  importadas…). Puede tardar varias horas en una materia completa.
- **Al final:** un aula con las unidades divididas en ① antes, ② en clase, ③ laboratorio y
  ④ cierre, con fechas reales, cuestionarios de entrada, tareas con rúbrica e imagen del curso.

## Consejos

- Revisa la planificación (`<curso>/planificacion-aprendizaje-invertido.md`) antes de dar el aula
  por buena: es la base de todo lo demás.
- Si no tienes manual de prácticas o libro de marca, ejecútalo igual: Claude hace el resto y te
  dice qué faltó.
- Puedes volver a ejecutarlo: la publicación es idempotente y solo cambia lo que cambió.
