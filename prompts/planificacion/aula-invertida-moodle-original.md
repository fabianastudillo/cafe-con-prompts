# Prompt: llevar una materia a aula invertida y publicarla en Moodle

**Antes de empezar:** deja todo el material de la materia en una carpeta `insumos/` en la
raíz del repositorio del curso (los nombres de archivo dan igual). Luego copia todo lo
que va debajo de la línea en una sesión de Claude Code abierta en ese repositorio.
**No hay nada que rellenar.**

Está escrito a partir de dos materias reales (Redes Inalámbricas y Seguridad en Redes,
Universidad de Cuenca, Moodle 5.0.2); la implementación de referencia vive en
`scripts/moodle/` y `scripts/cuestionarios/` de este mismo repositorio.

---

Vas a llevar una materia a aprendizaje invertido y a publicarla en Moodle. Todo el
material está en `insumos/`. Trabaja fase por fase: **no pases a la siguiente sin haber
verificado la anterior contra la fuente real** (el PDF, el sitio, el aula virtual).

## Fase 0 — Inventario de `insumos/`

**Lo primero, antes de abrir nada:** comprueba que `insumos/` esté en `.gitignore`; si
no lo está, añádelo. Ahí viven credenciales. Nunca imprimas, copies ni hagas *commit*
del contenido del archivo de la cookie.

Recorre `insumos/` (incluidas subcarpetas y archivos comprimidos) e **identifica cada
archivo por su contenido, no por su nombre**. Busca esto:

| Insumo | Cómo reconocerlo | Fases que lo necesitan |
|---|---|---|
| Sílabo oficial | Unidades, horas ACD/APE/AA, resultados de aprendizaje, código de la materia | Todas |
| Presentaciones del curso | Diapositivas por tema (PDF, PPTX, HTML, LaTeX/Beamer…) | 1, 2, 3 |
| Calendario académico del período | Fechas de inicio y fin, feriados, receso, exámenes | 1, 6 |
| Horario docente | Día, hora y duración de cada franja de la materia | 1, 3, 6 |
| Manual de prácticas en LaTeX | `.tex` con capítulos de prácticas | 5 |
| Datos del aula en Moodle | Un texto con la URL del curso (`…/course/view.php?id=N`): de ahí salen el sitio y el ID | 4–7 |
| Cookie de Moodle | Archivo con el valor de `MoodleSession` | 4–7 |
| Libro de marca institucional | Manual de identidad visual: colores, tipografías, logotipo y sus usos | 2, 5, 6, 7 |
| Documentación ABET (opcional) | Student Outcomes, indicadores, descriptores de niveles | 8 |

Del sílabo saca el **nombre y el código de la materia**. Usa una carpeta `<curso>/`
(el código en minúsculas) para todo lo que generes; en adelante, `<curso>/` se refiere
a ella.

Antes de pasar a la fase 1, **muéstrame el inventario**: insumo → archivo detectado →
estado (encontrado / falta / ambiguo). Después:

- Si **falta** algún insumo, **avísame en el chat en ese momento**, con un mensaje
  aparte del inventario. Para cada uno que falte, di:
  - qué insumo es,
  - qué fases quedan bloqueadas por su ausencia,
  - qué archivo debo añadir a `insumos/`.

  Luego continúa con las fases que no dependen de él. Sin manual LaTeX no hay fase 5;
  sin documentación ABET o si la carrera no está acreditada, la fase 8 se omite. No
  inventes el insumo que falta. Si lo añado más tarde, vuelve a la fase que quedó
  pendiente.
- Si hay **ambigüedad** (dos calendarios, dos versiones del sílabo), pregúntame cuál
  vale antes de seguir; es lo único por lo que debes detenerte.
- Si la cookie existe pero ha caducado (Moodle redirige al login), avísame cuando
  llegues a la fase 4 para que la renueve; no la busques en otro lado.

**El sílabo no se toca.** Se respetan las unidades, el reparto de horas y los
entregables declarados. El aula invertida cambia *cuándo* ocurre cada cosa, no *qué*
se enseña. Si al leerlo encuentras una inconsistencia (por ejemplo, que las horas por
unidad no sumen el total declarado), **no la corrijas en silencio**: documéntala como
aviso en la planificación y sigue.

