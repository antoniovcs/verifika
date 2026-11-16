# SC-001: Avisar cuando una verificación ya existe

> Solicitud de cambio · Ingeniería de Software I · SIS3407  
> Solicita: Fabio Perez

---

## Estado

Propuesta · 16 de noviembre de 2026

Propuesta mientras se analiza. Después pasa a Aceptada, Rechazada o Aplazada,
y quien la cambia escribe aquí por qué. El registro nunca se borra.

## Quién la pide

Fabio Perez Gutierrez, en calidad de revisor de requisitos y de la
experiencia de uso.

## Qué se pide

“Cuando alguien pegue una afirmación o un fragmento de código que ya se haya
verificado, queremos que Verifika le avise y le permita abrir el resultado
anterior antes de iniciar otra verificación.”

## Por qué

En la entrevista se identificó que actualmente algunas personas encuentran
resultados previos solo si los buscan manualmente en el historial. Un aviso
antes de repetir el análisis puede ahorrar tiempo y evitar resultados duplicados
que después haya que comparar.

## Qué pasa si no se hace

Las personas pueden volver a enviar contenido ya revisado, repetir el costo y
el tiempo de análisis y generar registros adicionales para una misma duda. La
función básica de verificación sigue disponible, pero se pierde la oportunidad
de reutilizar el trabajo anterior.

## Tipo de mantenimiento

- [ ] **Correctivo** · algo está mal y hay que repararlo
- [ ] **Adaptativo** · el entorno cambió y hay que ajustarse
- [x] **Perfectivo** · funciona, pero puede ser mejor
- [ ] **Preventivo** · todavía no falla, pero va a fallar

Se propone mejorar el flujo de consulta y reutilización del historial; la
verificación normal puede completarse aunque esta mejora no exista.

## Criterio de aceptación

Con un conjunto de prueba que contenga una verificación previa identificada
como igual o similar, al enviar de nuevo ese contenido, Verifika muestra el
resultado previo antes de iniciar otra verificación y permite abrirlo. Para un
contenido sin coincidencia en ese conjunto, Verifika permite iniciar una nueva
verificación. **Criterio satisfecho: sí/no.**

## Prioridad que sugiere el solicitante

**Media.** Evita repetir análisis y facilita encontrar resultados anteriores,
pero no bloquea la función principal si se implementa después.

---

*La solicitud describe qué se quiere, nunca cómo hacerlo. El cómo se decide en el análisis de impacto, y lo decide quien conoce el diseño.*
