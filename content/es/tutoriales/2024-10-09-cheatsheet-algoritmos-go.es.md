---
title: "Cheatsheet de Algoritmos en Golang (Go)"
slug: "algoritmos-cheatsheet-golang"
date: "2024-10-09"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Explicaciones breves y fragmentos de código en Go para tener a mano al resolver problemas y para un repaso rápido antes de entrevistas de codificación"
tags: ["algoritmos"]
---

# Algoritmos en Golang

## Arrays

Estructura de datos que organiza datos secuencialmente en memoria contigua.

- O(1) lectura indexada
- O(n) inserción/eliminación aleatoria
- O(n) búsqueda

{{< highlight go "linenos=table" >}}
mylist := []int{1, 2, 3}
size := len(mylist)

for _, val := range mylist {
    // pasar
}

for index, val := range mylist {
    // pasar
}

mylist = append(mylist, 4)        // -> [1, 2, 3, 4]
mylist = append(mylist, 5, 6)     // -> [1, 2, 3, 4, 5, 6]
{{< / highlight >}}

## Ordenamiento

El ordenamiento es el proceso de reorganizar los datos en un array, de modo que se almacenen en orden ascendente o descendente. Existen muchos algoritmos para ordenar, pero la mejor complejidad es O(nlogn).

{{< highlight go "linenos=table" >}}
// Implementación incorporada de ordenamiento
import "sort"

sort.Ints(mylist)

sort.Slice(mylist, func(i, j int) bool {
    return mylist[i][0] < mylist[j][0]
})
{{< / highlight >}}

### Quicksort

El algoritmo de ordenamiento más común, es la implementación incorporada en muchos lenguajes. Este algoritmo es muy adecuado para ordenar arrays en el lugar.

Esta técnica utiliza divide y vencerás para dividir el problema en subproblemas más pequeños seleccionando un pivote en el array y comparando el resto con el pivote.

- O(nlogn) - complejidad

{{< highlight go "linenos=table" >}}
func quickSort(arr []int) []int {
    if len(arr) <= 1 {
        return arr
    }

    pivot := arr[len(arr)-1]
    lessThanPivot := []int{}
    equalToPivot := []int{}
    greaterThanPivot := []int{}

    for _, element := range arr {
        if element < pivot {
            lessThanPivot = append(lessThanPivot, element)
        } else if element > pivot {
            greaterThanPivot = append(greaterThanPivot, element)
        } else {
            equalToPivot = append(equalToPivot, element)
        }
    }

    left := quickSort(lessThanPivot)
    right := quickSort(greaterThanPivot)

    result := append(left, equalToPivot...)
    result = append(result, right...)

    return result
}
{{< / highlight >}}

### Merge Sort

Es una técnica recursiva para ordenar un array, es muy común para el ordenamiento paralelo porque puede particionar los datos entre muchos procesos.

Este algoritmo utiliza una técnica de divide y vencerás y funciona muy bien porque fusionar arrays ya ordenados es una operación O(n).

- O(nlogn) - complejidad

{{< highlight go "linenos=table" >}}
func merge(arr1, arr2 []int) []int {
    result := []int{}
    i, j := 0, 0
    for i < len(arr1) && j < len(arr2) {
        if arr1[i] < arr2[j] {
            result = append(result, arr1[i])
            i++
        } else {
            result = append(result, arr2[j])
            j++
        }
    }
    result = append(result, arr1[i:]...)
    result = append(result, arr2[j:]...)
    return result
}

func mergeSort(arr []int) []int {
    if len(arr) <= 1 {
        return arr
    }
    mid := len(arr) / 2
    left := mergeSort(arr[:mid])
    right := mergeSort(arr[mid:])
    return merge(left, right)
}
{{< / highlight >}}

### Búsqueda Binaria

Este algoritmo mejora la búsqueda en listas que están ordenadas. Generalmente, buscar un valor en una lista tiene complejidad O(n), pero si la lista ya está ordenada, esta técnica puede hacerlo en O(logn), lo cual es mucho mejor.

Esta técnica utiliza un enfoque de divide y vencerás para reducir el espacio de búsqueda saltando valores irrelevantes.

- O(logn) - complejidad

Si el elemento no se encuentra, el elemento debería estar en la posición anotada con left.

