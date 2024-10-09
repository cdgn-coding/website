---
title: "Cheatsheet de Algoritmos en Javascript"
slug: "algoritmos-cheatsheet-javascript"
date: "2024-10-09"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Explicaciones breves y fragmentos de código JavaScript para tener a mano al resolver problemas y para un repaso rápido antes de entrevistas de programación"
tags: ["algoritmos"]
---

# Algoritmos en JavaScript

## Arreglos (Arrays)

Estructura de datos que organiza datos secuencialmente en memoria contigua.

- O(1) lectura indexada
- O(n) inserción/eliminación aleatoria
- O(n) búsqueda

{{< highlight javascript "linenos=table" >}}
let mylist = [1, 2, 3];
let size = mylist.length;

for (let val of mylist) {
    // pasar
}

for (let [index, val] of mylist.entries()) {
    // pasar
}

mylist.push(4); // -> [1, 2, 3, 4]
mylist.push(5, 6); // -> [1, 2, 3, 4, 5, 6]
{{< / highlight >}}

## Ordenamiento

El ordenamiento es el proceso de reorganizar los datos en un arreglo, de modo que se almacenen en orden ascendente o descendente. Existen muchos algoritmos para ordenar, pero la mejor complejidad es O(nlogn).

{{< highlight javascript "linenos=table" >}}
// Implementación incorporada de ordenamiento
mylist.sort((a, b) => a - b);

mylist.sort((a, b) => a[0] - b[0]);
{{< / highlight >}}

### Quicksort

El algoritmo de ordenamiento más común, es la implementación incorporada en muchos lenguajes. Este algoritmo es muy adecuado para ordenar arreglos en su lugar.

Esta técnica utiliza divide y vencerás para dividir el problema en subproblemas más pequeños seleccionando un pivote en el arreglo y comparando el resto con el pivote.

- O(nlogn) - complejidad

{{< highlight javascript "linenos=table" >}}
function quickSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }

    // Elegir el pivote (usaremos el último elemento aquí)
    const pivot = arr[arr.length - 1];
    const less_than_pivot = [];
    const greater_than_pivot = [];
    const equal_to_pivot = [];

    // Particionar el arreglo en tres partes
    for (const element of arr) {
        if (element < pivot) {
            less_than_pivot.push(element);
        } else if (element > pivot) {
            greater_than_pivot.push(element);
        } else {
            equal_to_pivot.push(element);
        }
    }

    // Ordenar recursivamente las partes izquierda y derecha
    return quickSort(less_than_pivot).concat(equal_to_pivot, quickSort(greater_than_pivot));
}
{{< / highlight >}}

### Merge Sort

Es una técnica recursiva para ordenar un arreglo, es muy común para el ordenamiento paralelo porque puede dividir los datos entre muchos procesos.

Este algoritmo utiliza una técnica de divide y vencerás y funciona muy bien porque fusionar arreglos ya ordenados es una operación O(n).

- O(nlogn) - complejidad

{{< highlight javascript "linenos=table" >}}
function merge(arr1, arr2) {
    const result = [];
    let i = 0,
        j = 0;
    while (i < arr1.length && j < arr2.length) {
        if (arr1[i] < arr2[j]) {
            result.push(arr1[i]);
            i += 1;
        } else {
            result.push(arr2[j]);
            j += 1;
        }
    }
    result.push(...arr1.slice(i));
    result.push(...arr2.slice(j));

    return result;
}

function mergeSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }
    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));
    return merge(left, right);
}
{{< / highlight >}}

### Búsqueda Binaria

Este algoritmo mejora la búsqueda en listas que están ordenadas. Generalmente, buscar un valor en una lista tiene complejidad O(n), pero si la lista ya está ordenada, esta técnica puede hacerlo en O(logn), lo cual es mucho mejor.

Esta técnica utiliza un enfoque de divide y vencerás para reducir el espacio de búsqueda saltando valores irrelevantes.

- O(logn) - complejidad

Si el elemento no se encuentra, el elemento debería estar en la posición anotada con left.

{{< highlight javascript "linenos=table" >}}
function binarySearch(arr, value) {
    if (arr.length === 0) return -1;
    let left = 0,
        right = arr.length - 1;
    while (left <= right) {
        const pivot = Math.floor((left + right) / 2);
        if (arr[pivot] < value) {
            left = pivot + 1;
        } else if (arr[pivot] > value) {
            right = pivot - 1;
        } else {
            return pivot;
        }
    }
    return -1;
}
{{< / highlight >}}

