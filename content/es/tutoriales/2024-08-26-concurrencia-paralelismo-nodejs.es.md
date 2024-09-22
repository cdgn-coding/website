---
title: "Concurrencia y paralelismo en Node.js"
slug: "concurrencia-paralelismo-nodejs"
date: "2024-08-22"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "En este artículo aprenderás como usar Node.js para ejecutar procesos intensos y distribuir la carga usando Worker Threads para aprovechar el CPU de forma efectiva. Además, entenderás como funciona la API de Worker Threads sin librerías y así ganarás un conocimiento más profundo sobre Node.js."
tags: ["node.js", "javascript", "guias"]
---

En este artículo aprenderás como usar Node.js para ejecutar procesos intensos y distribuir la carga usando Worker Threads, una funcionalidad de esta plataforma que te permite aprovechar el CPU de forma efectiva. Además, entenderás como funciona la API de Worker Threads sin librerías y así ganarás un conocimiento más profundo sobre Node.js.

## Introducción

Node.js es una plataforma para ejecutar Javascript y se basa en un solo hilo, es decir, aunque las operaciones son asincrónicas, el código de los programas se ejecuta en un sólo hilo.

El procesamiento multi-hilo que realiza Node.js se centra en las operaciones de entrada/salida como peticiones de red y lectura de archivos. Esta orquestación se logra con el [Event Loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick) y es transparente para el programador.

## Por qué es importante

Si un programa realiza operaciones bloqueantes, se pueden crear cuellos de botella que impidan la ejecución de todas las operaciones. De aquí nace el mantra [no bloquees el Event Loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop).

En las aplicaciones modernas el procesamiento de datos es de vital importancia, con el uso de concurrencia y paralelismo se gana más control para lograr un performance óptimo.

Dentro de los casos más comunes están el procesamiento de archivos multimedia, validar la integridad de archivos y la ejecución de algoritmos.

## Uso básico

Veamos primero como se puede lanzar un hilo que realiza la pesada operación de contar hasta el trillón, sin bloquear el hilo principal, utilizando worker threads.

{{< highlight javascript "linenos=table" >}}
const { Worker, isMainThread, parentPort, workerData } = require('node:worker_threads');
const { performance } = require('perf_hooks');

if (isMainThread) {
  // This is the main thread

  // Use the same file as the worker
  // Pass some data to the worker
  const workerThread = new Worker(__filename, {
    workerData: {
      countTo: 1000000000 
    }
  });

  // Setup event listener to receive messages from the worker
  workerThread.on('message', (message) => {
    console.log(timeOfExecution(), 'Message from worker:', message);
  });

  console.log(timeOfExecution(), 'Main thread can still do other work');
} else {
  // This is the worker thread
  console.log(timeOfExecution(), 'Worker thread is running with', workerData);

  // Get the data passed to the worker
  const { countTo } = workerData;
  console.log(timeOfExecution(), 'Worker thread will count to:', countTo);

  // Do some heavy work
  for (let i = 0; i < countTo; i++) {}

  // Send a message to the main thread
  parentPort.postMessage('done');
}

function timeOfExecution() {
  const ms = performance.now().toFixed(2);
  return `${ms}ms`;
}
{{< / highlight >}}

Al ejecutar este programa, obtendrás la siguiente salida por consola

```
44.54ms Main thread can still do other work
82.85ms Worker thread is running with { countTo: 1000000000 }
644.04ms Message from worker: done
85.06ms Worker thread will count to: 1000000000
```

Aquí ya podemos notar las características principales de los hilos trabajadores

1. La plataforma se encarga de juntar la salida estándar de los hilos.
2. El hilo principal queda libre durante durante la ejecución del hilo trabajador.
3. Es posible pasar variables a los hilos trabajadores.

## Patrón productor-consumidor

Un patrón muy común en las aplicaciones es tener un hilo produciendo datos y otro procesándolos. A este patrón le llamamos patrón productor-consumidor y es muy útil para separar responsabilidades y para realizar un procesamiento eficiente.

En este ejemplo tendremos un hilo que produce valores y otro hilo que los procesa más lentamente.

