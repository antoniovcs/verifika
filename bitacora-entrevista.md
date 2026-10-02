# Bitácora de entrevista · Verifika

*Entrevistado: dupla, representando al responsable de moderación y curación de fuentes (según ficha de dominio).*

## 1. Preguntas de verificación

**¿Cuénteme de alguna vez que no hayan podido encontrar información suficiente sobre algo que alguien quería verificar? ¿Qué hicieron en ese caso?**

Sí, pasa más seguido de lo que creería con temas muy nuevos. Hace unas semanas alguien pegó una afirmación sobre un lanzamiento de software que acababa de salir, y el agente no encontró nada confiable todavía, solo rumores. En vez de forzar un veredicto, yo entré y lo revisé a mano, y lo dejamos como "no se pudo verificar" hasta que hubiera algo más sólido escrito sobre el tema.

**¿Ha pasado que dos verificaciones sobre lo mismo lleguen a resultados diferentes? ¿Cómo se dieron cuenta y qué se hizo después?**

Sí, nos pasó con una afirmación sobre un tema de salud. Una revisión dijo que era cierta y otra la marcó como dudosa. Nos dimos cuenta porque alguien del equipo revisando el tablero notó que aparecían dos entradas casi idénticas con veredictos distintos. Tuve que abrir las dos por separado, comparar qué fuente citó cada una, y al final la dejamos en disputa hasta resolverlo.

**¿Qué pasa cuando descubren que una fuente que normalmente usan dio información incorrecta? Cuénteme la última vez que ocurrió.**

Hay una fuente que usábamos para temas de nutrición que nos dio problemas dos veces seguidas. Ya no la dejamos como la única fuente para un veredicto; todavía aparece si acaso como referencia, pero ya no decide ella sola si algo es cierto o falso.

**Cuando alguien pregunta algo que ya se verificó antes, ¿cómo se enteran de que ya existe esa verificación?**

Ahorita no tenemos una forma muy elegante de hacerlo, la verdad. A veces el mismo usuario nota que la pregunta se parece a una ya resuelta porque aparece en el buscador del historial, pero no es automático todavía.

**¿Se maneja distinto cuando alguien trae código, comparado con cuando trae una afirmación normal? Deme un ejemplo.**

Totalmente distinto. Con código nunca decimos "cierto" o "falso", porque no tiene sentido. Por ejemplo, alguien trajo una función para quitar elementos duplicados de una lista, y el código no estaba mal, pero no era la forma más eficiente. Ahí le damos algo como un puntaje de qué tan óptimo es, y unas sugerencias, no un veredicto binario.

**¿Cuánto se tardan normalmente en darle una respuesta a alguien desde que la piden?**

Casi siempre es rápido, unos segundos. El único momento en que se tarda más es cuando yo tengo que meterme a revisar algo a mano, ahí sí puede tardar horas, mientras me desocupo.

**¿Cómo manejan hoy el acceso o las credenciales que usa el sistema para conectarse con el agente de IA?**

Eso la verdad no me toca a mí directamente, eso lo lleva la persona que programa el sistema. Lo que sí sé es que no guardamos nada de eso visible en ningún lado que yo pueda ver desde mi parte.

## 2. Preguntas de contexto

**Cuénteme cómo es un día normal en Verifika, qué tipo de cosas le llegan y con qué frecuencia.**

Llego en la mañana y lo primero que hago es revisar el tablero de lo que quedó pendiente de la noche, normalmente entre diez y veinte cosas marcadas como dudosas o en disputa. De ahí me voy resolviendo lo que puedo yo solo, y lo que es muy técnico lo dejo para consultarlo con el resto del equipo.

**¿Quiénes son las personas que más usan esto, y en qué se nota que son distintas entre sí?**

Hay de todo, pero se nota mucho la diferencia entre alguien que solo quiere saber si algo es cierto y ya, y alguien que me escribe pidiendo que le explique por qué llegamos a ese resultado, con todo y fuente. Los que preguntan código son otro grupo aparte, casi nunca les interesa el mismo tipo de respuesta que a los demás.

## 3. Cómo resuelven hoy sin el sistema

**Deme un ejemplo real de la última vez que alguien verificó algo. ¿Qué trajo, y qué pasó paso a paso hasta que le dieron una respuesta?**

La más reciente que recuerdo fue alguien que pegó la afirmación de que cierto alimento ayuda a dormir mejor. Se mandó al agente, el agente regresó con un par de fuentes, una decía que sí ayudaba un poco y otra decía que no había evidencia suficiente. Como no era un sí o no claro, se marcó como dudoso y se le mostró al usuario las dos fuentes para que él viera el contraste.

**Cuénteme de una verificación que les haya costado trabajo resolver. ¿Qué la hizo difícil?**

La de salud que mencioné antes, la de las dos revisiones que no coincidían. Fue difícil porque las dos fuentes eran igual de respetables, solo que hablaban de poblaciones distintas, y entender eso tomó tiempo.

**¿Cómo deciden hoy, en el momento, si una fuente es lo suficientemente confiable para usarla en una verificación?**

Vemos si ya la hemos usado antes y no nos ha fallado, y si es del tipo correcto para el tema, por ejemplo documentación oficial si es de código. Si es una fuente nueva que no conocemos, la tratamos con más cuidado hasta que demuestre que es consistente.

## 4. Excepciones

**¿Qué pasa cuando alguien no está de acuerdo con el resultado que le dieron? Cuénteme la última vez que ocurrió.**

Hace poco alguien se quejó de un veredicto sobre código, decía que su solución sí era buena y que no estábamos tomando en cuenta su contexto. Lo revisé y la verdad tenía algo de razón, el contexto sí importaba, así que ajustamos la sugerencia.

**¿Ha habido algún caso raro o inesperado que los haya hecho cambiar cómo hacen las cosas? ¿Cuál fue?**

Sí, el de la fuente de nutrición que fallaba seguido. Antes no teníamos ninguna regla sobre qué hacer cuando una fuente se equivoca varias veces, y después de esa fue cuando empezamos a bajarle la categoría a una fuente que ya no es confiable.

## Qué aprendimos de esta entrevista

Lo que más confirmó esta sesión es que las reglas que ya habíamos supuesto (no forzar veredicto, marcar disputas, degradar fuentes poco confiables) sí reflejan cómo se maneja el problema hoy, aunque todo de forma manual. Lo nuevo que salió y no habíamos anotado antes es que el proceso para notar que algo ya se verificó no es automático todavía, depende de que el usuario lo note por su cuenta en el buscador. Eso es algo que conviene revisar si de verdad queremos que el historial cumpla su propósito de no repetir trabajo.
