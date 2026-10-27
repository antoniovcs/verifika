# Diagramas de arquitectura Verifika

## 1. Nivel 1 Diagrama de contexto

El sistema completo, quién lo usa y con quién habla.

```mermaid
flowchart TD
    usuario([Persona usuaria])
    moderador([Responsable de moderación])
    sis[Verifika]
    agente[Agente de IA externo]
    usuario -->|verifica texto o código y consulta el historial| sis
    moderador -->|revisa pendientes, resuelve disputas y cura fuentes| sis
    sis -->|solicita verificaciones y recibe análisis con fuentes| agente
    style sis fill:#ff5d00,color:#fff
```

## 2. Nivel 2 Diagrama de contenedores

Las piezas que se despliegan y se ejecutan por separado.

```mermaid
flowchart TD
    usuario([Persona usuaria])
    moderador([Responsable de moderación])
    subgraph sistema[Verifika]
        web[App web<br/>React]
        api[API<br/>Node.js]
        db[(Base de datos<br/>PostgreSQL)]
    end
    agente[Agente de IA externo]
    usuario -->|usa| web
    moderador -->|revisa y resuelve casos| web
    web -->|HTTPS/JSON| api
    api -->|lee y escribe| db
    api -->|HTTPS/JSON| agente
    style api fill:#ff5d00,color:#fff
```
## 3. Nivel 3 Diagrama de componentes

Lo que hay dentro de un contenedor: la API de Verifika.

```mermaid
flowchart TD
    web[App web]
    subgraph api[API de Verifika]
        verificacion[Verificaciones<br/>clasificación y evaluación de contenido]
        historial[Historial<br/>revisiones y evidencia]
        moderacion[Moderación<br/>pendientes y disputas]
        fuentes[Fuentes<br/>categorías y confiabilidad]
        agente[Verificador<br/>interfaz de análisis]
    end
    db[(Base de datos)]
    proveedor[Agente de IA externo]
    web -->|solicita verificación o segunda revisión| verificacion
    web -->|consulta resultados y evidencia| historial
    web -->|revisa y resuelve casos| moderacion
    verificacion -->|consulta coincidencias y registra revisiones| historial
    verificacion -->|solicita análisis con evidencia| agente
    agente -->|HTTPS/JSON mediante adaptador del proveedor| proveedor
    verificacion -->|consulta confiabilidad de las fuentes| fuentes
    verificacion -->|remite casos sin evidencia o con resultados distintos| moderacion
    moderacion -->|consulta revisiones y registra la resolución| historial
    moderacion -->|registra errores y actualiza categorías| fuentes
    historial -->|lee y escribe revisiones y evidencia| db
    moderacion -->|lee y escribe casos pendientes| db
    fuentes -->|lee y escribe fuentes y categorías| db
    style verificacion fill:#ff5d00,color:#fff
```