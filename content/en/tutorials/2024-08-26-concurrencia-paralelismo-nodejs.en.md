---
title: "Concurrency and Parallelism in Node.js"
slug: "concurrency-parallelism-nodejs"
date: "2024-08-22"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "In this article, you'll learn how to use Node.js to execute intensive processes and distribute the load using Worker Threads to effectively utilize the CPU. Additionally, you'll understand how the Worker Threads API works without libraries, thus gaining deeper knowledge about Node.js."
tags: ["node.js", "javascript", "guides"]
---

Here's the translated version of the article in English, formatted in Markdown:

# Using Worker Threads in Node.js for Efficient CPU Utilization

In this article, you'll learn how to use Node.js to execute intensive processes and distribute the load using Worker Threads, a feature of this platform that allows you to effectively utilize the CPU. Additionally, you'll understand how the Worker Threads API works without libraries, thus gaining deeper knowledge about Node.js.

## Introduction

Node.js is a platform for executing JavaScript and is based on a single thread, meaning that although operations are asynchronous, the code of the programs runs on a single thread.

The multi-threaded processing that Node.js performs focuses on input/output operations such as network requests and file reading. This orchestration is achieved with the [Event Loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick) and is transparent to the programmer.

## Why It's Important

If a program performs blocking operations, bottlenecks can be created that prevent the execution of all operations. From here comes the mantra [don't block the Event Loop](https://nodejs.org/en/learn/asynchronous-work/dont-block-the-event-loop).

In modern applications, data processing is of vital importance, and with the use of concurrency and parallelism, more control is gained to achieve optimal performance.

Among the most common cases are multimedia file processing, validating file integrity, and algorithm execution.

## Basic Usage

Let's first see how we can launch a thread that performs the heavy operation of counting to a trillion, without blocking the main thread, using worker threads.

```javascript
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
```

When you run this program, you'll get the following console output:

```
44.54ms Main thread can still do other work
82.85ms Worker thread is running with { countTo: 1000000000 }
644.04ms Message from worker: done
85.06ms Worker thread will count to: 1000000000
```

Here we can already notice the main characteristics of worker threads:

1. The platform takes care of combining the standard output of the threads.
2. The main thread remains free during the execution of the worker thread.
3. It's possible to pass variables to worker threads.

## Producer-Consumer Pattern

A very common pattern in applications is to have one thread producing data and another processing it. We call this pattern the producer-consumer pattern, and it's very useful for separating responsibilities and for efficient processing.

In this example, we'll have one thread that produces values and another thread that processes them more slowly.

In the main thread, the workers are configured and the times at which values are produced and consumed are displayed.

```javascript
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
```

In the producer, data is sent to the parent thread simulating heavy work that takes one and a half seconds.

```javascript
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
```

Finally, the consumer reads the data through the message channel and simulates a computation job of one second.

```javascript
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
```

If we run this program, we'll find that its execution time is considerably less compared to executing the same algorithm synchronously. Remember that this is an example that you can adapt to the processing needs of your application.

## Message Channel Pattern

In the previous example, we saw how we can separate the production and consumption of data, however, the main program has the complexity of orchestrating the passing of messages between threads.

Fortunately, Node.js implements the functionality of [MessageChannel](https://nodejs.org/api/worker_threads.html#class-messagechannel), which allows bidirectional communication between two threads. This pattern can significantly simplify the implementation of the producer-consumer pattern.

In the main program, we'll create a channel that will give us two ports or communication points, one of which will be passed to the producer and the other to the consumer. This will result in both threads being able to communicate with each other.

```javascript
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
```

Both in the producer and the consumer, the communication port will be taken from the data that initializes the thread.

```javascript
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
```

Finally, from the consumer, we report the calculated data from our intensive computation simulation to the parent thread's port.

```javascript
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
```

## Worker Pool Pattern

Creating threads every time a task is to be performed is expensive at the operating system level, so what more efficient applications usually do is create a set of threads and reuse them.

In this example, we'll implement the worker pool pattern, which consists of a group of threads that are ready to process data in parallel. For this, we'll refactor the previous implementation so that there are 2 consumers processing data at the same time.

First, we'll create a Pool class which will manage the workers and orchestrate communication in the main thread.

```javascript
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
```

Now, let's reconfigure the consumer to communicate with the main thread, which is in charge of the pool, and not with the main program.

```javascript
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
```

## Access to Shared Resources

Threads, in addition to running in parallel, can share memory within the same process. In fact, this is one of the advantages of using Worker Threads instead of Child Process.

This functionality is available through the SharedArrayBuffer and Atomics API. The first allows us to reserve a space in memory, which will be shared between different worker children. The second enables safe access to that memory space.

Let's see an example of an atomic counter.

```javascript
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
```

In the previous examples, the consumer polls at one-second intervals to take pending jobs and complete them. This implementation, although intuitive, is less efficient.

One way to improve the previous implementation is to share a memory space between threads that counts pending jobs. In this way, the worker threads will activate as soon as they have a task they need to perform.

We'll refactor the Pool to send workers the shared memory space and a new channel where they will receive messages.

```javascript
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
```

Now, let's reimplement the consumer so that, instead of taking pending jobs at intervals, it simply waits for the pending job counter to be different from zero.

```javascript
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
```

The rest of the parts remain intact, meaning a transparent refactor for the producer and the main program.

## Community Libraries

Implementing your own patterns serves your knowledge of software engineering and programming. Depending on your needs, it may be convenient to use community libraries.

One of the most recommendable is [Piscina](https://snyk.io/blog/node-js-multithreading-with-worker-threads/), which implements the worker pool pattern with some very interesting features:

- It allows reusing the same pool of workers for different tasks.
- It sends tasks to the first available worker.
- It automatically channels the results of tasks.

## Conclusions

In this article, you learned how to do concurrency and parallelism in Javascript with Node.js. Now you know how to use worker threads, message channels, and atomic operations.

Additionally, you know how to implement concurrent programming patterns in your applications.

I invite you to think about which processes in your application can leverage this functionality to provide a better user experience. If your application performs large and complex tasks, taking them out of the main thread will significantly improve performance.

Although what we've seen in this article is sufficient to improve performance in many cases, if your project heavily depends on concurrent processing, I recommend applying more specialized languages for this purpose, such as Go or Java.