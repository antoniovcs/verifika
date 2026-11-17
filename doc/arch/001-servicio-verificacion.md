# ADR-001: Coordinar CU-01 desde ServicioVerificacion

> Registro de decisión de arquitectura · Verifika · SIS3407

## Estado

Propuesta. Pendiente de validación con el prototipo y de definir tecnologías.

## Contexto

El caso de uso CU-01 recibe afirmaciones o fragmentos de código, busca una
coincidencia previa, solicita análisis externo, evalúa la evidencia y conserva
el resultado con sus fuentes.

Dos hechos del caso justifican una frontera de coordinación explícita:

1. La entrevista encontró que una verificación anterior solo se localiza cuando
   alguien la busca manualmente en el historial. RF-005 pide detectar la
   coincidencia antes de repetir el análisis.
2. RF-001 exige conservar la verificación y sus fuentes, mientras RF-003 prohíbe
   forzar un veredicto concluyente cuando la evidencia no alcanza.

El flujo tiene que coordinar persistencia y un agente externo, además de
conservar las ramas de coincidencia, falta de evidencia y segunda revisión.
Todavía no se han elegido tecnologías ni se ha implementado el sistema.

## Decisión

Mantener la coordinación del caso de uso en `ServicioVerificacion`, una capa de
servicio de aplicación. La interfaz delega el flujo al servicio. El servicio
consulta `RepositorioVerificaciones`, llama a `AgenteIA` cuando corresponde,
registra cada `Revision` y guarda la `Verificacion` con sus fuentes.

`RepositorioVerificaciones` queda como interfaz para aislar el dominio de la
tecnología de persistencia. No se prescribe un protocolo para el agente ni un
motor de base de datos.

## Patrón y ubicación

- **Service Layer / servicio de aplicación:** `ServicioVerificacion` coordina
  las operaciones de CU-01 y las reglas de flujo sin ponerlas en la interfaz.
- **Repository:** `RepositorioVerificaciones` encapsula la búsqueda de
  coincidencias, la consulta por identificador y el guardado del historial.

Ambos aparecen en `clases-dominio.mmd` y en `c4-contenedores.mmd`.

## Consecuencias positivas

- La búsqueda previa de RF-005 y el registro de RF-001 tienen un punto de
  coordinación identificable.
- La interfaz no necesita conocer cómo se consulta el historial ni cómo se
  invoca al agente.
- El repositorio puede sustituir su tecnología sin cambiar las reglas de
  dominio.
- Las ramas de RF-003 y RF-004 pueden conservar revisiones y estados sin
  sobrescribir la evidencia previa.

## Consecuencias negativas y riesgos

- `ServicioVerificacion` puede acumular demasiadas responsabilidades si futuras
  funciones se agregan sin separar políticas del dominio.
- La respuesta depende de la latencia y disponibilidad del agente y de las
  fuentes externas. Una falla puede retrasar o impedir un resultado concluyente.
- La búsqueda de coincidencias necesita una definición de “igual o similar”;
  falsos positivos podrían reutilizar un resultado que no corresponde.
- La interfaz del repositorio no resuelve por sí sola transacciones,
  concurrencia ni política de retención. Esas decisiones quedan pendientes.
- La separación propuesta añade contratos y coordinación frente a una llamada
  directa desde la interfaz, por lo que el diseño requiere pruebas de
  integración cuando se implemente.

## Alternativas consideradas

1. **Poner la orquestación en la interfaz:** se descarta porque mezclaría
   presentación, búsqueda, agente y persistencia, dificultando mantener las
   mismas reglas para otros clientes.
2. **Dejar que el agente coordine todo:** se descarta porque el agente externo
   no debe controlar el historial ni decidir si Verifika conserva revisiones
   anteriores.
3. **Elegir ahora un stack concreto:** se aplaza porque los requisitos y el
   prototipo no fijan plataforma, base de datos ni proveedor de agente.

## Trazabilidad

- Requisitos: RF-001, RF-003, RF-004, RF-005, RNF-TRZ-001 y RNF-DISP-001.
- Entrevista: búsqueda manual de coincidencias y necesidad de no forzar
  resultados sin evidencia suficiente.
- Diagramas: `c4-contexto.mmd`, `c4-contenedores.mmd`,
  `clases-dominio.mmd`, `secuencia-cu01.mmd`, `actividad-cu01.mmd` y
  `estados-verificacion.mmd`.

## Validación pendiente

Validar con el prototipo si el aviso de coincidencia previa es comprensible,
definir el criterio de similitud y medir el tiempo de respuesta cuando el agente
esté integrado.