{{< highlight go "linenos=table" >}}
func binarySearch(arr []int, value int) int {
    if len(arr) == 0 {
        return -1
    }
    left, right := 0, len(arr)-1
    for left <= right {
        pivot := (left + right) / 2
        if arr[pivot] < value {
            left = pivot + 1
        } else if arr[pivot] > value {
            right = pivot - 1
        } else {
            return pivot
        }
    }
    return -1
}
{{< / highlight >}}

### Dos Punteros

Técnica para recorrer un array y encontrar un subarray, es útil en problemas donde la solución ingenua es un doble bucle. Problemas comunes están relacionados con subarrays o intervalos, aprovechando un array ordenado.

{{< highlight go "linenos=table" >}}
// Encontrar subarray con dos punteros
left, right := 0, len(mylist)-1
for left < right {
    // hacer algo
    left++
    right--
}
{{< / highlight >}}

### Intervalos

Los intervalos representan rangos definidos por un punto de inicio y fin, comúnmente usados en programación de horarios y problemas de rango. Un enfoque común es **ordenar los intervalos** por tiempos de inicio o fin para simplificar las comparaciones y reducir la complejidad temporal.

{{< highlight go "linenos=table" >}}
import "sort"

func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func mergeIntervals(intervals [][]int) [][]int {
    if len(intervals) == 0 {
        return intervals
    }

    // Ordenar intervalos por inicio
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    merged := [][]int{intervals[0]}
    for _, interval := range intervals[1:] {
        last := merged[len(merged)-1]
        if interval[0] <= last[1] {
            last[1] = max(last[1], interval[1])
        } else {
            merged = append(merged, interval)
        }
    }
    return merged
}
{{< / highlight >}}

## Lista Enlazada

Una Lista Enlazada es una estructura de datos que almacena elementos secuencialmente, pero con nodos dispersos aleatoriamente en memoria. Cada nodo contiene un valor y un puntero al siguiente nodo.

Las Listas Enlazadas permiten inserción/eliminación eficiente en ambos extremos pero no soportan acceso indexado.

{{< highlight go "linenos=table" >}}
type ListNode struct {
    value int
    next  *ListNode
}

head := &ListNode{value: 0}
head.next = &ListNode{value: 1}
head.next.next = &ListNode{value: 3}
{{< / highlight >}}

La lista enlazada puede ser doblemente enlazada y almacenar un puntero al elemento anterior también.

Para iterar a través de una lista, puede verificar los siguientes nodos hasta que no pueda.

{{< highlight go "linenos=table" >}}
curr := head
for curr != nil {
    // Procesar valor
    curr = curr.next
}
{{< / highlight >}}

### Pila y Cola

Estas son estructuras de datos enfocadas en acceder a un extremo. Una Pila es una colección de tipo Último en Entrar Primero en Salir (LIFO), mientras que una Cola es una colección de tipo Primero en Entrar Primero en Salir (FIFO). Pueden implementarse fácilmente, y Go proporciona las estructuras de datos necesarias.

- O(1) - agregar o eliminar de los extremos
- O(n) - búsqueda y acceso indexado

{{< highlight go "linenos=table" >}}
// Cola
var queue []int

queue = append(queue, 1)
queue = append(queue, 2)

var element int
element, queue = queue[0], queue[1:] // element = 1

// Pila
var stack []int

stack = append(stack, 1)
stack = append(stack, 2)

element = stack[len(stack)-1]
stack = stack[:len(stack)-1] // element = 2
{{< / highlight >}}

## Hashmap

Un Hashmap es una estructura de datos que almacena pares clave/valor. Los valores pueden ser leídos eficientemente usando la clave. Funciona haciendo hash de la clave para almacenar valores en una tabla.

- O(1) - lectura y escritura si se proporciona la clave
- O(n) - si no se proporciona la clave

{{< highlight go "linenos=table" >}}
hashmap := make(map[string]string)
hashmap["key"] = "value"

_, exists := hashmap["key"]       // exists == true
_, exists = hashmap["other key"]  // exists == false

value := hashmap["key"] // value == "value"

// Formatos de iteración
for key, value := range hashmap {
    // pasar
}
for _, value := range hashmap {
    // pasar
}
for key := range hashmap {
    // pasar
}
{{< / highlight >}}

### Hashset

Es un caso especial de hashmap donde los valores son structs vacíos. Esta estructura de datos se utiliza para representar conjuntos y generalmente viene con operaciones de álgebra de conjuntos eficientes.

- O(1) - leer y escribir un valor particular
- O(n) - la mayoría de las operaciones de conjunto