### Dos Punteros

Técnica para recorrer un arreglo y encontrar un subarreglo, es útil en problemas donde la solución ingenua es un doble bucle. Los problemas comunes están relacionados con subarreglos o intervalos, aprovechando un arreglo ordenado.

{{< highlight javascript "linenos=table" >}}
// Encontrar subarreglo con dos punteros
let left = 0,
    right = mylist.length - 1;
while (left < right) {
    // hacer algo
    left++;
    right--;
}
{{< / highlight >}}

### Intervalos

Los intervalos representan rangos definidos por un punto de inicio y fin, comúnmente usados en problemas de programación y rango. Un enfoque común es **ordenar los intervalos** por tiempos de inicio o fin para simplificar las comparaciones y reducir la complejidad temporal.

{{< highlight javascript "linenos=table" >}}
function merge_intervals(intervals) {
    // Ordenar intervalos por inicio
    intervals.sort((a, b) => a[0] - b[0]);

    // Inicializar con el primer intervalo
    const merged = [intervals[0]];
    for (const [start, end] of intervals.slice(1)) {
        // Último intervalo fusionado
        const last = merged[merged.length - 1];
        const [s, e] = last;
        // Si el intervalo actual se superpone
        if (start <= e) {
            last[1] = Math.max(e, end);
        } else {
            merged.push([start, end]);
        }
    }
    return merged;
}
{{< / highlight >}}

## Lista Enlazada (Linked List)

Una Lista Enlazada es una estructura de datos que almacena elementos secuencialmente, pero con nodos dispersos aleatoriamente en memoria. Cada nodo contiene un valor y un puntero al siguiente nodo.

Las Listas Enlazadas permiten una inserción/eliminación eficiente en ambos extremos, pero no admiten acceso indexado.

{{< highlight javascript "linenos=table" >}}
class ListNode {
    constructor(value) {
        this.value = value;
        this.next_node = null;
    }
}

const head = new ListNode(0);
head.next_node = new ListNode(1);
head.next_node.next_node = new ListNode(3);
{{< / highlight >}}

La lista enlazada puede ser doblemente enlazada y almacenar un puntero al elemento anterior también.

Para iterar a través de una lista, puede verificar los siguientes nodos hasta que no pueda.

{{< highlight javascript "linenos=table" >}}
let curr = head;
while (curr !== null) {
    // Procesar valor
    curr = curr.next_node;
}
{{< / highlight >}}

### Pila y Cola (Stack and Queue)

Estas son estructuras de datos enfocadas en acceder a un extremo. La pila es una colección de tipo Último en Entrar, Primero en Salir (LIFO), mientras que una cola es una colección de tipo Primero en Entrar, Primero en Salir (FIFO). Pueden implementarse fácilmente, pero JavaScript ya tiene una implementación.

- O(1) - agregar o eliminar de los extremos
- O(n) - búsqueda y acceso indexado

{{< highlight javascript "linenos=table" >}}
// Cola
const queue = [];

queue.push(1);
queue.push(2);
queue.shift(); // -> 1

// Pila
const stack = [];

stack.push(1);
stack.push(2);
stack.pop(); // -> 2
{{< / highlight >}}

## Hashmap

El Hashmap es una estructura de datos que almacena pares clave/valor. Los valores pueden leerse eficientemente usando la clave. Funciona hashando la clave para almacenar valores en una tabla.

- O(1) - lectura y escritura si se proporciona la clave
- O(n) - si no se proporciona la clave

{{< highlight javascript "linenos=table" >}}
const hashmap = {};
hashmap['key'] = 'value';

'key' in hashmap; // -> true
'other key' in hashmap; // -> false

hashmap['key']; // -> 'value'

// Formatos de iteración
for (const [key, value] of Object.entries(hashmap)) {
    // pasar
}
for (const value of Object.values(hashmap)) {
    // pasar
}
for (const key in hashmap) {
    // pasar
}
{{< / highlight >}}

### Conjunto (Hashset)

Es un caso especial de hashmap en el cual los valores son de tipo booleanos. Esta estructura de datos se utiliza para representar conjuntos y generalmente viene con operaciones de álgebra de conjuntos eficientes.

