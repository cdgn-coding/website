---
title: "Ciclo de deudas técnicas"
slug: "ciclo-deudas-tecnicas"
date: "2025-02-01"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
tags: ["calidad-software"]
---

## ¿Qué es la deuda técnica?

La deuda técnica es el costo futuro de tomar atajos en la calidad del código para acelerar la entrega de un producto. Tomar deuda técnica es una decisión intencional o accidental que implica comprometer la estructura del software con el fin de obtener un beneficio inmediato, a expensas de costos adicionales en el futuro.

## ¿Cómo puede ayudar a un equipo?

Tomar decisiones rápidas y asumir cierta deuda técnica puede ser una estrategia efectiva cuando se busca validar un modelo de negocio o acelerar la llegada al mercado. En estos casos, reducir el tiempo de entrega puede marcar la diferencia entre captar una oportunidad o perderla.

Las funcionalidades incompletas o soluciones provisionales pueden ser aceptables cuando se requiere eliminar riesgos mayores, como la propia viabilidad y deseabilidad del producto.

## ¿Qué riesgos tiene?

Sin embargo, la deuda técnica no es un recurso inagotable y debe manejarse con cuidado para evitar que se convierta en un obstáculo en el desarrollo del producto y la organización.

La deuda técnica tiene un límite debido a su naturaleza de [rendimientos decrecientes](https://martinfowler.com/articles/is-quality-worth-cost.html). A medida que se acumula, el equipo necesita cada vez más esfuerzo para desarrollar nuevas funcionalidades, ya que la complejidad y el desorden del código aumentan.

Esto puede llevar a que el management asuma que la solución es aumentar el tamaño del equipo, lo que no solo incrementa los costos, sino que en algunos casos puede hacer que el desarrollo sea aún más lento, como se describe en The [Mythical Man-Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month).

Las empresas dependen de tiempos de entrega para coordinar equipos, alinear expectativas con inversores y garantizar el cumplimiento de compromisos con clientes. No obstante, si un equipo tiene un codebase con deuda técnica y además baja calidad de código, los tiempos de entrega pueden terminar perpetuando el problema, convirtiéndose en un ciclo vicioso.

## El ciclo de deuda técnica

Cuando un equipo enfrenta fechas límite ajustadas y un codebase deteriorado, se ve obligado a tomar más atajos técnicos para cumplir con los plazos. Pero, a medida que la deuda técnica se acumula, el esfuerzo necesario para implementar nuevas funcionalidades crece exponencialmente. Como consecuencia, los plazos no se cumplen y el equipo entra en un ciclo:

- Se toman atajos para cumplir con la entrega.
- El codebase empeora y el equipo pierde agilidad.
- La siguiente entrega requiere aún más atajos.
- Se mantiene el mismo esfuerzo, pero con expectativas irreales.
- El ciclo se repite

Como consecuencia, los desarrolladores trabajan horas extras solo para cumplir fechas artificiales. Seguidamente, la moral del equipo disminuye y el cinismo aumenta (“en esta compañía se trabaja así”). Por último, los mejores colaboradores se van, lo que agrava aún más el problema.

Si no se rompe este ciclo, la empresa se arriesga a perder talento, tiempo y dinero.

## Cómo escapar del ciclo de deuda técnica

Si quieres romper con este ciclo y recuperar la salud del equipo y del producto, aquí tienes algunas estrategias clave que he visto funcionar en diferentes equipos.

### Aborda la situación sin culpas

No culpes a los desarrolladores por la deuda técnica; en su lugar, enfócate en las soluciones. La deuda técnica no es intrínsecamente mala: los atajos tomados permitieron a la empresa avanzar y conseguir resultados. En lugar de señalar errores del pasado, el equipo debe enfocarse en estrategias para manejar la deuda técnica de manera sostenible.

Esto ayuda a cambiar la narrativa de *deuda técnica = fracaso* a *deuda técnica = herramienta estratégica*, promoviendo un enfoque más colaborativo y menos reactivo.

Un ejemplo de cómo abordar la deuda técnica sin culpas es [Amazon Prime Video](https://archive.is/ehJbY). Inicialmente adoptaron una arquitectura microservicios serverless pero con el tiempo se volvió costosa. En lugar de culpar la decisión original, analizaron el problema y migraron a un monolito, reduciendo costos en un 90%.

Esto demuestra que la deuda técnica no es un error, sino una consecuencia natural del crecimiento, y que lo importante es saber cuándo y cómo gestionarla.

### Dedica tiempo a investigar

No subestimes el valor de la investigación rápida y eficiente.

Los equipos que reservan tiempo para entender los problemas toman mejores decisiones. No se trata de detener todo el desarrollo por semanas, sino de integrar análisis rápidos en el flujo de trabajo. Una hora de investigación hoy puede ahorrar días de retrabajo en el futuro.

Un gran ejemplo de la importancia de la investigación es [Discord](https://discord.com/blog/how-discord-stores-trillions-of-messages). El equipo técnico identificó problemas de latencia y escalabilidad a medida que su base de datos crecía. Luego de analizar e investigar distintas opciones, decidieron migrar a ScyllaDB, lo que les permitió mantener la eficiencia sin comprometer el rendimiento.

Invertir tiempo en entender los problemas actuales y anticipar futuros cuellos de botella puede ahorrar costos y mejorar la estabilidad del producto.

### Promueve calidad estratégicamente

No es necesario perseguir la perfección en cada línea de código, pero sí definir una calidad mínima aceptable y respetarla. Un [estudio](https://arxiv.org/abs/2401.13407) mostró que la calidad del código permite a los equipos entregar valor cada vez más rapido.

Aceptar concesiones en la calidad  puede ser útil para acelerar entregas, pero es fundamental planificar cómo y cuándo abordar esos puntos más adelante. Esto requiere compromiso tanto del equipo como del management.

### Fomenta la autonomía y el feedback

Los equipos autónomos son más propensos a salir del ciclo de deuda técnica porque toman decisiones con criterio y no por imposición.

La clave está en equilibrar esta autonomía con un sistema de feedback continuo que permita mejorar sin caer en controles excesivos.

### Contrata con estándares altos

Cada nueva persona en el equipo debe entender que su rol no es solo escribir más código, sino contribuir a la mejora continua del sistema.

Ingenieros con experiencia en mantener estándares altos de calidad pueden ayudar a transformar la cultura del equipo y evitar recaer en malas prácticas.

[PostHog](https://newsletter.posthog.com/p/the-deadline-doom-loop) es un gran ejemplo, su equipo se conforma por ex-CTOs, ex-Founders e ingenieros con un amplio conocimiento técnico y habilidades blandas. Como consecuencia, un equipo es capaz de tomar decisiones y entregar valor más rapido que un equipo más grande pero con menos experiencia.

## Conclusión

La deuda técnica puede ser una herramienta útil cuando se usa con estrategia, pero si se convierte en un hábito, puede hundir a un equipo en un ciclo de ineficiencia, desmotivación y altos costos operativos.

Para escapar de este ciclo, es clave priorizar la investigación, definir estándares de calidad realistas, fomentar la autonomía del equipo y contratar con altos estándares. Además, desafiar prácticas comunes como las fechas artificiales de entrega puede ayudar a recuperar la agilidad y moral del equipo.

La deuda técnica debe ser una decisión consciente, no una trampa en la que se cae por falta de planificación.

En un próximo artículo abordaré por qué controlar la deuda técnica es necesario, pero no suficiente, para lograr un equipo ágil y productivo.

## Referencias

- [Technical Debt](https://martinfowler.com/bliki/TechnicalDebt.html)
- [The Mythical Man-Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month)
- [The Deadline Doom Loop](https://newsletter.posthog.com/p/the-deadline-doom-loop?utm_source=tldrfounders)
- [Technical Debt Quadrant](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html)
- [Is Quality Worth the Cost?](https://martinfowler.com/articles/is-quality-worth-cost.html)
- [Increasing, not Diminishing: Investigating the Returns of Highly Maintainable Code](https://arxiv.org/abs/2401.13407)
- [Scaling up the Prime Video audio/video monitoring service and reducing costs by 90%](https://archive.is/ehJbY)
- [How Discord stores trillions of messages](https://discord.com/blog/how-discord-stores-trillions-of-messages)