## Fase 1 — Planificación semana a semana

Cruza el calendario académico con el horario y produce el calendario real del período:
semanas lectivas, feriados, receso y ventanas de exámenes. **Las semanas que pierden
una franja por feriado o por fin de período no se ignoran**: cada una necesita una
línea que diga cómo se resuelve (trabajo autónomo guiado, recuperación en la semana
siguiente, etc.).

Escribe `<curso>/planificacion-aprendizaje-invertido.md` con, para cada semana:

- **ANTES (AA)** — qué presentación se estudia y qué cuestionario se responde.
- **DURANTE (ACD)** — bloques con minutos: puesta a punto · instrucción entre pares
  con **ConcepTests** (escribe las preguntas, son la materia prima de la fase 6) ·
  trabajo aplicado · puesta en común · cierre.
- **DESPUÉS (APE)** — la práctica de laboratorio de esa semana.

Añade el mapa del semestre, el calendario en tabla, las rúbricas breves y la propuesta
de distribución de evidencias (sin fijar porcentajes si el sílabo no los fija).

## Fase 2 — Guías de estudio

Una por unidad, en `<curso>/guias/`. Cada guía dice qué mirar en cada presentación, qué
conceptos deben quedar firmes, qué preguntas traer a clase y cuánto tiempo dedicar.
Conviértelas a HTML para pegarlas en Moodle, con la identidad del libro de marca de
`insumos/`: sus colores, sus tipografías y su uso del logotipo, sin aproximaciones.
Usa esa misma identidad en todo HTML que publiques (enunciados, rúbricas, etiquetas).
**Moodle
conserva los estilos en línea pero descarta los bloques `<style>`**, así que todo el
CSS va inline.

## Fase 3 — Banco de preguntas

Extrae las autoevaluaciones que ya viven dentro de las presentaciones. Cuenta cuántas
salen y **compáralo con las que hay**: si el extractor solo reconoce un formato de
marcado, se perderán en silencio las de los demás. Escribe a mano, en un JSON aparte,
las preguntas de las presentaciones que no traían ninguna.

Emite un `.gift` por sesión con su `$CATEGORY:`. Cada cuestionario de entrada:
**10 preguntas al azar** del banco de su unidad, **2 intentos**, cuenta el mejor, una
pregunta por página, retroalimentación inmediata, **abre el viernes anterior a las
18:00 y cierra dos horas antes de la clase**. Pesa poco en la nota: diagnostica, no
sanciona.

## Fase 4 — Publicación en Moodle

La definición del aula vive en un YAML por curso y se aplica con una orden idempotente:
crea lo que falta, actualiza lo que cambió y no toca lo demás. El sitio y el ID del
curso salen del inventario de la fase 0. Estas son las trampas que cuestan horas si no
se saben (Moodle 5.0.x):

- **Autenticación**: si el sitio usa ADFS/OIDC no hay login por usuario y contraseña.
  Usa la cookie `MoodleSession` del archivo identificado en `insumos/`, leyéndola en
  tiempo de ejecución. **Nunca imprimas su valor.**
- **Formularios**: lee primero el formulario y conserva sus campos ocultos (`sesskey`,
  `_qf__*`, `availabilityconditionsjson`, `completionunlocked`). **Descarta todos los
  inputs de tipo `submit`, `button`, `reset` e `image`**: si envías uno de los botones
  "no-submit" (del estilo «Añadir 3 campos más de retroalimentación»), Moodle recarga
  el formulario y **descarta el guardado sin devolver ningún error**.
- La cabecera `Referer` debe apuntar al propio formulario.
- `assignsubmission_file_filetypes` vacío devuelve **HTTP 404**: envíalo solo si tiene
  valor.
- `gradingduedate` no puede ser anterior a `duedate`.
- Actualizar un recurso de archivo **añade** el nuevo junto al viejo: vacía el área de
  borrador (`draftfiles_ajax.php?action=delete`) antes de subir.
- **El banco de preguntas ya no cuelga del curso**: es una actividad `qbank`. Créalo o
  localízalo en `/question/banks.php?courseid=<id>&createdefault=1` e importa el GIFT
  en `/question/bank/importquestions/import.php?cmid=<cmid del banco>`.
