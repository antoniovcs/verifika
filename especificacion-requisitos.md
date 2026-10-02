# Especificación de requisitos

**Sistema:** Verifika
**Autor:** Ricardo Antonio Vargas Cremades
**Versión:** 1.0
**Fecha de la última actualización:** 1 de octubre de 2026

---

## 1. Propósito y alcance

**Propósito del documento:** este documento detalla los requisitos funcionales y no funcionales de Verifika, a partir de lo definido en la Visión del producto y enriquecido con lo elicitado en la entrevista al responsable de moderación y curación de fuentes del sistema. Va dirigido a quien continúe el desarrollo del sistema (incluyéndome a mí mismo en semanas posteriores) y a quien evalúe el proyecto.

**Alcance del sistema:**

- Verificación de afirmaciones de texto pegadas por el usuario, usando un agente de IA que consulta fuentes confiables.
- Verificación de fragmentos de código pegados por el usuario, contrastando contra documentación oficial y buenas prácticas reconocidas.
- Historial de verificaciones ya realizadas, consultable por otros usuarios.
- Posibilidad de que un usuario marque un veredicto como "no me convence" y solicite una segunda revisión.

**Fuera del alcance:**

- Verificación de contenido multimedia (imágenes o video generado por IA).
- Verificación en tiempo real durante una conversación con un chatbot.
- Que el sistema aprenda o ajuste sus propios criterios de verificación con el tiempo a partir de sus errores pasados.

*(Este alcance se retoma íntegro de la Visión del producto; no ha cambiado desde esa versión.)*

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
|---|---|---|
| Usuario casual | Le cree a ciegas al chatbot, o busca manualmente cada dato en internet | Pegar una duda y recibir un veredicto rápido y claro (cierto, falso, dudoso) |
| Usuario avanzado / investigador | Verifica por su cuenta cruzando varias fuentes, sin un lugar que le muestre esa evidencia de forma ordenada | Ver exactamente qué fuentes citó el agente y poder pedir una segunda revisión si no le convence |
| Programador | Usa el código sugerido por el chatbot tal cual, o lo valida manualmente contra documentación oficial cuando tiene dudas | Saber si hay una forma más óptima de resolver algo, con un formato que no sea un simple cierto/falso |


El usuario casual quiere un resultado simple tipo semáforo, pero el usuario avanzado necesita ver el detalle completo de las fuentes, y el programador no puede recibir el mismo tipo de veredicto porque "optimizar código" es un espectro, no una respuesta binaria. Esto obliga a que el formato del resultado cambie según el tipo de contenido verificado, en vez de ser uno solo para los tres usuarios.

*(Nota de origen: la tabla anterior proviene de la Visión del producto. La entrevista de elicitación con el responsable de moderación de Verifika confirmó y amplió las reglas de negocio que aparecen en las fichas de la sección 3 y 4; donde algo sigue sin confirmarse con el cliente real, se marca explícitamente en el campo Origen de cada requisito.)*

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
|---|---|---|---|
| RF-001 | Registro de verificación | Imprescindible | Visión del producto |
| RF-002 | Clasificación del tipo de contenido | Imprescindible | Visión del producto |
| RF-003 | Bloqueo de veredicto sin evidencia suficiente | Imprescindible | Entrevista con responsable de moderación, 1 de octubre de 2026 |
| RF-004 | Marcado de disputa entre revisiones | Imprescindible | Entrevista con responsable de moderación, 1 de octubre de 2026 |
| RF-005 | Aviso de contenido ya verificado | Deseable | Visión del producto |
| RF-006 | Degradación de fuentes poco confiables | Deseable | Entrevista con responsable de moderación, 1 de octubre de 2026 |

### 3.2 Fichas

**RF-001 · Registro de verificación**

| Campo | Contenido |
|---|---|
| Descripción | El sistema registra cada verificación realizada, junto con el contenido evaluado, el veredicto obtenido y las fuentes consultadas. |
| Origen | Visión del producto. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al completarse una verificación, esta aparece en el historial con el contenido original, el veredicto y al menos una fuente asociada. Si el agente no regresó ninguna fuente, el campo de fuentes queda explícitamente vacío, no se omite el registro. |
| Relacionado con | RF-005, RNF-TRZ-001 |

**RF-002 · Clasificación del tipo de contenido**

