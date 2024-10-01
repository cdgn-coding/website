---
title: "Cheatsheet de Algoritmos en Python"
slug: "problemas-leetcode"
date: "2024-10-01"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Explicaciones breves y snippets de código en Python para tener a mano al resolver problemas y para refrescar antes de entrevistas de código"
tags: ["algoritmos"]
---

# Algoritmos en Python

## Arrays

Estructura de datos que organiza los datos secuencialmente en memoria contigua.

- O(1) lectura indexada
- O(n) inserción/eliminación aleatoria
- O(n) búsqueda

```python
mylist = [1,2,3]
size = len(mylist)

for val in mylist:
	pass

for index, val in enumerate(mylist):
	pass
	
mylist.append(4) # -> [1,2,3,4]
mylist.extend([5,6]) # -> [1,2,3,4,5,6]
```

## Ordenación

La ordenación es el proceso de reorganizar los datos en un array para que estén almacenados en orden ascendente o descendente. Existen muchos algoritmos de ordenación, pero la mejor complejidad es O(nlogn).

```python
# Implementación incorporada de ordenación
mylist.sort()

mylist.sort(key = lambda x: x[0])
```

### Quicksort

El algoritmo de ordenación más común, es la implementación incorporada en muchos lenguajes. Este algoritmo es muy adecuado para ordenar arrays in situ.

Esta técnica utiliza divide y vencerás para dividir el problema en subproblemas más pequeños seleccionando un pivote en el array y comparando el resto con el pivote.

- O(nlogn) - complejidad

```python
def quickSort(arr):
    if len(arr) <= 1:
        return arr

    # Choosing the pivot (we'll use the last element here)
    pivot = arr[-1]
    less_than_pivot = []
    greater_than_pivot = []
    equal_to_pivot = []

    # Partitioning the array into three parts
    for element in arr:
        if element < pivot:
            less_than_pivot.append(element)
        elif element > pivot:
            greater_than_pivot.append(element)
        else:
            equal_to_pivot.append(element)

    # Recursively sort the left and right parts
    return quickSort(less_than_pivot) + equal_to_pivot + quickSort(greater_than_pivot)
```

### Merge Sort

Es una técnica recursiva para ordenar un array, es muy común para la ordenación paralela porque puede repartir los datos entre muchos procesos.

Este algoritmo utiliza una técnica de divide y vencerás y funciona muy bien porque combinar arrays ya ordenados es una operación O(n).

- O(nlogn) - complejidad

```python
def merge(arr1, arr2):
	result = []
	i = j = 0
	while i < len(arr1) and j < len(arr2):
		if arr1[i] < arr2[j]:
			result.append(arr1[i])
			i += 1
		else:
			result.append(arr2[j])
			j += 1
	result.extend(left[i:])
	result.extend(right[j:])
	
	return result

def mergeSort(arr):
	if len(arr) <= 1:
		return arr
	mid = len(arr) // 2
	left = mergeSort(arr[:mid])
	right = mergeSort(arr[mid:])
	return merge(left, right)
```

### Búsqueda Binaria

Este algoritmo mejora la búsqueda en listas que están ordenadas. Generalmente, buscar un valor en una lista tiene una complejidad O(n), pero si la lista ya está ordenada, esta técnica puede hacerlo en O(logn), lo cual es mucho mejor.

Esta técnica utiliza un enfoque de divide y vencerás para reducir el espacio de búsqueda saltando valores irrelevantes.

- O(logn) - complejidad

Si el elemento no se encuentra, debería estar en la posición anotada con left.

```python
def binarySearch(arr, value):
	if len(arr) == 0: return -1
	left, right = 0, len(arr) - 1
	while left <= right:
		pivot = (left + right) // 2
		if arr[pivot] < value:
			left = pivot + 1
		elif arr[pivot] > value:
			right = pivot - 1
		else:
			return pivot
	return -1
```

### Dos punteros

Técnica para recorrer un array y encontrar un subarray, es útil en problemas donde la solución ingenua es un doble bucle. Los problemas comunes están relacionados con subarrays o intervalos, aprovechando un array ordenado.

```python
# Encontrar subarray con dos punteros
left, right = 0, len(mylist) - 1
while left < right:
	pass
```

### Intervalos

Los intervalos representan rangos definidos por un punto de inicio y fin, comúnmente utilizados en problemas de programación y rangos. Un enfoque común es **ordenar los intervalos** por tiempos de inicio o fin para simplificar las comparaciones y reducir la complejidad temporal.

