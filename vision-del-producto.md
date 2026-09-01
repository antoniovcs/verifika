# Visión del producto

> **Plantilla del curso · Ingeniería de Software I · SIS3407**
> Este documento es el primer entregable del semestre y la base de todo lo que viene después.
> Se entrega completo en la **semana 4** y se presenta ante el grupo.

---

**Autor:** Anton
**Fecha de la última versión:** 25 de agosto de 2026
**Repositorio:** https://github.com/antoniovcs/verifika/tree/main

---

## 1. Descripción del sistema

**Nombre del sistema:** Verifika

**Descripción:** Verifika es un sistema donde una persona puede pegar algo que le dijo un chatbot de inteligencia artificial —un dato, una afirmación, o un pedazo de código— y un agente de inteligencia artificial lo revisa contra fuentes confiables para decir qué tan cierto u óptimo es. El sistema guarda ese resultado, para que la próxima persona con la misma duda no tenga que empezar de cero.

---

## 2. Problema y usuarios

**El problema:** la gente recibe respuestas de chatbots de IA todo el tiempo —afirmaciones, explicaciones, código— y no tiene una forma rápida de saber qué tan confiables son. El resultado es que muchas veces le cree a la IA sin cuestionarla, incluso cuando se equivoca.

**Cómo se resuelve hoy sin el sistema:** cada persona verifica por su cuenta, buscando manualmente en internet dato por dato, o simplemente confía en lo que la IA le dijo sin comprobarlo. Ese trabajo de verificación, cuando se hace, se pierde: nadie más se beneficia de él, y la siguiente persona con la misma duda empieza desde cero.

**Usuarios del sistema:**

| Tipo de usuario | Qué necesita del sistema | Qué le preocupa |
|---|---|---|
| Usuario casual | Pegar una duda o afirmación y recibir un veredicto rápido y claro (cierto, falso, dudoso) | Perder tiempo leyendo detalles técnicos que no entiende ni le interesan |
| Usuario avanzado / investigador | Ver exactamente qué fuentes citó el agente, poder marcar que un veredicto no le convence y pedir una segunda revisión | Que el sistema le muestre un resultado simplificado sin poder comprobar de dónde salió |
| Programador | Pegar un fragmento de código sugerido por un chatbot y saber si hay una forma más óptima de resolverlo, contrastando contra documentación oficial o buenas prácticas reconocidas | Que el sistema lo trate igual que una afirmación de texto y le dé un "cierto/falso" que no tiene sentido para código |

**Un conflicto entre usuarios:** el usuario casual quiere un resultado simple, tipo semáforo (cierto/falso/dudoso), pero el usuario avanzado necesita ver el detalle completo de las fuentes que usó el agente, y el programador ni siquiera puede recibir ese mismo tipo de veredicto, porque "optimizar código" no es una respuesta binaria sino un espectro (más rápido, más legible, más idiomático). Forzar un solo formato de resultado para los tres usuarios rompe la utilidad del sistema para al menos dos de ellos. La decisión de diseño real que esto obliga a tomar es: **¿el sistema muestra siempre el mismo tipo de veredicto, o el tipo de contenido que se verifica (afirmación vs. código) cambia la forma en que se presenta el resultado?**

---

## 3. Alcance

### Dentro del alcance

- Verificación de afirmaciones de texto pegadas por el usuario, usando un agente de IA que consulta fuentes confiables (Wikipedia, sitios de noticias reconocidos, documentación oficial)
- Verificación de fragmentos de código pegados por el usuario, contrastando contra documentación oficial y buenas prácticas reconocidas
- Historial de verificaciones ya realizadas, consultable por otros usuarios para no repetir el mismo trabajo
- Posibilidad de que un usuario marque un veredicto como "no me convence" y solicite una segunda revisión

### Explícitamente fuera del alcance

- Verificación de contenido multimedia (imágenes o video generado por IA)
- Verificación en tiempo real durante una conversación con un chatbot (el usuario debe pegar el contenido después de recibirlo)
- Que el sistema aprenda o mejore sus criterios de verificación con el tiempo a partir de sus propios errores

**Por qué queda fuera:** que el sistema aprenda de sus propios errores implica construir un mecanismo de aprendizaje automático propio, lo cual es un problema de otro curso y no aporta al objetivo central del semestre, que es diseñar bien el sistema alrededor de un agente de IA ya existente, no construir inteligencia artificial desde cero.

---

## 4. Tipo de sistema y restricciones

**Tipo de sistema:** De datos y análisis

**Por qué es de ese tipo:** el valor central de Verifika no es solo guardar y mostrar información, sino procesarla: tomar una afirmación o un fragmento de código, contrastarlo contra fuentes externas a través de un agente de IA, y producir un análisis (un veredicto con evidencia) que antes no existía. Eso lo distingue de un sistema de información simple, donde el sistema solo administraría datos que el usuario ya tiene.