| Campo | Contenido |
|---|---|
| Descripción | El sistema clasifica el contenido recibido como afirmación de texto o como fragmento de código antes de enviarlo al agente de verificación. |
| Origen | Visión del producto. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Al recibir contenido, el sistema determina su tipo (texto o código) antes de iniciar la verificación, y ese tipo queda guardado junto con el resultado. Si el usuario indicó un tipo y el contenido no corresponde (por ejemplo, marcó "código" pero pegó una oración en español), el sistema lo señala antes de continuar. |
| Relacionado con | RF-001, RNF-USA-001 |

**RF-003 · Bloqueo de veredicto sin evidencia suficiente**

| Campo | Contenido |
|---|---|
| Descripción | El sistema impide mostrar un veredicto de "cierto" o "falso" cuando el agente no encuentra fuentes suficientes que lo respalden; en su lugar, muestra explícitamente "no se pudo verificar". |
| Origen | Entrevista con responsable de moderación, 1 de octubre de 2026: confirmó que esto ya ocurre de forma manual hoy, cuando el agente regresa una respuesta ambigua sobre un tema muy nuevo o muy específico. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si el agente regresa cero fuentes o fuentes que no respaldan directamente la afirmación, el sistema muestra "no se pudo verificar" en vez de forzar cierto o falso. Una prueba con una afirmación deliberadamente inventada debe producir este resultado, no una respuesta con falsa seguridad. |
| Relacionado con | RF-001, RNF-CONF-001 |

**RF-004 · Marcado de disputa entre revisiones**

| Campo | Contenido |
|---|---|
| Descripción | El sistema marca una verificación como "en disputa" cuando dos revisiones del mismo contenido arrojan resultados distintos, y no la resuelve automáticamente promediando o quedándose con la más reciente. |
| Origen | Entrevista con responsable de moderación, 1 de octubre de 2026: confirmó que hoy esto se revisa a mano, abriendo cada verificación por separado, sin una vista que las compare. |
| Prioridad | Imprescindible |
| Criterio de aceptación | Si dos verificaciones sobre el mismo contenido tienen veredictos distintos, el sistema cambia el estado a "en disputa" y lo mantiene así hasta que alguien lo resuelva explícitamente. El sistema nunca sobrescribe un veredicto en disputa con el resultado más reciente sin intervención. |
| Relacionado con | RF-001, RNF-USA-001 |

**RF-005 · Aviso de contenido ya verificado**

| Campo | Contenido |
|---|---|
| Descripción | El sistema notifica al usuario si la afirmación o el código que está por verificar ya existe en el historial, antes de generar una nueva verificación desde cero. |
| Origen | Visión del producto. |
| Prioridad | Deseable |
| Criterio de aceptación | Al recibir contenido que coincide (exacto o muy similar) con una verificación previa, el sistema muestra esa verificación existente antes de iniciar una nueva llamada al agente. |
| Relacionado con | RF-001 |

**RF-006 · Degradación de fuentes poco confiables**

| Campo | Contenido |
|---|---|
| Descripción | El sistema deja de usar como respaldo único de un veredicto cualquier fuente que haya resultado incorrecta más de dos veces, aunque pueda seguir apareciendo como referencia secundaria. |
| Origen | Entrevista con responsable de moderación, 1 de octubre de 2026. |
| Prioridad | Deseable |
| Criterio de aceptación | Si una fuente acumula más de dos verificaciones marcadas como incorrectas, el sistema la excluye como única fuente de un veredicto nuevo, aunque puede seguir mostrándola junto con otra fuente que sí respalde el resultado. |
| Relacionado con | RF-001, RF-003 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
|---|---|---|---|---|
| RNF-DISP-001 | Disponibilidad | Tiempo de respuesta de la verificación | Imprescindible | Visión del producto |
| RNF-TRZ-001 | Trazabilidad | Registro de la fuente exacta por veredicto | Imprescindible | Visión del producto |
| RNF-CONF-001 | Confiabilidad | Tasa de veredictos forzados sin evidencia | Imprescindible | Visión del producto |
| RNF-SEG-001 | Seguridad | Manejo de la clave de API del usuario | Imprescindible | Supuesto propio — pendiente de confirmar si el modelo final usa clave propia del usuario |

### 4.2 Fichas

**RNF-DISP-001 · Tiempo de respuesta de la verificación**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Disponibilidad |
| Descripción | El sistema entrega un veredicto en menos de 10 segundos desde que el usuario envía el contenido a verificar. |
| Métrica | Tiempo entre el envío del contenido y el despliegue del veredicto, medido en condiciones normales de uso (sin saturación del agente externo). |
| Origen | Derivado del tipo de sistema (de datos y análisis) y de la Visión del producto, donde se identificó que una espera larga empuja al usuario de vuelta a verificar manualmente. |
| Prioridad | Imprescindible |
| Por qué importa | Si el sistema tarda demasiado, el usuario abandona la verificación y regresa a confiar a ciegas o a buscar por su cuenta, que es justo el problema que Verifika busca resolver. |
| Afecta a | RF-001, RF-003 |

