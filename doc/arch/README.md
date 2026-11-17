# Arquitectura y modelo UML de Verifika

Esta carpeta conserva las fuentes editables Mermaid. Los PNG se generan desde
estos archivos para usarlos en presentaciones y revisiones visuales; no
reemplazan las fuentes.

## Diagramas

| Archivo | Vista | Para qué sirve |
| --- | --- | --- |
| `c4-contexto.mmd` | C4 de contexto | Delimita Verifika y sus actores y sistemas externos. |
| `c4-contenedores.mmd` | C4 de contenedores | Muestra la interfaz, el servicio de aplicación y el repositorio. |
| `actividad-cu01.mmd` | Actividad | Expone el flujo de CU-01 y sus alternativas. |
| `secuencia-cu01.mmd` | Secuencia | Muestra las llamadas de CU-01 y sus respuestas. |
| `estados-verificacion.mmd` | Estados | Define las transiciones del ciclo de vida de una verificación. |
| `clases-dominio.mmd` | Clases | Define entidades, operaciones y multiplicidades del modelo. |

## Decisión de arquitectura

`001-servicio-verificacion.md` registra la decisión propuesta de coordinar el
caso de uso desde `ServicioVerificacion`, con sus motivos, alternativas y
consecuencias. Es una decisión de diseño, no una descripción de una
implementación existente. Las tecnologías siguen pendientes.

## Consistencia UML

La secuencia invoca operaciones del modelo de clases: `buscarCoincidencia`,
`crear`, `analizar`, `registrar`, `agregarRevision`, `establecerEstado` y
`guardar`. La actividad y el diagrama de estados comparten las ramas de falta
de evidencia (`SinEvidencia`) y desacuerdo (`EnDisputa`).