- O(1) - leer y escribir un valor particular
- O(n) - la mayoría de las operaciones de conjunto

{{< highlight javascript "linenos=table" >}}
const A = new Set([1, 2, 3]);
A.has(1); // -> true
A.has(4); // -> false

const B = new Set([3, 4, 5]);

const AuB = new Set([...A, ...B]);
// -> {1, 2, 3, 4, 5}

const AnB = new Set([...A].filter(x => B.has(x)));
// -> {3}
{{< / highlight >}}

### Diccionario Ordenado (Ordered Dictionary)

Los diccionarios regulares no garantizan ningún orden en particular al iterar sus valores. Sin embargo, un Diccionario Ordenado es un Hashmap especializado que mantiene el orden de inserción, permitiendo la iteración sobre los elementos en la misma secuencia en que fueron agregados.

Esto se logra utilizando memoria adicional para almacenar el orden de inserción, típicamente usando una lista enlazada.

{{< highlight javascript "linenos=table" >}}
const ordered_dict = new Map();

ordered_dict.set('a', 1);
ordered_dict.set('b', 2);
ordered_dict.set('c', 3);

// Iterando sobre el Map
for (const [key, value] of ordered_dict.entries()) {
    console.log(key, value);
}
{{< / highlight >}}

## Cola de Prioridad (Priority Queue)

La Cola de Prioridad, también conocida como PQ, es una estructura de datos cuyo objetivo es dar acceso a elementos basados en su prioridad asignada. La cola mantiene un orden de prioridad tal que los elementos con mayor prioridad (o menor, dependiendo de la implementación) siempre se acceden primero.

Esta estructura de datos se implementa comúnmente con montículos (heaps), un tipo especial de árbol binario que asegura que el elemento de mayor prioridad sea accesible eficientemente después de operaciones de agregar o eliminar.

- O(1) - leer el elemento de mayor prioridad
- O(logn) - insertar o eliminar un elemento

JavaScript no tiene una Cola de Prioridad incorporada en su biblioteca estándar, pero puedes implementar una usando una clase personalizada o utilizar bibliotecas de terceros.

{{< highlight javascript "linenos=table" >}}
class PriorityQueue {
    constructor() {
        this.heap = [];
    }

    push(element) {
        this.heap.push(element);
        this.heap.sort((a, b) => a[0] - b[0]); // Ordenar basado en prioridad
    }

    pop() {
        return this.heap.shift();
    }
}

const pq = new PriorityQueue();

// Tuplas de prioridad, valores
pq.push([1, 'Charly']);
pq.push([0, 'David']);
pq.push([3, 'John']);

pq.pop(); // -> [0, 'David']
pq.pop(); // -> [1, 'Charly']
{{< / highlight >}}

## Mapas de Bits (Bitmaps)

Estructura de datos que almacena eficientemente valores de bits en un arreglo, los bits pueden representar valores booleanos y el propio arreglo puede representar conjuntos. Esta estructura de datos es menos flexible que un conjunto, pero puede ser muy eficiente en memoria y tiempo.

- O(1) - escribir y leer un valor binario
- O(1) - álgebra de conjuntos

{{< highlight javascript "linenos=table" >}}
// Puede inicializarse como entero también.
let bitmap = 0b0000;

// Establecer bit a 1
const i = 2;
bitmap |= (1 << i);
// -> 0b100

// Establecer bit a 0
bitmap &= ~(1 << i);
// -> 0b000

// Alternar bit
bitmap ^= (1 << i);
// -> 0b100

// Comprobar bit
((bitmap >> i) & 1);

// Operaciones de conjunto
let A = 0b1010;
let B = 0b1101;

// Intersección
A & B; // -> 0b1000

// Unión
A | B; // -> 0b1111

// Diferencia
A & ~B;

// Comprobar todos los bits
const numBits = 4;
A & ((1 << numBits) - 1);
{{< / highlight >}}

## Grafos

Estructura de datos que organiza datos en términos de sus relaciones.

### Árbol

Estructura de datos de grafo que organiza datos en formato jerárquico y no tiene ciclos. A menudo se representa como una clase con punteros recursivos a sus hijos.

{{< highlight javascript "linenos=table" >}}
class Node {
    constructor(val) {
        this.val = val;
        this.children = [];
    }
}

