# Análisis de sustantivos: Verifika

## Paso 1. Sustantivos identificados (sin filtrar)

afirmación, agente de IA, análisis, año, atributo de calidad, búsqueda, chatbot, código, código fuente, contenido, criterio, cuenta, dato, documentación oficial, duda, evidencia, fuente, fuente confiable, fuente degradada, historial, información, interfaz, investigador, lista de fuentes, moderación, modelo de IA, nota de actualización, optimización, persona usuaria, programador, prioridad, proceso, producto, prueba, registro, resultado, revisión, segunda revisión, sistema, SaaS, tipo de contenido, usuario avanzado, usuario casual, usuario, valoración, veredicto, verificación, verificación anterior, verificación duplicada, verificación en disputa, verificación sin evidencia.

## Paso 2. Clases candidatas (4)

Se conservaron sustantivos que tienen identidad o ciclo de vida propio, datos asociados y operaciones relevantes para las reglas del dominio.

| Clase | Sustantivos de los que sale | Por qué pasa el filtro |
| :--- | :--- | :--- |
| Verificación | verificación, contenido verificado, registro, historial | Representa el caso persistente de verificar contenido. Tiene contenido original, tipo, estado y fecha; se consulta después y conserva el resultado incluso si no hay fuentes. Es la unidad que el usuario busca en el historial. |
| Revisión | revisión, segunda revisión, análisis, resultado, veredicto | Cada intento de análisis puede producir una valoración y un conjunto de fuentes. Debe conservarse para comparar el intento inicial con el segundo; si sus resultados difieren, la verificación queda en disputa. |
| Usuario | persona usuaria, usuario casual, usuario avanzado, investigador, programador, responsable de moderación | Tiene identidad dentro del dominio y realiza acciones. Los perfiles y permisos varían por rol; no requieren clases distintas mientras no tengan datos o comportamientos propios separados. |
| Fuente | fuente, fuente confiable, fuente degradada, documentación oficial | Tiene identidad consultable y un estado de confiabilidad que puede cambiar. Se relaciona con las revisiones que la consultaron y puede dejar de ser respaldo único después de errores repetidos. |

## Paso 3. Qué sabe y qué hace cada clase

| Clase | Sabe | Hace |
| :--- | :--- | :--- |
| Verificación | Identificador; contenido original; tipo (texto o código); fecha de creación; estado (registrada, no se pudo verificar, dudosa, en disputa o resuelta); revisiones asociadas. | Inicia el proceso de verificación; conserva el historial; identifica una coincidencia previa; agrega revisiones; cambia a “en disputa” si hay resultados distintos; conserva el resultado original y las actualizaciones. |
| Revisión | Identificador; fecha; análisis; resultado o valoración; fuentes consultadas y evidencia asociada; si corresponde a la revisión inicial o a una segunda revisión. | Registra una evaluación; asocia las fuentes y fragmentos que la respaldan; permite comparar su resultado con otra revisión. |
| Usuario | Identificador y datos de acceso si el diseño final requiere cuenta; rol o permisos (casual, avanzado, programador o moderación, según la actividad). | Envía contenido; consulta verificaciones y fuentes; solicita una segunda revisión; si tiene permisos de moderación, revisa casos y resuelve disputas o actualiza la confiabilidad de fuentes. |
| Fuente | Identificador o referencia consultable; nombre; tipo o ámbito (por ejemplo, documentación oficial); historial de uso; conteo o registro de revisiones marcadas como incorrectas; nivel de confiabilidad. | Proporciona información para una revisión; se asocia como respaldo o referencia secundaria; puede degradarse y dejar de ser el único respaldo de un veredicto. |

## Paso 4. Sustantivos descartados y por qué

| Razón | Descartados | Qué son realmente |
| :--- | :--- | :--- |
| Es un atributo | contenido, afirmación, fragmento de código, tipo de contenido, texto, código, fecha, estado, prioridad, valoración, veredicto, nivel de confiabilidad, análisis, evidencia, fragmento relevante | Datos de Verificación, Revisión o Fuente. “Texto/código” y los estados son valores enumerados, salvo que más adelante adquieran ciclo de vida propio. |
| Es un atributo | identificador, credenciales, cuenta, rol, permisos, historial de errores | Datos de Usuario o Fuente. El historial de verificaciones se modela como una consulta o colección de Verificación, no como entidad independiente. |
| Es sinónimo o variante de otra | persona usuaria, usuario casual, usuario avanzado, investigador, programador, moderador, responsable de moderación | Usuario con un perfil, rol o permiso diferente. El agente de IA se considera actor/sistema externo, no usuario humano ni clase interna del dominio en este análisis. |
| Es sinónimo o parte de otra | verificación anterior, verificación duplicada, verificación sin evidencia, verificación en disputa | Una Verificación identificada por su estado o relación con otras; “sin evidencia” y “en disputa” describen resultados del flujo, no tipos de entidad distintos. |
| Es sinónimo o parte de otra | veredicto, resultado, valoración, respuesta | Resultado de una Revisión. En la verificación de código, la salida puede ser un nivel de optimización y sugerencias, no “cierto” o “falso”. |
| Es una colección, vista o relación | historial, lista de fuentes, búsqueda, tablero, reporte, registro de verificación | Formas de consultar Verificaciones y Revisiones. La búsqueda y el tablero son funciones o vistas del sistema. |
| Es una actividad o regla | verificación, segunda revisión, comparación, moderación, curación, evaluación, clasificación, consulta, registro, marcado de disputa, degradación de fuente | Operaciones o procesos que ejecutan las clases candidatas o el sistema. “Verificación” se conserva como clase cuando significa el caso persistente, no cuando nombra la actividad. |
| Es externo al sistema | chatbot, agente de IA, proveedor o modelo de IA, buscador, internet, páginas gov/edu, noticias, documentación oficial consultada | Actores, servicios o recursos externos. “Fuente” sí se conserva como clase porque Verifika necesita identificarla, vincularla a revisiones y mantener su confiabilidad. |
| Es demasiado general o técnico | sistema, producto, proceso, interfaz, pantalla, historial de navegación, SaaS, base de datos, criterio de aceptación, requisito, métrica, disponibilidad, confiabilidad, trazabilidad | El sistema completo, componentes de interfaz e infraestructura, o conceptos de análisis y calidad; no son entidades del dominio del problema. |
| No tiene identidad propia en el modelo actual | usuario casual, usuario avanzado, tipo de contenido, estado, tipo de fuente, etiqueta | Categorías o valores de Usuario, Verificación o Fuente. Podrían convertirse en entidades si se gestionaran como catálogos editables con atributos propios. |

