---
title: "¿Qué es Vibecoding?"
slug: "que-es-vibecoding"
description: "El vibecoding está redefiniendo cómo se crea software: explicar en lenguaje natural lo que queremos y dejar que la IA escriba el código. ¿Es el futuro o una tendencia pasajera? Descubre sus implicaciones, ventajas y desafíos."
date: "2025-03-04"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
tags: ["calidad-de-software"]
---

[Vibecoding](https://alitu.com/creator/workflow/what-is-vibe-coding/) es una forma emergente de crear software explicando en lenguaje natural cómo queremos que sea, sin preocuparnos por el código que hace funcionar el software.

El término fue acuñado por [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383), quien describe que las inteligencias artificiales están siendo *demasiado buenas*, por lo que puede olvidarse que el código siquiera existe y solo lo lee ocasionalmente.

En esta *metodología* o manera de programar, nos enfocamos en los aspectos técnicos y funcionales que deseamos y abrazamos la potencialidad que tiene la descripción en lenguaje natural.

Esto es un cambio de paradigma en cómo se programó durante décadas.

## ¿Por qué es relevante?

En los últimos meses, [muchas personas](https://x.com/search?q=vibecoding%203.7&src=typed_query) han reportado hacer software de forma muy rápida y en algunos casos sin contar con el conocimiento necesario para hacerlo por sí mismas.

[Juegos en línea](https://x.com/Hesamation/status/1896363229888258363), [simulaciones de vuelo](https://x.com/devazeez/status/1896296088979882019), [aplicaciones completamente funcionales](https://x.com/cjchua21/status/1896238873346506799) son solo algunos de los ejemplos más impresionantes.

Esto ha encendido las alarmas de la comunidad de programación y la ha dividido.

Algunos creen que estas herramientas potenciarán a los expertos, haciéndolos más productivos y revalorizando la industria del software.

Otros piensan que el uso desmedido de inteligencia artificial genera código de baja calidad, desmejora el conocimiento de los programadores acerca del código y puede afectar negativamente el trabajo de los programadores.

## ¿En qué se diferencia este paradigma?

### Delegamos programar

Anteriormente, los programadores buscaban en internet preguntas similares a las que tenían o hacían preguntas en foros (muchos recordarán a StackOverflow).
 
A partir de la información que encontraban, analizaban cómo aplicarlo en su proyecto y escribían código tecla por tecla.
 
En el vibecoding, en cambio, dejamos que la inteligencia artificial llene los espacios vacíos de la descripción que le damos, nos genera código y lo tomamos como viene.

A diferencia de las herramientas de completado como Copilot, en las que el programador escribe parte del código y la IA lo completa, en el vibecoding la IA escribe todo (o prácticamente todo) el código.

### El código no nos importa

La generación de código existe hace décadas. Los compiladores son un gran ejemplo de ello. Gran parte del software moderno, en lenguajes de alto nivel, es una especificación funcional.

En los lenguajes modernos como JavaScript y Python, no nos preocupamos por cómo lucen las estructuras de datos en la memoria.

En Java, la sintaxis de [annotation](https://en.wikipedia.org/wiki/Java_annotation) permite generar código en el momento de la compilación, permitiendo al programador *automatizar* código repetitivo o parametrizable.

Generalmente no nos preocupamos por leer el código ejecutable generado por un compilador, y con el vibecoding sucede lo mismo. Aceptamos el código sin revisarlo.

### Mayor tolerancia a errores

Las herramientas que hemos usado para programar durante décadas no cometen errores, por diseño se busca que sean predecibles y puras.

El compilador de Java siempre generará el mismo código bytecode ejecutable dado el mismo código fuente.

La expectativa del conjunto de herramientas de programación, como los compiladores y linters, es que jamás cometa errores.

Esto no sucede con ninguna aplicación de los LLM (Modelos de Lenguaje de Gran Escala), debido a que son estocásticos, de naturaleza aleatoria e impredecible.

Es por esto que en el vibecoding, la inteligencia artificial puede responder con distinto resultado para el mismo prompt y además puede cometer errores.

Además, esta *metodología* acepta los errores como parte del proceso y le pedimos a la misma inteligencia artificial que, de forma iterativa, los corrija.

## Pros y contras

### Cabida a no expertos

El *vibecoding* permite crear software funcional rápidamente y brinda acceso a personas sin conocimientos técnicos. Nadie puede discutir esto.

Si no tienes conocimientos de programación, serás capaz de crear algo básico pero funcional y tal vez suficiente para tus necesidades. Incluso, tal vez seas capaz de monetizarlo.

### 10 veces más rápido

Si cuentas con conocimientos de computación y programación, serás capaz de hacer software de forma exponencialmente más rápida, y la IA llenará los huecos de conocimiento en los que tienes lo básico.

Por lo tanto, un programador experimentado por sí solo podrá lograr lo que antes lograban varios de ellos en el mismo tiempo.

### Contexto limitado

Aunque los LLM son cada vez [más capaces y baratos](https://www.reddit.com/r/LocalLLaMA/comments/1i5piy1/deepseek_r1_219m_tok_output_vs_o1_60m_tok_insane/), sus limitaciones son por diseño y seguirán presentes.

En primer lugar, la ventana de contexto es inherente a estos modelos, por este motivo los LLM son propensos a cometer errores en bases de código grandes, lo que es común en el software profesional empresarial.

En este sentido, el vibecoding puede ser muy útil en proyectos pequeños y prototipos, pero a medida que el software crece, esta metodología se hará más cuesta arriba e improductiva.

Para superar esta desventaja, los prompts que se le dan a la inteligencia artificial deben ser cada vez más específicos, hasta llegar a un punto en el que los prompts describen el código deseado.

### Calidad inconsistente

Los LLM pueden producir código de baja calidad. Es muy frecuente en el *vibecoding* que los archivos crezcan de forma indefinida, sin [separación de responsabilidades](https://en.wikipedia.org/wiki/Single-responsibility_principle), ni principios como [no te repitas](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

Además, los LLM tienden a cometer más errores cuando parten de código de baja calidad. En contraposición, la IA se beneficia de código de buena calidad, aunque tenga dificultades para generarlo.

La única forma de evitar este inconveniente es refactorizar, cambiar el código a consciencia y de forma asistida por IA. Inevitablemente leerás el código para asegurar la calidad y, por lo tanto, saber leer código fluidamente es una ventaja.

### Poco fiable

Cuando se practica el *vibecoding* es probable que se encuentren errores mediante prueba y error. Una vez que se genera una iteración de software funcionando, se prueba para verificar que se comporta como se espera.

Si la persona no sabe [cómo probar bien](https://www.ibm.com/es-es/topics/software-testing), lo más seguro es que el software contenga errores desconocidos y, en consecuencia, sea un producto poco fiable.

En el simulador de vuelo que creó Peter Lievels vía *vibecoding*, a pesar de que lo monetizó y es jugado por cientos de personas, se le encontró una [vulnerabilidad XSS](https://es.wikipedia.org/wiki/Vulnerabilidad) que permitía al atacante [inyectar código malicioso](https://x.com/levelsio/status/1896210668648612089) en todos los jugadores conectados.

Esto ratifica que el software generado por IA no solo puede cometer errores, sino que puede incluir problemas de seguridad serios.

Para eliminar este riesgo es imperativo tener conocimiento en ciencias de la computación, seguridad informática y diseño de software, debido a que permite articular de forma precisa el software deseado a otras personas y ahora a las máquinas.

Además, deberá complementarse el código funcional con pruebas automatizadas y prácticas avanzadas de testing, para asegurar que el software es fiable y funciona correctamente.

## Conclusiones

### El conocimiento será más clave

El *vibecoding* no es muy diferente del [arte con IA](https://www.adobe.com/ar/products/firefly/discover/what-is-ai-art.html#:~:text=Por%20%E2%80%9Carte%20con%20IA%E2%80%9D%20se,informaci%C3%B3n%20para%20crear%20contenido%20nuevo.)

Una persona sin conocimientos ni experiencia en arte puede crear resultados decentes utilizando herramientas como [Davinci AI](https://davinci.ai/onboarding?utm_source=GoogleAds&utm_medium=cpc&utm_id=21843058525&adgroupid=170577477112&utm_content=720159112044&utm_term=generador%20de%20arte%20con%20ia&gad_source=1&gclid=CjwKCAiA5pq-BhBuEiwAvkzVZcfd7_uPo0-tZKS8ZJiO--zJLVbSp8WcpqMqOcWstcr6Cp15fro24hoC1ccQAvD_BwE) o [Midjourney](https://www.midjourney.com/home).

Sin embargo, una persona con conocimientos específicos en artes plásticas podrá obtener resultados extraordinarios, debido a que podrá articular mejor qué técnicas y referencias desea y utilizará su conocimiento para llenar de contexto el arte.

Lo mismo sucede con el *vibecoding* y herramientas de desarrollo con IA, como [v0](https://v0.dev/), [Cursor](http://cursor.com/) y [Windsurf](https://windsurf.ai/): solo una persona con conocimientos específicos y contexto podrá hacer software fiable y consistente.

Al igual que los artistas se beneficiarán de conocer la historia del arte y técnicas, los programadores se beneficiarán por conocer ciencias de la computación y el negocio.

### Una era de software hiperpersonalizado

El software empresarial seguirá necesitando la fiabilidad, consistencia y seguridad que aporten expertos en la materia.

No obstante, la posibilidad de crear software sin siquiera tener conciencia del código es una realidad, aunque sea para software pequeño y acotado.

Similar a las hojas de cálculo como Excel, en las que cada persona *configura* hojas personalizadas ya sea para llevar las cuentas de su hogar o sus tareas diarias, es probable que suceda exactamente algo similar con el software.

Es probable que en el futuro las personas utilicen *vibecoding* para crear software adaptado a sus necesidades. Ya se han reportado casos de [personas desarrollando software para reemplazar hojas de cálculo](https://x.com/techycarlos/status/1896383106820776418).

### El trabajo en programación será diferente

En definitiva, el desarrollo de software cambiará drásticamente en los próximos años.

Aún es pronto para prever el desenlace, pero creo que la industria se dividirá en dos roles principales, cada uno asociado a un tipo de organización diferente.

Por un lado, las empresas que no producen tecnología como su producto principal podrán apoyarse en profesionales con un conocimiento técnico *suficiente*, complementado con un profundo entendimiento del negocio, para desarrollar productos exitosos.

A este rol se le conoce como [Ingeniero de Producto](https://posthog.com/blog/what-is-a-product-engineer), un término relativamente nuevo pero con [definiciones establecidas](https://productengineer.org/). Gracias a su enfoque en el negocio, este perfil aprovechará mejor el *vibecoding*.

Por otro lado, las empresas que crean tecnología necesitarán expertos con conocimientos profundos en computación e ingeniería para desarrollar productos innovadores.

Este rol seguirá llamándose [Ingeniero de Software](https://en.wikipedia.org/wiki/Software_engineering), y será responsable de construir las bases tecnológicas, desde bases de datos y redes hasta los sistemas que permiten entrenar un LLM.