**Atributos de calidad que impone:**

| Atributo | Por qué importa en mi caso | Qué pasa si no se cumple |
|---|---|---|
| Confiabilidad | El usuario toma decisiones basado en el veredicto del sistema | Si el sistema da un veredicto con seguridad y resulta incorrecto, el usuario queda peor que si nunca hubiera preguntado, porque ahora confía en algo falso pensando que fue verificado |
| Trazabilidad | El usuario avanzado necesita poder comprobar de dónde salió cada veredicto | Sin trazabilidad, un veredicto es solo una opinión sin respaldo, y pierde todo su valor frente al usuario que más lo exige |
| Disponibilidad razonable | Los usuarios esperan una respuesta en un tiempo corto, no minutos de espera por cada verificación | Si el sistema tarda demasiado, la gente deja de usarlo y vuelve a verificar manualmente, que es justo el problema que se quiere resolver |

**Reglas de negocio que ya identifiqué:**

1. Si el agente de IA no encuentra fuentes suficientes para respaldar o refutar una afirmación, el sistema no puede forzar un veredicto de "cierto" o "falso" — debe devolver explícitamente "no se pudo verificar", porque un veredicto inventado por falta de información es más dañino que admitir que no se sabe.
2. Si dos revisiones del mismo contenido llegan a resultados distintos (por ejemplo, al pedir una segunda revisión), el sistema no puede simplemente quedarse con el resultado más reciente o promediar ambos — debe marcar el contenido como "en disputa" hasta que se resuelva, porque mostrar seguridad donde no la hay es peor que mostrar que existe un desacuerdo.
3. El tipo de contenido que se verifica (afirmación de texto vs. fragmento de código) determina el formato del veredicto que se muestra: uno puede resolverse como cierto/falso/dudoso, pero el otro necesita mostrarse como un espectro de mejora, no como una respuesta binaria.

---

## 5. Ciclo de vida elegido

**Modelo elegido:** Prototipado rápido

**Por qué le conviene a este proyecto:** el punto más incierto de todo el diseño de Verifika no es técnico, es de experiencia de usuario: todavía no sé con certeza si el veredicto debe verse igual para los tres tipos de usuario o si el formato debe cambiar según el contenido (afirmación de texto vs. código), y esa es exactamente el tipo de duda que no se resuelve escribiéndola en un documento, sino mostrándosela a alguien y viendo cómo reacciona. Trabajo solo, sin un cliente real disponible más allá de mis propios compañeros y el profesor, así que necesito una forma barata de validar esa decisión antes de construir el sistema completo alrededor de una suposición equivocada. El riesgo más grande del proyecto no está en la parte técnica (llamar a un agente de IA y guardar resultados es relativamente directo), está en diseñar mal la forma en que se presenta el veredicto a cada usuario. El prototipado rápido deja que ese riesgo se descubra temprano, con un prototipo desechable, en vez de hasta el final.

### Alternativas descartadas

**Alternativa 1: Cascada**

*Por qué la descarté:* la cascada apuesta a que los requisitos ya están claros y no van a cambiar, y ese no es mi caso. La forma exacta en que se muestra un veredicto —lo que más le importa a los tres tipos de usuario que definí— sigue siendo una pregunta abierta, no un requisito cerrado. Si sigo cascada y me equivoco en la fase de especificación, no lo voy a descubrir hasta la validación, cuando ya esté todo construido encima de esa decisión.

**Alternativa 2: Espiral**

*Por qué la descarté:* el modelo espiral está pensado para proyectos de alto riesgo, donde vale la pena invertir tiempo en analizar riesgos de forma explícita en cada vuelta del ciclo. Mi proyecto es de riesgo moderado y lo trabajo solo, en un semestre con tiempo limitado — meter el nivel de gestión de riesgo que pide el modelo espiral es más proceso del que el tamaño de este proyecto justifica. Lo que necesito no es analizar riesgos formalmente, es validar rápido una decisión de diseño con algo tangible, que es justo lo que ofrece el prototipado sin la carga adicional del espiral.

---

## Antes de entregar

Reviso que el documento cumpla lo siguiente:

- [x] La descripción del apartado 1 se entiende sin ser del área
- [x] Hay al menos dos tipos de usuario con necesidades distintas
- [x] Identifiqué un conflicto real entre usuarios
- [x] El alcance dice qué queda fuera, no solo qué queda dentro
- [x] Las exclusiones son específicas, no genéricas
- [x] Identifiqué el tipo de sistema y al menos dos atributos de calidad
- [x] Anoté al menos tres reglas de negocio no obvias
- [x] Justifiqué el ciclo de vida contra dos alternativas descartadas
- [ ] El documento está en mi repositorio y se puede leer desde el navegador *(confirmar que ya esté subido en `docs/vision-del-producto.md`)*
- [x] Borré todas las instrucciones en cursiva de la plantilla
