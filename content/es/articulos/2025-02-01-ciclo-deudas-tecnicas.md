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

Funcionalidades incompletas o soluciones provisionales pueden ser aceptables cuando se requiere eliminar riesgos mayores, como la incertidumbre sobre la viabilidad del producto.

## ¿Qué riesgos tiene?

Sin embargo, la deuda técnica no es un recurso inagotable y debe manejarse con cuidado para evitar que se convierta en un obstáculo en el desarrollo del producto y la organización.

La deuda técnica tiene un límite debido a su naturaleza de rendimientos decrecientes.

A medida que se acumula, el equipo necesita cada vez más esfuerzo para desarrollar nuevas funcionalidades, ya que la complejidad y el desorden del código aumentan. Esto puede llevar a que el management asuma que la solución es aumentar el tamaño del equipo, lo que no solo incrementa los costos, sino que en algunos casos puede hacer que el desarrollo sea aún más lento, como se describe en The [Mythical Man-Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month).

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

Si quieres romper con este ciclo y recuperar la salud del equipo y del producto, aquí tienes algunas estrategias clave que he visto funcionar en diferentes equipos:

### Dedica tiempo a investigar

No subestimes el valor de la investigación rápida y eficiente. Los equipos que reservan tiempo para entender los problemas toman mejores decisiones. No se trata de detener todo el desarrollo por semanas, sino de integrar análisis rápidos en el flujo de trabajo. Una hora de investigación hoy puede ahorrar días de retrabajo en el futuro.

### Promueve calidad estratégicamente

No es necesario perseguir la perfección en cada línea de código, pero sí definir una calidad mínima aceptable y respetarla. Aceptar concesiones en la calidad  puede ser útil para acelerar entregas, pero es fundamental planificar cómo y cuándo abordar esos puntos más adelante. Esto requiere compromiso tanto del equipo como del management.

### Fomenta la autonomía y el feedback

Los equipos autónomos son más propensos a salir del ciclo de deuda técnica porque toman decisiones con criterio y no por imposición. La clave está en equilibrar esta autonomía con un sistema de feedback continuo que permita mejorar sin caer en controles excesivos.

### Contrata con estándares altos

Cada nueva persona en el equipo debe entender que su rol no es solo escribir más código, sino contribuir a la mejora continua del sistema. Ingenieros con experiencia en mantener estándares altos de calidad pueden ayudar a transformar la cultura del equipo y evitar recaer en malas prácticas.

## Conclusión

La deuda técnica puede ser una herramienta útil cuando se usa con estrategia, pero si se convierte en un hábito, puede hundir a un equipo en un ciclo de ineficiencia, desmotivación y altos costos operativos.

Para escapar de este ciclo, es clave priorizar la investigación, definir estándares de calidad realistas, fomentar la autonomía del equipo y contratar con altos estándares. Además, desafiar prácticas comunes como las fechas artificiales de entrega puede ayudar a recuperar la agilidad y moral del equipo.

La deuda técnica debe ser una decisión consciente, no una trampa en la que se cae por falta de planificación.

En un próximo artículo abordaré por qué mantener la deuda técnica bajo control es una condicion necesaria pero no suficiente para lograr un equipo ágil y productivo.

## Referencias

- [Technical Debt](https://martinfowler.com/bliki/TechnicalDebt.html)
- [The Mythical Man-Month](https://en.wikipedia.org/wiki/The_Mythical_Man-Month)
- [PostHog](https://posthog.com/blog/posthog-engineering-culture)
- [Technical Debt Quadrant](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html)