- Las preguntas al azar de un cuestionario se añaden por AJAX
  (`mod_quiz_add_random_questions`).
- **Editar una pregunta crea una versión nueva**: vuelve a leer los identificadores
  después de editar, o verificarás sobre los viejos.
- Si algo falla, **lee el HTML de respuesta**: Moodle devuelve 200 con el error dentro.

Verifica siempre contra el aula: cuenta actividades, preguntas por categoría y
preguntas efectivas dentro de cada cuestionario.

## Fase 5 — Prácticas y rúbricas desde el manual

**El manual es la fuente; no escribas enunciados a mano en Moodle.** Parsea su LaTeX y
genera, por práctica, el enunciado HTML de la tarea y su rúbrica de calificación.

Revisa antes que el manual cubra **todas** las unidades del sílabo y que la unidad
declarada en cada práctica coincida con la del sílabo: es el error más común al heredar
un manual de un período anterior. Si falta una unidad, escribe el capítulo con la misma
estructura y el mismo tono que los demás. Trabaja sobre una copia del manual en
`<curso>/`; no modifiques el original de `insumos/`.

Del LaTeX salen bien: cabecera, objetivos, materiales, pasos del procedimiento,
entregables y rúbrica. Notas de LaTeX: usa `xltabular` para tablas largas con columna
`X` (`longtable` no la admite y falla con *Extra alignment tab*), y deja el material
final —bibliografía, `\backmatter`— justo antes de `\end{document}` o los capítulos
dejarán de numerarse.

La rúbrica se carga como **calificación avanzada** de la tarea
(`/grade/grading/manage.php` → `setmethod=rubric` →
`/grade/grading/form/rubric/edit.php?areaid=N`), sobre 100 puntos, que Moodle escala a
la nota.

## Fase 6 — Etiquetas: los cuatro momentos de cada unidad

Dentro de una sección, las actividades salen en una lista plana y el estudiante no sabe
si una presentación se mira antes o después de clase. Divide cada unidad con etiquetas
que respondan tres preguntas: **¿cuándo lo hago?, ¿cuánto me toma?, ¿qué tengo que
poder hacer al terminar?**

| Etiqueta | Componente | Qué agrupa debajo |
|---|---|---|
| ① ANTES · Llego sabiendo de qué hablamos | AA | Guía de estudio, presentaciones, cuestionario de entrada |
| ② EN CLASE · Resolvemos juntos lo difícil | ACD | Guía de la unidad |
| ③ EN EL LABORATORIO · Lo hago con mis manos | APE | Tareas de entrega con rúbrica |
| ④ ¿YA PUEDO…? | cierre | Nada: las preguntas van dentro de la etiqueta |

Cada etiqueta lleva **la fecha real de esa semana**, calculada del calendario y del
horario, no una fórmula genérica. Las preguntas del bloque ④ son los ConcepTests que
escribiste en la fase 1, dos por semana. Añade una etiqueta «Empieza aquí» en la
bienvenida y una por parcial al cierre de su última unidad.

El orden de los momentos **fuerza el orden de las actividades**: las presentaciones
suben al ① y las entregas bajan al ③.

- **Las etiquetas no tienen nombre propio**: Moodle lo recorta de los primeros ~50
  caracteres del contenido sin etiquetas HTML **al crearlas**, y no lo recalcula al
  editarlas. Deja un espacio entre bloques desde el principio (si no, el título se pega
  a la línea siguiente) e identifícalas por un prefijo estable, no por el nombre.
- **Reordenar dentro de una sección** se hace por AJAX con
  `core_courseformat_update_course`, `action: cm_move`, `targetsectionid` y
  `targetcmid`, que inserta **antes** del objetivo. Para imponer un orden completo,
  recorre la lista al revés: la última al final y cada anterior delante de la ya
  colocada.

## Fase 7 — La imagen del curso

El aula llega con el marcador de posición del tema, que no dice nada de la materia.
Genera una imagen de cabecera propia **con la misma identidad que ya usan las páginas
publicadas dentro**: el mismo degradado de fondo, la misma franja de acento y la misma
paleta, tomadas del libro de marca. Si el aula y su banner no se parecen, el banner
parece pegado.