En el hilo principal, se configuran los trabajadores y se muestran los tiempos en los que se producen y consumen los valores.

{{< highlight javascript "linenos=table" >}}
const { Worker } = require('node:worker_threads');
const { timeOfExecution } = require("./timeOfExecution");

// Launch producer and consumer workers
const producer = new Worker('./producer.js');
const consumer = new Worker('./consumer.js');

// Producer sends data to consumer
producer.on('message', (message) => {
  if (message.type === 'data') {
    console.log(timeOfExecution(), "Produced:", message.data);

    consumer.postMessage({
      type: 'data',
      data: message.data,
    });
  }

  if (message.type === 'end') {
    consumer.postMessage({
      type: 'end',
    });
  }
})

// Consumer receives data from producer
consumer.on('message', (message) => {
  console.log(timeOfExecution(), "Consumed:", message.data);
});

// Show time of exit of each worker
producer.on('exit', () => {
  console.log(timeOfExecution(), "Producer exited");
});

consumer.on('exit', () => {
  console.log(timeOfExecution(), "Consumer exited");
});

// Show time of exit of main process
process.on('exit', () => {
  console.log(timeOfExecution(), "Main process exited");
});
{{< / highlight >}}

En el productor envía datos al hilo padre simulando un trabajo pesado que toma segundo y medio.

{{< highlight javascript "linenos=table" >}}
const { parentPort } = require('worker_threads');

for (let i = 0; i < 5; i++) {
  // Simulate some heavy work that takes 1.5 seconds
  const now = Date.now();
  while (Date.now() - now < 1500) {}

  // Produce data to be consumed by consumer worker
  parentPort.postMessage({
    type: 'data',
    data: i.toString(),
  });
}

// Send end message to main thread
parentPort.postMessage({
  type: 'end',
});
{{< / highlight >}}

Por último, el consumidor lee los datos a través del canal de mensajes y simula un trabajo de cómputo de un segundo.

{{< highlight javascript "linenos=table" >}}
const { parentPort } = require("worker_threads");

function expensiveComputation(n) {
  // Simulate some heavy work that takes 1 second
  const now = Date.now();
  while (Date.now() - now < 1000) {}
  return n;
}

async function main() {
  // Queue to store data
  let queue = [];
  let finished = false;

  // Consumer receives data from producer
  parentPort.on("message", (message) => {
    if (message.type === "data") {
      queue.push(message);
    }

    if (message.type === "end") {
      finished = true;
    }
  });

  // While loop to consume data
  while (true) {
    // If finished and queue is empty, break the loop
    if (finished && queue.length === 0) {
      break;
    }

    // If queue is empty, wait for 1 second before checking again
    if (queue.length === 0) {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      continue;
    }

    // Consume data, one at a time
    const message = queue.shift();
    const result = expensiveComputation(message.data);
    parentPort.postMessage({ type: "data", data: result });
  }
}

// Start the consumer
// When the consumer is done, exit the process
main().then(() => {
  process.exit();
});
{{< / highlight >}}

Si ejecutamos este programa, encontraremos que su tiempo de ejecución es considerablemente menor respecto de ejecutar el mismo algoritmo de forma sincrónica. Recuerda que este es un ejemplo que puedes adaptar a las necesidades de procesamiento de tu aplicación.


## Patrón de canales de mensajería

En el ejemplo anterior vimos como es podemos separar la producción y consumo de datos, sin embargo, el programa principal tiene la complejidad de orquestar el paso de mensajes entre hilos.