```python
def merge_intervals(intervals):
		# Sort intervals by start
    intervals.sort(key=lambda x: x[0])
    
    # Initialize with the first interval
    merged = [intervals[0]]
    for (start, end) in intervals[1:]:
		    # Latest merged interval
		    s, e = merged[-1]
		    # If current interval overlaps
        if start <= e:
            merged[-1][1] = max(e, end)
        else:
            merged.append(curr)
    return merged
```

## Lista Enlazada

Una Lista Enlazada es una estructura de datos que almacena elementos secuencialmente, pero con nodos dispersos aleatoriamente en memoria. Cada nodo contiene un valor y un puntero al siguiente nodo.

Las Listas Enlazadas permiten una inserción/eliminación eficiente en ambos extremos, pero no admiten acceso indexado.

```python
class ListNode:
	def __init__(self, value):
		self.value = value
		self.next_node = None
		
head = ListNode(0)
head.next_node = ListNode(1)
head.next_node.next_node = ListNode(3)
```

La lista enlazada puede ser doblemente enlazada y almacenar también un puntero al elemento anterior.

Para iterar a través de una lista, se pueden comprobar los nodos siguientes hasta que no se pueda.

```python
curr = head
while curr != None:
	# Process value
	curr = curr.next_node
```

### Pila y Cola

Estas son estructuras de datos enfocadas en acceder a un extremo. La Pila es una colección de tipo Último en Entrar, Primero en Salir, mientras que una Cola es una colección de tipo Primero en Entrar, Primero en Salir. Se pueden implementar fácilmente, pero Python ya tiene una implementación.

- O(1) - añadir o eliminar de los extremos
- O(n) - búsqueda y acceso indexado

```python
from collections import deque

queue = deque()
queue.appendleft(1)
queue.appendleft(2)
queue.pop() # -> 1

stack = deque()
stack.append(1)
stack.append(2)
stack.pop() # -> 2
```

## Mapa de hash

El mapa de hash es una estructura de datos que almacena pares clave/valor. Los valores se pueden leer eficientemente usando la clave. Funciona haciendo un hash de la clave para almacenar valores en una tabla.

- O(1) - lectura y escritura si se proporciona la clave
- O(n) - si no se proporciona la clave

```python
hashmap = {}
hashmap['key'] = 'value'

'key' in value # -> true
'other key' in value # -> false

hashmap['key'] # -> value

# Formatos de iteración
for key, value in hashmap.items(): pass
for key in hashmap.values(): pass
for key in hashmap: pass
```

### Conjunto de hash

Es un caso especial de mapa de hash en el que los valores son de tipo booleano. Esta estructura de datos se utiliza para representar conjuntos y generalmente viene con operaciones de álgebra de conjuntos eficientes.

- O(1) - leer y escribir un valor particular
- O(n) - la mayoría de las operaciones de conjuntos

```python
A = set([1,2,3])
1 in A # -> true
4 in A # -> false

B = set([3,4,5])

AuB = A.union(B)
# -> {1, 2, 3, 4, 5)

AnB = A.intersection(B)
# -> {3}
```

### Diccionario Ordenado

Los diccionarios regulares no garantizan ningún orden particular al iterar sus valores. Sin embargo, el Diccionario Ordenado es un Mapa de hash especializado que mantiene el orden de inserción, permitiendo la iteración sobre los elementos en la misma secuencia en que fueron añadidos.

Esto se logra utilizando memoria adicional para almacenar el orden de inserción, típicamente usando una lista enlazada.

```python
from collections import OrderedDict

# Creating an OrderedDict
ordered_dict = OrderedDict()
ordered_dict['a'] = 1
ordered_dict['b'] = 2
ordered_dict['c'] = 3

# Iterating over the OrderedDict
for key, value in ordered_dict.items():
    print(key, value)
```

## Cola de Prioridad

La Cola de Prioridad, también conocida como PQ, es una estructura de datos cuyo objetivo es dar acceso a los elementos basándose en su prioridad asignada. La cola mantiene un orden de prioridad de tal manera que los elementos con la prioridad más alta (o más baja, dependiendo de la implementación) siempre se acceden primero.

Esta estructura de datos se implementa comúnmente con montículos, un tipo especial de árbol binario que asegura que el elemento de mayor prioridad sea accesible eficientemente después de operaciones de adición o eliminación.

- O(1) - leer el elemento de mayor prioridad
- O(logn) - insertar o eliminar un elemento

```python
import heapq

pq = []

# Tuples of priority, values
heapq.heappush(pq, (1, 'Charly'))
heapq.heappush(pq, (0, 'David'))
heapq.heappush(pq, (3, 'John'))

heapq.heappop(pq)
# -> (0, David)
heapq.heappop(pq)
# -> (1, Charly)
```

## Mapas de bits