**RNF-TRZ-001 · Registro de la fuente exacta por veredicto**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Trazabilidad |
| Descripción | El sistema registra la fuente exacta consultada para cada veredicto, de forma que pueda mostrarse al usuario avanzado en cualquier momento posterior. |
| Métrica | Porcentaje de verificaciones cuyo registro incluye al menos una fuente identificable (nombre o referencia consultable), medido sobre una muestra de verificaciones guardadas. Meta: 100%, salvo los casos legítimos de "no se pudo verificar". |
| Origen | Visión del producto; reforzado por la entrevista, donde el responsable de moderación describió que comparar fuentes de verificaciones en disputa hoy es manual y lento por falta de una vista comparativa. |
| Prioridad | Imprescindible |
| Por qué importa | Sin trazabilidad, un veredicto es solo una opinión sin respaldo, y pierde todo su valor frente al usuario que más lo exige. |
| Afecta a | RF-001, RF-004 |

**RNF-CONF-001 · Tasa de veredictos forzados sin evidencia**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Confiabilidad |
| Descripción | El sistema no debe emitir un veredicto de "cierto" o "falso" cuando no cuenta con fuentes suficientes que lo respalden. |
| Métrica | Porcentaje de verificaciones sin fuentes suficientes que terminan mostrando "no se pudo verificar" en vez de un veredicto forzado, medido sobre una muestra de contenido deliberadamente ambiguo o inventado. Meta: 100%. |
| Origen | Visión del producto; confirmado en la entrevista, donde el responsable describió que hoy esto se corrige manualmente cuando el agente regresa una respuesta ambigua. |
| Prioridad | Imprescindible |
| Por qué importa | Un veredicto equivocado con apariencia de seguridad deja al usuario peor que si nunca hubiera preguntado, porque ahora confía en algo falso pensando que fue verificado. |
| Afecta a | RF-003 |

**RNF-SEG-001 · Manejo de la clave de API del usuario**

| Campo | Contenido |
|---|---|
| Atributo de calidad | Seguridad |
| Descripción | Si el modelo de operación usa clave de API provista por el usuario, el sistema nunca la almacena en texto plano ni la envía a ningún destino que no sea el proveedor del agente de IA. |
| Métrica | Cero incidentes de exposición de clave en pruebas de almacenamiento y transmisión; verificación mediante inspección de la base de datos y del tráfico saliente. |
| Origen | Supuesto propio, derivado de la decisión (todavía no cerrada del todo) de que el usuario podría poner su propia clave de API para no absorber el costo de las llamadas al agente. Pendiente de confirmar si este es el modelo final antes de construirlo. |
| Prioridad | Imprescindible |
| Por qué importa | Una fuga de credenciales rompería la confianza en el sistema de forma inmediata y sería el tipo de falla que ningún usuario perdona, sin importar qué tan bien funcione el resto del producto. |
| Afecta a | — |

---

## 5. Casos de uso

*Diagrama completo en* `casosdeuso.drawio`. *Actores: Persona usuaria, Agente de IA, Responsable de moderación.*

### CU-01 · Verificar una afirmación de texto

**Actores:** Persona usuaria, Agente de IA

**Precondición:** la persona tiene una afirmación que quiere verificar.

**Flujo normal:**

1. Pega la afirmación y selecciona "texto" como tipo de contenido.
2. El sistema envía la afirmación al agente de IA.
3. El agente regresa un resultado con fuentes.
4. El sistema muestra el veredicto (cierto, falso, dudoso).
5. El sistema guarda la verificación en el historial.

**Flujo alterno — Sin fuentes suficientes (RF-003):**

3a. El agente no encuentra fuentes suficientes.
3a.1. El sistema muestra "no se pudo verificar" en vez de forzar un veredicto.
3a.2. La verificación queda en la cola de CU-06 para revisión del responsable de moderación.

**Postcondición:** la persona ve un resultado y queda registrado en el historial.

### CU-02 · Evaluar un fragmento de código

**Actores:** Persona usuaria, Agente de IA

**Precondición:** la persona tiene un fragmento de código sugerido por un chatbot.

**Flujo normal:**

1. Pega el código y selecciona "código" como tipo de contenido.
2. El sistema envía el código al agente de IA.
3. El agente regresa un análisis contra documentación oficial y buenas prácticas.
4. El sistema muestra un nivel de optimización y sugerencias, no un veredicto binario.
5. El sistema guarda la evaluación en el historial.