{{< highlight go "linenos=table" >}}
// Inicializar conjuntos
A := make(map[int]struct{})
A[1] = struct{}{}
A[2] = struct{}{}
A[3] = struct{}{}

_, exists := A[1] // exists == true
_, exists = A[4]  // exists == false

B := make(map[int]struct{})
B[3] = struct{}{}
B[4] = struct{}{}
B[5] = struct{}{}

AuB := make(map[int]struct{})
for k := range A {
    AuB[k] = struct{}{}
}
for k := range B {
    AuB[k] = struct{}{}
}
// AuB contiene 1, 2, 3, 4, 5

AnB := make(map[int]struct{})
for k := range A {
    if _, exists := B[k]; exists {
        AnB[k] = struct{}{}
    }
}
// AnB contiene 3
{{< / highlight >}}

## Cola de Prioridad

La Cola de Prioridad, también conocida como PQ, es una estructura de datos cuyo objetivo es dar acceso a elementos basándose en su prioridad asignada. La cola mantiene un orden de prioridad tal que los elementos con mayor prioridad (o menor, dependiendo de la implementación) siempre son accedidos primero.

Esta estructura de datos se implementa comúnmente con montículos (heaps), un tipo especial de árbol binario que asegura que el elemento de mayor prioridad sea accesible eficientemente después de operaciones de agregar o eliminar.

- O(1) - leer el elemento de mayor prioridad
- O(logn) - insertar o eliminar un elemento

La biblioteca estándar de Go proporciona un paquete `container/heap` que puede usarse para implementar una cola de prioridad.

{{< highlight go "linenos=table" >}}
import (
    "container/heap"
)

type Item struct {
    value    string // El valor del elemento; arbitrario.
    priority int    // La prioridad del elemento.
    index    int    // El índice del elemento en el heap.
}

// Una PriorityQueue implementa heap.Interface y mantiene Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    // Queremos que Pop nos dé el de menor prioridad, así que usamos menor que aquí.
    return pq[i].priority < pq[j].priority
}

func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].index = i
    pq[j].index = j
}

func (pq *PriorityQueue) Push(x interface{}) {
    n := len(*pq)
    item := x.(*Item)
    item.index = n
    *pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
    old := *pq
    n := len(old)
    item := old[n-1]
    old[n-1] = nil  // evitar fuga de memoria
    item.index = -1 // por seguridad
    *pq = old[0 : n-1]
    return item
}

// Usando la PriorityQueue
pq := &PriorityQueue{}
heap.Init(pq)

heap.Push(pq, &Item{
    value:    "Charly",
    priority: 1,
})

heap.Push(pq, &Item{
    value:    "David",
    priority: 0,
})

heap.Push(pq, &Item{
    value:    "John",
    priority: 3,
})

item := heap.Pop(pq).(*Item)
// item.value == "David"

item = heap.Pop(pq).(*Item)
// item.value == "Charly"
{{< / highlight >}}

## Bitmaps

Estructura de datos que almacena eficientemente valores de bits en un array; los bits pueden representar valores booleanos, y el array en sí puede representar conjuntos. Esta estructura de datos es menos flexible que un conjunto pero puede ser muy eficiente en memoria y tiempo.

- O(1) - escribir y leer un valor binario
- O(1) - álgebra de conjuntos

{{< highlight go "linenos=table" >}}
// También puede inicializarse como entero.
bitmap := 0b0000

// Establecer bit a 1
i := 2
bitmap |= (1 << i)
// -> 0b100

// Establecer bit a 0
bitmap &= ^(1 << i)
// -> 0b000

// Alternar bit
bitmap ^= (1 << i)
// -> 0b100

// Comprobar bit
(bit >> i) & 1

// Operaciones de conjunto
A := 0b1010
B := 0b1101

// Intersección
A & B // -> 0b1000

// Unión
A | B // -> 0b1111

// Diferencia
A & ^B

// Comprobar todos los bits
numBits := 4
A & ((1 << numBits) - 1)
{{< / highlight >}}

## Grafos

Estructura de datos que organiza datos en términos de sus relaciones.

### Árbol

Estructura de datos de grafo que organiza datos en formato jerárquico y no tiene ciclos. A menudo se representa como una estructura con punteros recursivos a sus hijos.

{{< highlight go "linenos=table" >}}
type Node struct {
    val      int
    children []*Node
}

root := &Node{val: 0}
child := &Node{val: 1}
root.children = append(root.children, child)
{{< / highlight >}}

