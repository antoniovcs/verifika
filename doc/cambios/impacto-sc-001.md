# Análisis de impacto · SC-001

## Cambio recibido

SC-001 solicita avisar cuando el contenido enviado ya tenga una verificación
igual o similar, mostrar el resultado anterior antes de iniciar otro análisis y
permitir abrir esa verificación. Para contenido sin coincidencia, el flujo
normal continúa.

## Motivación en el caso

La entrevista indica que hoy la persona usuaria a veces encuentra verificaciones
previas por su cuenta en el buscador del historial. La búsqueda no es automática,
por lo que se puede repetir el análisis y generar registros duplicados.

## Elementos afectados

| Elemento | Impacto |
| --- | --- |
| RF-005 | Precisar detección y presentación de una coincidencia previa. Su prioridad actual es deseable. |
| CU-01 | Añadir la alternativa de coincidencia previa antes de invocar al agente. |
| CU-03 | Reutilizar la consulta de una verificación existente. |
| `ServicioVerificacion` | Coordinar la búsqueda antes del análisis y devolver la coincidencia. |
| `RepositorioVerificaciones` | Buscar por contenido y tipo, y recuperar el registro coincidente. |
| `secuencia-cu01.mmd` | Mantener búsqueda previa y añadir la respuesta de coincidencia a la interfaz. |
| `actividad-cu01.mmd` | Reflejar las ramas con y sin coincidencia y la apertura del resultado previo. |
| Prototipo | Añadir el aviso y la acción para abrir el resultado anterior. |

El modelo de clases ya contiene `buscarCoincidencia` y `mostrarCoincidencia`,
por lo que no se identifica una clase nueva. La relación actual debe revisarse
para asegurar que la secuencia invoque operaciones definidas en las clases.

## Estimación preliminar

**3–5 días-persona**, suponiendo que exista un repositorio consultable y una
interfaz de búsqueda. La estimación cubre definir el criterio de similitud,
conectar la búsqueda previa, actualizar los diagramas y el prototipo, y revisar
los casos con y sin coincidencia. No incluye elegir tecnologías ni implementar
un sistema de persistencia todavía inexistente.

## Riesgos y dependencias

- Un falso positivo puede mostrar un resultado que no corresponde al nuevo
  contenido.
- Un criterio demasiado estricto dejaría pasar duplicados.
- RF-005 sigue como deseable; el cambio no debe bloquear una verificación nueva
  cuando no hay coincidencia.
- La estimación depende de acordar qué significa “igual o similar” para texto y
  código.

## Recomendación de la dupla

Mantener la prioridad media sugerida en SC-001 y aceptar el cambio cuando se
defina un conjunto de prueba con coincidencias y no coincidencias, y el aviso
muestre el resultado existente antes de iniciar otro análisis.

## Criterios de aceptación propuestos

1. Con contenido igual o similar a una verificación del conjunto de prueba, el
   sistema muestra esa verificación antes de invocar al agente y permite abrirla.
2. Con contenido sin coincidencia en el conjunto de prueba, el sistema permite
   iniciar una nueva verificación.
3. El aviso conserva el tipo de contenido y no trata una mejora de código como
   veredicto de cierto o falso.