**Postcondición:** la persona ve un análisis de optimización, no un cierto/falso.

### CU-03 · Consultar verificaciones anteriores

**Actor:** Persona usuaria

**Precondición:** existen verificaciones guardadas en el historial.

**Flujo normal:**

1. La persona entra a la sección de historial.
2. Busca por palabra clave o navega la lista.
3. El sistema muestra las verificaciones que coinciden, con su veredicto y fecha.

**Relación con RF-005:** si al iniciar CU-01 o CU-02 el sistema detecta que el contenido ya existe en el historial, redirige aquí antes de gastar una verificación nueva.

**Postcondición:** la persona ve resultados existentes sin repetir trabajo.

### CU-04 · Examinar las fuentes de un resultado

**Actor:** Persona usuaria

**Precondición:** existe un resultado de verificación con al menos una fuente registrada.

**Flujo normal:**

1. La persona abre un resultado específico (propio o del historial).
2. El sistema despliega la lista completa de fuentes citadas, con su nombre y el fragmento relevante.

**Postcondición:** la persona puede comprobar de dónde salió el veredicto, no solo confiar en él.

### CU-05 · Solicitar una segunda revisión

**Actor:** Persona usuaria

**Precondición:** existe un resultado con el que la persona no está conforme.

**Flujo normal:**

1. La persona marca el resultado como "no me convence".
2. El sistema reenvía el contenido al agente de IA para una nueva verificación.
3. El sistema compara el nuevo resultado con el original.

**Flujo alterno — Los veredictos no coinciden (RF-004):**

3a. El nuevo veredicto es distinto al original.
3a.1. El sistema marca la verificación como "en disputa" (continúa en CU-07).

**Postcondición:** la persona recibe confirmación del veredicto original, o ve que quedó en disputa.

### CU-06 · Revisar casos sin evidencia o en disputa

**Actor:** Responsable de moderación

**Precondición:** hay verificaciones marcadas como "no se pudo verificar" o "en disputa" pendientes.

**Flujo normal:**

1. El responsable abre el tablero de casos pendientes.
2. Revisa cada caso buscando contexto adicional.
3. Resuelve el caso manualmente o lo deja pendiente si requiere más información.

**Postcondición:** los casos revisados quedan resueltos o documentados con lo que falta.

### CU-07 · Resolver una disputa

**Actor:** Responsable de moderación

**Precondición:** una verificación está marcada como "en disputa" (originada en CU-05).

**Flujo normal:**

1. El responsable compara las fuentes citadas por cada revisión.
2. Decide cuál veredicto es correcto, o si ambos aplican con matices distintos.
3. El sistema actualiza el estado de la verificación según la decisión.

**Postcondición:** la disputa queda resuelta y el historial refleja la decisión final.

### CU-08 · Curar fuentes de referencia

**Actor:** Responsable de moderación

**Precondición:** una fuente ha sido usada como respaldo en al menos una verificación.

**Flujo normal:**

1. El responsable revisa el desempeño de una fuente (cuántas veces respaldó un veredicto correcto o incorrecto).
2. Si la fuente acumula errores repetidos, decide bajarle la categoría.

**Flujo alterno — Revisión manual antes de degradar (RF-006):**

2a. El responsable decide que un error fue un caso aislado y no degrada la fuente.

**Postcondición:** la lista de fuentes confiables queda actualizada.

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
|---|---|---|---|
| RF-001 | Visión del producto | CU-01, CU-02 | Pantalla de resultado / Historial |
| RF-002 | Visión del producto | CU-01, CU-02 | Selector "Afirmación de texto / Código" en Home |
| RF-003 | Entrevista 1 oct 2026 | CU-01 (flujo alterno), CU-06 | Pantalla de resultado (estado "no se pudo verificar") |
| RF-004 | Entrevista 1 oct 2026 | CU-05 (flujo alterno), CU-07 | Historial (estado "en disputa") |
| RF-005 | Visión del producto | CU-03 | *No implementado todavía en el prototipo — pendiente tras hallazgo de la entrevista* |
| RF-006 | Entrevista 1 oct 2026 | CU-08 | No implementado todavía en el prototipo |
| RNF-TRZ-001 | Visión del producto | CU-04 | Detalle de fuentes en la pantalla de resultado |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
|---|---|---|---|
| 1 de octubre de 2026 | RF-003, RF-004, RF-006 | Se agregaron tras la entrevista de elicitación con el responsable de moderación | La entrevista confirmó reglas de negocio que no eran obvias desde la Visión del producto original |