### Árbol Binario

Es un caso especial de un árbol donde cada nodo tiene como máximo 2 hijos. Esta estructura de datos usualmente se representa con hijos izquierdo y derecho.

{{< highlight go "linenos=table" >}}
type Node struct {
    val   int
    left  *Node
    right *Node
}

root := &Node{val: 0}
root.left = &Node{val: 1}
{{< / highlight >}}

### Búsqueda en Profundidad

También conocida como DFS, es una técnica algorítmica para recorrer un grafo o árbol visitando cada camino en profundidad antes de visitar otros caminos. Es útil cuando el problema requiere una comparación de escenarios finales de cada camino.

Al recorrer un árbol, el procesamiento del valor puede ocurrir antes de visitar el resto del camino o después; esto se llama recorrido preorden o postorden.

- O(n) - complejidad temporal
- O(h) - complejidad de memoria, donde h es la altura del árbol
    - O(n) - complejidad de memoria en el peor de los casos si el árbol está sesgado
    - O(logn) - complejidad de memoria si el árbol está equilibrado

{{< highlight go "linenos=table" >}}
// Implementación iterativa de DFS
stack := []*Node{}
stack = append(stack, root)

for len(stack) > 0 {
    // Sacar el elemento superior
    curr := stack[len(stack)-1]
    stack = stack[:len(stack)-1]

    // Procesar aquí si es recorrido preorden

    // Agregar hijos a la pila
    if curr.right != nil {
        stack = append(stack, curr.right)
    }
    if curr.left != nil {
        stack = append(stack, curr.left)
    }

    // Procesar aquí si es recorrido postorden
}

// Implementación recursiva
func DFS(node *Node) {
    if node == nil {
        return
    }
    // Procesar aquí si es recorrido preorden

    // Visitar recursivamente los hijos
    DFS(node.left)
    DFS(node.right)

    // Procesar aquí si es recorrido postorden
}

// Comenzar recorriendo la raíz
DFS(root)
{{< / highlight >}}

### Búsqueda en Anchura

También llamada BFS, es una técnica para recorrer un grafo o árbol en niveles. Puede ser útil para encontrar el camino más corto desde la raíz pero puede ser menos eficiente en memoria para caminos largos.

- O(n) - complejidad temporal

{{< highlight go "linenos=table" >}}
// Implementación iterativa de BFS
queue := []*Node{}
queue = append(queue, root)

for len(queue) > 0 {
    // Desencolar el elemento frontal
    curr := queue[0]
    queue = queue[1:]

    // Procesar aquí

    // Agregar hijos a la cola
    if curr.left != nil {
        queue = append(queue, curr.left)
    }
    if curr.right != nil {
        queue = append(queue, curr.right)
    }
}
{{< / highlight >}}

### Backtracking

Una técnica algorítmica para recorrer un array y procesar no solo el valor sino el camino tomado hasta el valor. Esto es especialmente útil al modelar decisiones como árbol y comparar resultados, por ejemplo en juegos.

{{< highlight go "linenos=table" >}}
type StackItem struct {
    path []int
    node *Node
}

stack := []StackItem{}

// Agregar un array vacío como camino y el nodo raíz
stack = append(stack, StackItem{path: []int{}, node: root})

for len(stack) > 0 {
    // Sacar el elemento superior
    curr := stack[len(stack)-1]
    stack = stack[:len(stack)-1]

    path, node := curr.path, curr.node

    // Procesar camino y nodo actual

    // Poner el siguiente camino y nodo en la parte superior de la pila
    if node.right != nil {
        stack = append(stack, StackItem{path: append(path, node.val), node: node.right})
    }
    if node.left != nil {
        stack = append(stack, StackItem{path: append(path, node.val), node: node.left})
    }
}
{{< / highlight >}}

El backtracking también puede implementarse de manera recursiva, y a veces puede ser un poco más fácil pensar en este algoritmo de esta manera.

{{< highlight go "linenos=table" >}}
func BacktrackingWithDFS(path []int, node *Node) {
    // Procesar fin del grafo
    if node == nil {
        return
    }
    // Opcionalmente procesar nodos hoja
    if node.left == nil && node.right == nil {
        // Procesar nodo hoja
    }
    // Procesar camino y nodo actual
    BacktrackingWithDFS(append(path, node.val), node.left)
    BacktrackingWithDFS(append(path, node.val), node.right)
}

BacktrackingWithDFS([]int{}, root)
{{< / highlight >}}