- **Sin título.** El tema superpone el nombre del curso sobre la imagen; cualquier
  texto grande choca con él. Deja como mucho una línea institucional pequeña en una
  esquina libre, con el mismo tratamiento en versalitas espaciadas de las cabeceras.
- **El motivo, hacia el centro.** El banner se ve apaisado, pero la tarjeta de «Mis
  cursos» recorta por los lados: un motivo pegado al borde desaparece ahí. 1600×500
  funciona bien si lo importante cae en la franja central.
- **Que hable de la materia**, no de «tecnología» en abstracto: una torre radiando con
  el espectro al lado dice *redes inalámbricas*; un grafo de nodos, un perímetro y una
  intrusión detenida en él dice *seguridad en redes*. Un diagrama genérico no dice nada.
- Dibújalo como SVG por programa y conviértelo a PNG: se regenera con un comando cuando
  cambie la paleta, y no dependes de un editor. Fija la semilla de cualquier posición
  aleatoria para que el resultado sea reproducible.
- Cuidado con el contraste sobre fondo oscuro: un rojo institucional profundo casi no
  se ve sobre azul marino. Úsalo en la franja de acento —donde va junto a un color
  claro— y resuelve los elementos destacados del dibujo con el tono claro y el dorado
  de la paleta.

Se sube en los ajustes del curso (`/course/edit.php?id=<id>`), campo
`overviewfiles_filemanager`, con el botón `saveanddisplay`. **Tiene la misma trampa que
los recursos de archivo**: el gestor *añade* la imagen nueva junto a la anterior en vez
de sustituirla, así que vacía el área de borrador antes de subir.

## Fase 8 — Medición ABET

Solo si la carrera está acreditada y hay documentación ABET en `insumos/`. Averigua en
ella **qué Student Outcome mide esta materia**, sus indicadores de desempeño y los
descriptores oficiales de los cinco niveles (0-59,99 / 60-69,99 / 70-79,99 /
80-89,99 / 90-100).

- La actividad de medición debe pesar **al menos el 30 %** de la valoración.
- Cada indicador se implementa como **criterio propio de la rúbrica de Moodle**, con
  sus cinco niveles. Así el libro de calificaciones ya contiene los resultados por
  estudiante y por indicador, que es exactamente lo que pide la comisión.
- Redacta los cinco niveles con **el escenario concreto de esa práctica**, no con el
  descriptor genérico: se califica mejor y el estudiante entiende qué se le pide.
- Plantea las preguntas sobre un dilema realista de la materia, y pide de forma
  explícita el marco legal aplicable y el análisis de impacto económico, social y
  ambiental con contramedidas fundamentadas en estándares.

## Ortografía, al final

Revisa con un diccionario español (hunspell `es_EC`/`es_ES`) y usa uno de inglés para
filtrar los falsos positivos técnicos. **Nunca apliques una corrección automática**
dentro de una URL, un identificador de archivo, una entidad HTML (`&iacute;`) o un
escape (`\u00f3`), ni sobre nombres propios: acentuar dentro de una entidad rompe la
palabra, y revertir a ciegas esa rotura des-acentúa palabras legítimas que estaban
bien. Comprueba al terminar que no desapareció ninguna palabra acentuada.

## Cómo quiero que trabajes

- Verifica cada fase contra la fuente real y dime los números: actividades creadas,
  preguntas importadas, páginas del PDF, errores de compilación.
- Si algo no se puede hacer, dilo y termina todo lo demás; no lo escondas en un
  resumen optimista.
- Al cerrar, vuelve a listarme en el chat los insumos que sigan faltando y las fases
  que quedaron pendientes por ellos.
- Antes de borrar o reemplazar algo en el aula, comprueba que no tenga entregas de
  estudiantes y dímelo.
- `insumos/` es de solo lectura: todo lo que generes va a `<curso>/` o a `scripts/`.
- Todo lo generado sale de una fuente del repositorio (sílabo, planificación, manual,
  presentaciones). No inventes contenido de la materia.