Afortunadamente, Node.js implementa la funcionalidad de [MessageChannel](https://nodejs.org/api/worker_threads.html#class-messagechannel), el cual permite la comunicación bidireccional entre dos hilos. Este patrón puede simplificar significativamente la implementación del patrón productor-consumidor.

En el programa principal, crearemos un canal que nos dará dos puertos o puntos de comunicación, uno de ellos se le pasará al productor y el otro al consumidor. Esto resultará en que ambos hilos pueden comunicarse entre sí.

{{< highlight javascript "linenos=table" >}}
const { Worker, MessageChannel } = require('node:worker_threads');
const { timeOfExecution } = require("./timeOfExecution");

const channel = new MessageChannel();

// Launch producer and consumer workers
const producer = new Worker('./producer.js', {
  workerData: { port: channel.port1 },
  transferList: [channel.port1]
});

const consumer = new Worker('./consumer.js', {
  workerData: { port: channel.port2 },
  transferList: [channel.port2]
});

// Consumer receives data from producer
consumer.on('message', (message) => {
  console.log(timeOfExecution(), "Consumed:", message.data);
});

// Show time of exit of each worker
producer.on('exit', () => {
  console.log(timeOfExecution(), "Producer exited");
});

consumer.on('exit', () => {
  console.log(timeOfExecution(), "Consumer exited");
});

// Show time of exit of main process
process.on('exit', () => {
  console.log(timeOfExecution(), "Main process exited");
});
{{< / highlight >}}

Tanto en el productor como en el consumidor, se tomará el puerto de comunicación desde los datos que inicializan el hilo.

{{< highlight javascript "linenos=table" >}}
const { workerData } = require('worker_threads');

const { port } = workerData;

for (let i = 0; i < 5; i++) {
  // Simulate some heavy work that takes 1.5 seconds
  const now = Date.now();
  while (Date.now() - now < 1500) {}

  // Produce data to be consumed by consumer worker
  port.postMessage({
    type: 'data',
    data: i.toString(),
  });
}

// Send end message to the given port
port.postMessage({
  type: 'end',
});

{{< / highlight >}}

Por último, desde el consumidor, reportamos los datos calculados de nuestra simulación de cómputo intensivo al puerto del hilo padre.

{{< highlight javascript "linenos=table" >}}
const { parentPort, workerData } = require("worker_threads");
const { port } = workerData;

function expensiveComputation(n) {
  // Simulate some heavy work that takes 1 second
  const now = Date.now();
  while (Date.now() - now < 1000) {}
  return n;
}

async function main() {
  // Queue to store data
  let queue = [];
  let finished = false;

  // Consumer receives data from producer
  port.on("message", (message) => {
    if (message.type === "data") {
      queue.push(message);
    }

    if (message.type === "end") {
      finished = true;
    }
  });

  // While loop to consume data
  while (true) {
    // If finished and queue is empty, break the loop
    if (finished && queue.length === 0) {
      break;
    }

    // If queue is empty, wait for 1 second before checking again
    if (queue.length === 0) {
      await new Promise((resolve) => setTimeout(resolve, 1000));
      continue;
    }

    // Consume data, one at a time
    const message = queue.shift();
    const result = expensiveComputation(message.data);
    parentPort.postMessage({ type: "data", data: result });
  }
}

// Start the consumer
// When the consumer is done, exit the process
main().then(() => {
  process.exit();
});
{{< / highlight >}}

## Patrón de pool de trabajadores

Crear hilos cada vez que se va a realizar una tarea es costoso a nivel sistema operativo, por lo cual lo que suelen hacer las aplicaciones más eficientes es crear un conjunto de hilos y reutilizarlos.

En este ejemplo implementaremos el patrón de pool de trabajadores, el cual consiste de un grupo de hilos que están listos para procesar datos en paralelo. Para esto, refactorizaremos la implementación anterior para que haya 2 consumidores procesando datos al mismo tiempo.

Primero, crearemos una clase Pool la cual manejará los trabajadores y orquestará la comunicación en el hilo principal.

{{< highlight javascript "linenos=table" >}}
const { Worker,} = require('node:worker_threads');

class Pool {
  constructor(workerPath, size, port) {
    this.workerPath = workerPath;
    this.size = size;
    this.workers = [];
    this.port = port;

    // Create workers
    for (let i = 0; i < size; i++) {
      this.workers.push(new Worker(workerPath));
    }

    // Listen for messages in the port
    // and distribute them to workers
    this.port.on('message', message => {
      if (message.type === 'data') {
        this.postMessage(message);
      }
      
      if (message.type === 'end') {
        this.broadcast(message);
      }
    });
  }

  postMessage(message) {
    // Round-robin distribution
    const worker = this.workers.shift();
    worker.postMessage(message);
    this.workers.push(worker);
  }

  on(event, callback) {
    // Register callback for all workers
    this.workers.forEach(worker => worker.on(event, callback));
  }

  broadcast(message) {
    // Broadcast message to all workers
    this.workers.forEach(worker => worker.postMessage(message));
  }

  terminate() {
    // Terminate all workers
    this.workers.forEach(worker => worker.terminate());
  }
}

module.exports = { Pool }
{{< / highlight >}}

Ahora, reconfiguremos el consumer para que se comunique con el hilo principal, el cual está a cargo del pool, y no del programa principal.

{{< highlight javascript "linenos=table" >}}
const { Pool } = require('./pool');
const { Worker, MessageChannel } = require('node:worker_threads');
const { timeOfExecution } = require("./timeOfExecution");

const channel = new MessageChannel();

// Launch producer and consumer workers
const producer = new Worker('./producer.js', {
  workerData: { port: channel.port1 },
  transferList: [channel.port1]
});

const consumerPool = new Pool('./consumer.js', 5, channel.port2);

// Consumer receives data from producer
consumerPool.on('message', (message) => {
  console.log(timeOfExecution(), "Consumed:", message.data);
});
{{< / highlight >}}

## Acceso a recursos compartidos

Los hilos, además de ejecutarse en paralelo, pueden compartir memoria dentro del mismo proceso. De hecho, esta es una de las ventajas de usar Worker Threads en lugar de Child Process.

Esta funcionalidad está disponible mediante la API de SharedArrayBuffer y Atomics. El primero nos permite reservar un espacio en la memoria, que será compartido entre distintos hijos trabajadores. El segundo habilita acceder de forma segura a ese espacio de memoria.

Veamos un ejemplo de un contador atómico.

{{< highlight javascript "linenos=table" >}}
// Create a SharedArrayBuffer with a size in bytes
const sharedBuffer = new Int32Array(new SharedArrayBuffer(Int32Array.BYTES_PER_ELEMENT));
sharedBuffer[0] = 0;

// Prints 0
console.log(Atomics.load(sharedBuffer, 0));

// Add 1 to the shared buffer
Atomics.add(sharedBuffer, 0, 1)

// Prints 1
console.log(Atomics.load(sharedBuffer, 0));

// Subtract 1 from the shared buffer
Atomics.sub(sharedBuffer, 0, 1)

// Prints 0
console.log(Atomics.load(sharedBuffer, 0));
{{< / highlight >}}

En los ejemplos anteriores, el consumidor hace polling a intervalos de un segundo para tomar trabajos pendientes y completarlos. Esta implementación, aunque intuitiva, es menos eficiente. 

Una forma de mejorar la implementación anterior, es compartir un espacio de memoria entre los hilos que cuente los trabajos pendientes. De esta manera, los hilos trabajadores se activarán apenas tengan una tarea que deban realizar.

Refactorizaremos el Pool para que envíe a los trabajadores el espacio de memoria compartida y un nuevo canal donde recibirán los mensajes.

{{< highlight javascript "linenos=table" >}}
const { Worker, MessageChannel } = require('node:worker_threads');

class Pool {
  constructor(workerPath, size, port) {
    this.workerPath = workerPath;
    this.size = size;
    this.workers = [];
    this.port = port;

    // Create workers
    for (let i = 0; i < size; i++) {
      const {
        port1: poolChannel,
        port2: jobChannel
      } = new MessageChannel();

      // Create shared buffer for worker
      const sharedBuffer = new Int32Array(new SharedArrayBuffer(Int32Array.BYTES_PER_ELEMENT));

      // Initialize atomic counter
      sharedBuffer[0] = 0;

      // Create worker
      const worker = new Worker(workerPath);

      // Store worker, shared buffer and channel
      this.workers.push({
        worker,
        sharedBuffer,
        poolChannel,
        jobChannel,
      });

      // Send startup message to worker
      worker.postMessage({
        type: 'startup',
        payload: {
          sharedBuffer,
          jobChannel,
        }
      }, [jobChannel]);
    }

    
    // Listen for messages in the port
    // and distribute them to workers
    this.port.on('message', message => {
      if (message.type === 'data') {
        this.postMessage(message);
      }
      
      if (message.type === 'end') {
        this.broadcast(message);
      }
    });
  }

  postMessage(message) {
    // Round-robin distribution
    const worker = this.workers.shift();

    // Post message to worker
    worker.poolChannel.postMessage(message);

    // Increment atomic counter and notify worker
    Atomics.add(worker.sharedBuffer, 0, 1)
    Atomics.notify(worker.sharedBuffer, 0);

    // Add worker back to pool
    this.workers.push(worker);
  }

  on(event, callback) {
    // Register callback for all workers
    this.workers.forEach(worker => {
      worker.poolChannel.on(event, callback)
    });
  }

  broadcast(message) {
    // Broadcast message to all workers
    this.workers.forEach(worker => worker.poolChannel.postMessage(message));
  }

  terminate() {
    // Terminate all workers
    this.workers.forEach(worker => worker.worker.terminate());
  }
}

module.exports = { Pool }
{{< / highlight >}}

Ahora, reimplementaremos el consumidor para que, en lugar de tomar trabajos pendientes a intervalos, simplemente espere a que el contador de trabajos pendientes sea distinto de cero.

{{< highlight javascript "linenos=table" >}}
const { parentPort, receiveMessageOnPort } = require("worker_threads");

function expensiveComputation(n) {
  // Simulate some heavy work that takes 1 second
  const now = Date.now();
  while (Date.now() - now < 1000) {}
  return n;
}

parentPort.on("message", (message) => {
  if (message.type !== "startup") {
    return;
  }

  // First message received is the buffer
  const { jobChannel, sharedBuffer } = message.payload;

  // Start the consumer
  // When the consumer is done, exit the process
  main(sharedBuffer, jobChannel).then(() => {
    process.exit();
  });
});

async function main(sharedBuffer, jobChannel) {
  let finished = false;

  // While loop to consume data
  while (!finished || Atomics.wait(sharedBuffer, 0, 0) === "ok") {
    // Wait for the producer to produce data

    // Consume data
    let entry;
    while ((entry = receiveMessageOnPort(jobChannel)) !== undefined) {
      // Consume data, one at a time
      processMessage(entry.message);

      // Notify the producer that the data has been consumed
      Atomics.sub(sharedBuffer, 0, 1);
    }
  }

  function processMessage(message) {
    if (message.type === "data") {
      // Do the heavy computation and send the result back
      const result = expensiveComputation(message.data);
      jobChannel.postMessage({ type: "data", data: result });
    }

    if (message.type === "end") {
      // End of data reached
      finished = true;
    }
  }
}
{{< / highlight >}}

El resto de las partes queda intacta, significando un refactor transparente para el productor y el programa principal.


## Librerías de la comunidad

Implementar tus propios patrones sirve a tu conocimiento de ingeniería de software y programación. Según tus necesidades, puede ser conveniente usar librerías de la comunidad.

Una de las más recomendables es [Piscina](https://snyk.io/blog/node-js-multithreading-with-worker-threads/), la cual implementa el patrón pool de trabajadores con algunas características muy interesantes:

- Permite reutilizar un mismo pool de trabajadores para distintas tareas.
- Envía las tareas al primer trabajador disponible.
- Canaliza los resultados de las tareas de forma automática.

## Conclusiones

En este artículo aprendiste cómo hacer concurrencia y paralelismo en Javascript con Node.js. Ahora sabes como utilizar hilos trabajadores, canales de mensajería y operaciones atómicas.

Además, sabes como implementar patrones de programación concurrente en tus aplicaciones.

Te invito a pensar qué procesos en tu aplicación pueden apalancarse de esta funcionalidad para dar una mejor experiencia de usuario. Si tu aplicación realiza tareas grandes y complejas, sacarlas del hilo principal mejorará significativamente el performance.

Aunque lo visto en este artículo sea suficiente para mejorar el rendimiento en muchos casos, si tu proyecto depende en gran medida de procesamiento concurrente, te recomiendo aplicar lenguajes más especializados para este fin, como lo pueden ser Go o Java.
