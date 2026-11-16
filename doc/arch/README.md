# Diagramas de diseño · Verifika

Los diagramas Mermaid de esta carpeta detallan CU-01 y su consistencia con el dominio. Son un modelo de diseño propuesto, no una descripción de funcionalidades ya implementadas.

| Archivo | Pregunta que responde | Trazabilidad |
| :--- | :--- | :--- |
| [Clases](clases-dominio.mmd) | Qué objetos principales existen y qué operaciones tienen | RF-001 a RF-006; CU-01, CU-03 a CU-08 |
| [Secuencia de CU-01](secuencia-cu01.mmd) | Qué mensajes ocurren y en qué orden durante una verificación | CU-01; RF-001, RF-002, RF-003; RNF-TRZ-001 |
| [Estados de Verificación](estados-verificacion.mmd) | Cómo cambia el ciclo de vida de una verificación | RF-003, RF-004; CU-01, CU-05, CU-06, CU-07 |
| [Actividad de CU-01](actividad-cu01.mmd) | Qué hace cada participante y qué decisiones aparecen en el proceso | CU-01; RF-001, RF-002, RF-003, RF-005, RF-006 |

## Diagramas anteriores del proyecto

- [Diagrama de casos de uso editable](../../docs/diagramas/casos-de-uso.drawio) y [vista PNG](../../docs/diagramas/casos-de-uso.png).
- [Especificación de CU-01 y sus flujos alternos](../../docs/casos-de-uso.md).

## Decisiones y alcance

- La secuencia representa el camino normal de CU-01 y un único `alt` para el fallo más importante: evidencia insuficiente. Asume que no hay una coincidencia previa; la ruta RF-005 se muestra en el diagrama de actividad y se desarrolla en CU-03.
- La clase `Verificacion` es el registro que permanece en el historial. `Revision` representa cada intento para conservar y comparar el original con una segunda revisión.
- `EstadoVerificacion` enumera exactamente los estados de la máquina de estados. El atributo de estado está en `Verificacion`.
- La interfaz, el servicio y el repositorio son clases de diseño propuestas para hacer explícitos los mensajes de la secuencia. El agente de IA es externo al sistema.
- La secuencia no incorpora una operación de base de datos como decisión de producto: `RepositorioVerificaciones` representa el contrato de guardar y consultar historial, cuya tecnología no está definida.

## Revisión de consistencia

- Cada mensaje de la secuencia tiene una operación correspondiente en la clase que lo recibe.
- Todos los estados de `EstadoVerificacion` aparecen en la máquina de estados y representan situaciones de la entidad `Verificacion`.
- La ruta sin evidencia termina con el estado `SinEvidencia` y la interfaz muestra «no se pudo verificar», conforme a RF-003.
- Si una segunda revisión difiere, ambas revisiones se conservan y la verificación pasa a `EnDisputa`; solo moderación la resuelve, conforme a RF-004.
- La actividad incluye el aviso de coincidencia previa (RF-005) y la limitación de fuente degradada (RF-006). La secuencia normal presupone que esas ramas no se activan.

## Qué le faltaba al modelo de clases

Antes de este trabajo el repositorio tenía análisis de sustantivos y casos de uso, pero no un diagrama de clases con operaciones. Al construir la secuencia hicieron falta clases de diseño para representar la interfaz, el servicio que coordina CU-01 y el contrato del historial. También hicieron falta las operaciones `buscarCoincidencia`, `guardar`, `registrarSinEvidencia` y `agregarRevision`, además de `Verificacion.estado` y la clase `Revision` para conservar el intento inicial y el segundo sin sobrescribirlos. Se incorporaron al diagrama conceptual para que los mensajes de la secuencia tengan receptor y operación definidos.

## Alcance de la revisión

La regla adicional de comparar componentes C4 con paquetes no se pudo aplicar: en los archivos previos del repositorio no hay diagramas C4 ni de paquetes. Si esos diagramas forman parte de otra entrega del curso, habrá que agregarlos para poder revisar esa correspondencia. El diagrama de casos de uso anterior sí queda enlazado arriba.