const root = new Node(0);
const child = new Node(1);
root.children.push(child);
{{< / highlight >}}

### Árbol Binario

Es un caso especial de un árbol donde cada nodo tiene como máximo 2 hijos. Esta estructura de datos se suele representar con hijos izquierdo y derecho.

{{< highlight javascript "linenos=table" >}}
class Node {
    constructor(val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}

const root = new Node(0);
root.left = new Node(1);
{{< / highlight >}}

### Búsqueda en Profundidad (Depth-First Search)

También conocido como DFS, es una técnica algorítmica para recorrer un grafo o árbol visitando cada ruta en profundidad y luego visitando otras rutas. Es útil cuando el problema requiere una comparación de escenarios finales de cada ruta.

Al recorrer un árbol, el procesamiento del valor puede ocurrir antes de visitar el resto de la ruta o después; esto se llama recorrido en preorden o postorden.

- O(n) - complejidad temporal
- O(h) - complejidad de memoria, donde h es la altura del árbol
    - O(n) - peor caso de complejidad de memoria, si el árbol está sesgado
    - O(logn) - complejidad de memoria si el árbol está equilibrado

{{< highlight javascript "linenos=table" >}}
// Implementación iterativa de DFS
const stack = [];

// Agregar el nodo raíz a la pila
stack.push(root);

while (stack.length > 0) {
    // Obtener el nodo actual de la parte superior de la pila
    const curr = stack.pop();

    // Procesar aquí si es recorrido en preorden

    // Agregar hijos a la parte superior de la pila
    if (curr.right) stack.push(curr.right);
    if (curr.left) stack.push(curr.left);

    // Procesar aquí si es recorrido en postorden
}

// Implementación recursiva
function DFS(node) {
    if (!node) return;

    // Procesar aquí si es recorrido en preorden

    // Visitar recursivamente a los hijos
    DFS(node.left);
    DFS(node.right);

    // Procesar aquí si es recorrido en postorden
}

// Comenzar recorriendo la raíz
DFS(root);
{{< / highlight >}}

### Búsqueda en Anchura (Breadth-First Search)

También llamada BFS, es una técnica para recorrer un grafo o árbol en niveles. Puede ser útil para encontrar el camino más corto desde la raíz, pero también puede ser menos eficiente en memoria para rutas largas.

- O(n) - complejidad temporal

{{< highlight javascript "linenos=table" >}}
// Implementación iterativa de BFS
const queue = [];

// Agregar el nodo raíz a la cola
queue.push(root);

while (queue.length > 0) {
    // Obtener el nodo actual del frente de la cola
    const curr = queue.shift();

    // Procesar aquí

    // Agregar hijos al final de la cola
    if (curr.left) queue.push(curr.left);
    if (curr.right) queue.push(curr.right);
}
{{< / highlight >}}

### Backtracking

Una técnica algorítmica para recorrer un arreglo y procesar no solo el valor sino el camino tomado hasta el valor. Esto es especialmente útil al modelar decisiones como un árbol y comparar resultados, por ejemplo en juegos.

{{< highlight javascript "linenos=table" >}}
const stack = [];

// Agregar un arreglo vacío como el camino y el nodo raíz
stack.push([[], root]);

while (stack.length > 0) {
    const [path, curr] = stack.pop();

    // Procesar camino y nodo actual

    // Poner el siguiente camino y nodo en la parte superior de la pila
    if (curr.right) {
        stack.push([path.concat(curr.val), curr.right]);
    }
    if (curr.left) {
        stack.push([path.concat(curr.val), curr.left]);
    }
}
{{< / highlight >}}

El backtracking también puede implementarse de manera recursiva, y a veces puede ser un poco más fácil pensar en este algoritmo de esta manera.

{{< highlight javascript "linenos=table" >}}
function BacktrackingWithDFS(path, node) {
    // Procesar fin del grafo
    if (!node) return;
    // Opcionalmente procesar nodos hoja
    // Procesar camino y nodo actual
    if (!node.left && !node.right) {
        // Procesar nodo hoja
    }
    BacktrackingWithDFS([...path, node.val], node.left);
    BacktrackingWithDFS([...path, node.val], node.right);
}

BacktrackingWithDFS([], root);
{{< / highlight >}}