Estructura de datos que almacena eficientemente valores de bits en un array, los bits pueden representar valores booleanos y el array en sí puede representar conjuntos. Esta estructura de datos es menos flexible que un conjunto, pero puede ser muy eficiente en memoria y tiempo.

- O(1) - escribir y leer un valor binario
- O(1) - álgebra de conjuntos

```python
# Can be initialized as integer too.
bitmap = 0b0000

# Set bit to 1
i = 2
bitmap |= (1 << i)
# -> 0x0100

# Set bit to 0
bitmap &= ~(1 << i)
# -> 0x0000

# Toggle bit
bitmap ^= (1 << i)
# -> 0x0100

# Checkbit
(bitmap >> i) & 1

# Set operations
A = 0b1010
B = 0b1101

# Intersect
A & B # -> 0x1000

# Union
A | B # -> 0x1111

# Difference
A & ~B

# Check all bits
numBits = 4
A & ((1 << numBits) - 1)
```

## Grafos

Estructura de datos que organiza los datos en términos de sus relaciones.

### Árbol

Estructura de datos de grafo que organiza los datos en formato jerárquico y no tiene ciclos. A menudo se representa como una clase con punteros recursivos a sus hijos.

```python
class Node:
	def __init__(self, val):
		self.val = val
		self.children = []
		
root = Node(0)
child = Node(1)
root.children.append(child)
```

### Árbol Binario

Es un caso especial de árbol donde cada nodo tiene como máximo 2 hijos. Esta estructura de datos se suele representar con hijos izquierdo y derecho.

```python
class Node:
	def __init__(self, val):
		self.val = val
		self.left = None
		self.right = None

root = Node(0)
root.left = Node(1)
```

### Búsqueda en Profundidad

También conocida como DFS, es una técnica algorítmica para recorrer un grafo o árbol visitando cada camino en profundidad, luego visitando otros caminos. Es útil cuando el problema requiere una comparación de escenarios finales de cada camino.

Al recorrer un árbol, el procesamiento del valor puede ocurrir antes de visitar el resto del camino o después, esto se llama recorrido en pre-orden o recorrido en post-orden.

- O(n) - complejidad temporal
- O(h) - complejidad de memoria, donde h es la altura del árbol
    - O(n) - peor caso de complejidad de memoria, si el árbol está sesgado
    - O(logn) - complejidad de memoria si el árbol está equilibrado

```python
# Iterative implementation of DFS
stack = []
# Add the root node to the stack
stack.append(root)
while stack:
	# Get the current in the top of the stack
	curr = stack.pop()
	
	# Process here if Pre-order traversal
	
	# Add children to the top of the stack
	if curr.left: stack.append(curr.left)
	if curr.right: stack.append(curr.right)
	
	# Process here if Post-order traversal
	
# Recursive implementation
def DFS(node):
	# Process here if Pre-order traversal
	# Recursively visit children
	if curr.left: DFS(node.left)
	if curr.right: DFS(node.right)
	# Process here if Post-order traversal

# Start by traversing the root
DFS(root)
```

### Búsqueda en Amplitud

También llamada BFS, es una técnica para recorrer un grafo o árbol por niveles. Puede ser útil para encontrar el camino más corto desde la raíz, pero también puede ser menos eficiente en memoria para caminos largos.

- O(n) - complejidad temporal

```python
from collections import deque
# Iterative implementation of DFS
queue = deque()
# Add the root node to the stack
queue.append(root)
while queue:
	# Get the current in the top of the stack
	curr = queue.popleft()
	
	# Process here if Pre-order traversal
	
	# Add children to the top of the stack
	if curr.left: stack.append(curr.left)
	if curr.right: stack.append(curr.right)
	
	# Process here if Post-order traversal
```

### Backtracking

Una técnica algorítmica para recorrer un array y procesar no solo el valor sino también el camino tomado hasta el valor. Esto es especialmente útil para modelar decisiones como árboles y comparar resultados, por ejemplo en juegos.

```python
stack = []
# Add an empty array as the path and the root node
stack.append(([], root)
while stack:
	path, curr = stack.pop()
	# Process path and current node
	# Put the next path and node to the top of the stack
	if curr.left:
		stack.append((path + [curr.val], curr.left)
	if curr.right:
		stack.append((path + [curr.val], curr.right)
```

El backtracking también puede implementarse de manera recursiva, y a veces puede ser un poco más fácil pensar en este algoritmo de esta manera.

```python
def BacktrackingWithDFS(path, node):
	# Process end of graph
	if not node: return
	# Optionally process leaf nodes
	# Process path and current node
	if not node.left and not node.right: pass
	BacktrackingWithDFS(path + [node.val], node.left) 
	BacktrackingWithDFS(path + [node.val], node.right)

BacktrackingWithDFS([], root)
```