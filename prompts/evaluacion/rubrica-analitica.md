# Rúbrica analítica

> 🎬 **Video:** próximamente · ☕ **Categoría:** Evaluación

## Para qué sirve

Genera una rúbrica analítica (criterios × niveles de desempeño) lista para pegar en tu documento o
en el aula virtual. Sirve para ensayos, proyectos, exposiciones, informes de laboratorio o cualquier
tarea que quieras evaluar con criterios claros y compartir con tus estudiantes antes de la entrega.

## Prompt

```text
Actúa como un especialista en evaluación educativa.

Crea una rúbrica analítica para evaluar la siguiente tarea:
- Tarea: [descripción de la tarea]
- Asignatura y nivel: [asignatura, curso o nivel]
- Resultados de aprendizaje que evalúa: [qué deben demostrar los estudiantes]
- Número de criterios: [3 a 6]
- Niveles de desempeño: [por ejemplo: Excelente, Bueno, En desarrollo, Insuficiente]
- Puntaje total: [por ejemplo: 10 puntos]

Requisitos:
1. Presenta la rúbrica en una tabla: una fila por criterio y una columna por nivel.
2. Asigna a cada criterio un peso o puntaje, y que la suma sea el puntaje total.
3. Describe cada nivel con conductas observables y concretas; evita adjetivos vagos
   como "bueno" o "adecuado" sin explicar qué significan.
4. Usa un lenguaje que los estudiantes entiendan, porque la recibirán antes de la entrega.
5. Al final, añade tres preguntas que el estudiante pueda usar para autoevaluarse.
```

## Campos a rellenar

| Campo | Ejemplo |
|---|---|
| `[descripción de la tarea]` | Ensayo argumentativo de 800 palabras sobre el uso del celular en clase |
| `[asignatura, curso o nivel]` | Lengua y Literatura, 2.º de bachillerato |
| `[qué deben demostrar los estudiantes]` | Formular una tesis, sostenerla con evidencias y refutar un contraargumento |
| `[3 a 6]` | 4 |
| `[niveles]` | Excelente, Bueno, En desarrollo, Insuficiente |
| `[puntaje total]` | 10 puntos |

## Variaciones

- **Rúbrica holística:** pide "una rúbrica holística de 4 niveles" si solo necesitas una nota global.
- **Lista de cotejo:** pide "una lista de cotejo con criterios de sí/no" para tareas cortas.
- **Coevaluación:** añade "redacta los criterios en primera persona para que los estudiantes evalúen
  a un compañero".
- **Formato para el aula virtual:** pide "entrégala también en formato CSV" para importarla en Moodle
  u otra plataforma.

## Consejos

- Revisa que los pesos reflejen lo que de verdad importa en la tarea; la IA tiende a repartirlos por igual.
- Si un nivel queda ambiguo, pide: "reescribe el nivel En desarrollo del criterio X con un ejemplo
  concreto de lo que haría el estudiante".
- Comparte la rúbrica con los estudiantes **antes** de la entrega: así también funciona como guía.
