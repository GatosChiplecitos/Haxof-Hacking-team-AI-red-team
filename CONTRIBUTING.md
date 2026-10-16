# Cómo contribuir

El objetivo es que cada investigación resulte fácil de encontrar, entender y continuar. Se pueden subir apuntes incompletos: basta con indicar su estado y las preguntas pendientes.

## Elegir una ubicación

Dentro de `areas/hacking/`, elegir:

- `ai-red-team/`: pruebas adversarias sobre modelos, agentes y aplicaciones de IA.
- `ai-safety/`: riesgos del comportamiento, evaluaciones y salvaguardas de IA.
- `cybersecurity/`: aplicaciones, infraestructura, nube, identidades y datos.

Después, usar `apuntes/`, `herramientas/`, `metodologias/`, `laboratorios/` o `recursos/`, según el contenido.

Si una investigación toca varios temas, conservar el documento en la sección principal y enlazarlo desde las otras. Para un nuevo ámbito del equipo, crear `areas/<nombre-del-area>/README.md` y actualizar el [índice de áreas](areas/README.md).

## Nombres y formato

- Escribir principalmente en español; conservar los términos técnicos cuando ayuden a identificar el tema.
- Usar Markdown para los documentos y nombres en minúsculas con guiones: `evaluacion-de-agentes.md`.
- Guardar los archivos de apoyo en una carpeta con el mismo nombre del documento: `evaluacion-de-agentes/`.
- Usar enlaces relativos dentro del repositorio.
- Añadir cada aporte al índice `README.md` de su carpeta.
- Mantener la autoría en el documento; las carpetas se organizan por tema.
- Conservar las referencias y la atribución de los materiales de terceros.

## Estados

| Estado | Cuándo usarlo |
| --- | --- |
| Por explorar | Idea, referencia o investigación que todavía no se ha probado. |
| En pruebas | Trabajo en curso con comprobaciones pendientes. |
| Validado | Resultado reproducido con evidencia, entorno y versión documentados. |

Una actualización de la herramienta o un resultado contradictorio puede requerir volver a **en pruebas**. El estado no implica que un resultado se aplique a todos los entornos.

## Plantilla común

Copiar y adaptar lo siguiente en un archivo nuevo. Usar la fecha real del aporte en formato `AAAA-MM-DD`.

```markdown
# Título de la investigación

- **Autor:** Nombre o usuario de GitHub
- **Fecha:** AAAA-MM-DD
- **Última actualización:** AAAA-MM-DD
- **Estado:** Por explorar | En pruebas | Validado
- **Etiquetas:** tema, tecnología
- **Fuentes:** [Título del recurso](URL)

## Objetivo y contexto

Qué se investiga, para qué sirve y qué preguntas se buscan responder.

## Apuntes y hallazgos

Ideas principales, observaciones y conclusiones propias.

## Procedimiento o ejemplo

Pasos, comandos o ejemplos necesarios para entender o repetir el trabajo.
Indicar entorno, versiones y requisitos cuando corresponda.

## Resultados y evidencia

Qué se observó, qué se esperaba y cómo se comprobó.

## Límites y pendientes

Qué falta probar, dónde no aplica y qué dudas siguen abiertas.

## Referencias relacionadas

Enlaces a otros documentos del repositorio y fuentes adicionales.
```

Para una herramienta, añadir instalación, versión y ejemplos de uso. Para un laboratorio, añadir preparación, pasos de reproducción y limpieza del entorno. Para una metodología, añadir alcance, entradas, etapas y criterios de evaluación.

## Incorporar cambios

Para un apunte propio se puede trabajar directamente en la rama principal. Cuando el cambio afecte metodologías compartidas o reorganice contenido, usar una rama y una pull request para facilitar la revisión del equipo.
