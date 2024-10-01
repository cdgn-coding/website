---
title: "Python Algorithms Cheatsheet"
slug: "leetcode-problems"
date: "2024-10-01"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Brief explanations and Python code snippets to have on hand when solving problems and for a quick refresh before coding interviews"
tags: ["algorithms"]
---

# Algorithms in Python

## Arrays

Data structure that arranges data sequentially in contiguous memory.

- O(1) indexed read
- O(n) random insert/delete
- O(n) lookup

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

## Sorting

Sorting is the process of rearranging the data in an array, so it is stored in ascending or descending order. Many algorithms for sorting exist, but best complexity is O(nlogn).

```python
# Built-in implementation of sorting
mylist.sort()

mylist.sort(key = lambda x: x[0])
```

### Quicksort

Most common sorting algorithm, it is the built-in implementation in many languages. This algorithm is very suitable for sorting arrays in place.

This technique uses divide and conquer to divide the problem into smaller subproblems by selecting a pivot in the array, and comparing the rest to the pivot.

- O(nlogn) - complexity

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

It is a recursive technique to sort an array, it is very common for parallel sorting because it can partition the data amongst many processes.

This algorithm uses a divide and conquer technique and it works very well because merging already sorted arrays is an O(n) operation.

- O(nlogn) - complexity

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

### Binary Search

This algorithm improves lookup in lists that are sorted. Generally, looking for a value in a list has O(n) complexity but if the list is already sorted, this technique can do it in O(logn) which is way better.

This technique uses a divide and conquer approach to reduce the space of lookup by skipping irrelevant values.

- O(logn) - complexity

If the item is not found, the item should be in the position anotated with left.

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

### Two pointers

Technique to traverse an array and find a subarray, it is useful in problems where the naive solution is a double loop. Common problems are related to subarrays or intervals, leveraging a sorted array.

```python
# Find sub array with two pointers
left, right = 0, len(mylist) - 1
while left < right:
	pass
```

### Intervals

Intervals represent ranges defined by a start and end point, commonly used in scheduling and range problems. A common approach is to **sort intervals** by start or end times to simplify comparisons and reduce time complexity.

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

## Linked List

A LinkedList is a data structure that stores elements sequentially, but with nodes scattered randomly in memory. Each node contains a value and a pointer to the next node.

LinkedLists allow efficient insertion/deletion at both ends but do not support indexed access.

```python
class ListNode:
	def __init__(self, value):
		self.value = value
		self.next_node = None
		
head = ListNode(0)
head.next_node = ListNode(1)
head.next_node.next_node = ListNode(3)
```

Linked list can be doubly linked and store a pointer to the previous element too. 

To iterate through a list, it can check next nodes until it can’t.

```python
curr = head
while curr != None:
	# Process value
	curr = curr.next_node
```

### Stack and Queue

These are data structures focused in accessing one end. Stack is a Last-in First-Out type collection, whereas a Queue is a First-in First-out collection. They can be easily implemented but Python already has an implementation.

- O(1) - add or remove from the ends
- O(n) - lookup and indexed access

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

## Hashmap

Hashmap is a data structure that stores key/value pairs. The values can be efficiently read using the key. It works by hashing the key to store values in a table.

- O(1) - read and write if key is provided
- O(n) - if key is not provided

```python
hashmap = {}
hashmap['key'] = 'value'

'key' in value # -> true
'other key' in value # -> false

hashmap['key'] # -> value

# Iteration formats
for key, value in hashmap.items(): pass
for key in hashmap.values(): pass
for key in hashmap: pass
```

### Hashset

It’s a special case of hashmap in which the values are of type booleans. This data structure is used to represent sets and generally comes with efficient set algebra operations.

- O(1) - read and write a particular value
- O(n) - most of the set operations

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

### Ordered Dictionary

Regular dictionaries do not guarantee any particular order when iterating its values. However Ordered Dictionary is a specialized Hashmap that maintains the order of insertion, allowing iteration over the elements in the same sequence in which they were added.

This is achieved by using additional memory to store the insertion order, typically using a linked list.

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

## Priority Queue

Priority Queue also known as PQ, is a data structure whose objective is to give access to elements based on their assigned priority. The queue is maintains and order of priority such that elements with highest priority (or lowest, depending on the implementation) is always accessed first.

This data structure is commonly implemented with heaps, a special type of binary tree that ensure the highest priority element is efficiently accesible after add or delete operations.

- O(1) - read highest priority element
- O(logn) - insert or delete an element

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

## Bitmaps

Data structure that efficiently stores bit values in an array, the bits can represent boolean values and the array itself can represent sets. This data structure is less flexible than a set but can be very memory and time efficient.

- O(1) - write and read a binary value
- O(1) - algebra of sets

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

## Graphs

Data structure that arranges data in terms of its relations.

### Tree

Graph data structure that arranges data in hierarchical format and has no cycles. It is often represented as class with recursive pointers to its children.

```python
class Node:
	def __init__(self, val):
		self.val = val
		self.children = []
		
root = Node(0)
child = Node(1)
root.children.append(child)
```

### Binary Tree

It is an special case of a tree where each node has at most 2 children. This data structure is usually represented with left and right children.

```python
class Node:
	def __init__(self, val):
		self.val = val
		self.left = None
		self.right = None

root = Node(0)
root.left = Node(1)
```

### Depth-First Search

Also known as DFS is algorithm technique to traverse a graph or tree visiting each path in depth, then visiting other paths. It is useful when the problem requires a comparison of final escenarios of each path.

When traversing a tree, the process of the value can occur before visiting the rest of the path or after, this is called pre-order traversal or post-order traversal.

- O(n) - time complexity
- O(h) - memory complexity, where h is the height of the tree
    - O(n) - worst case memory complexity, if the tree is skewed
    - O(logn) -  memory complexity if the tree is balanced

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

### Breadth-First Search

Also called BFS is a technique to traverse a graph or tree in levels. It can be useful to find the shortest path from the root, but can also be less memory efficient for large paths.

- O(n) - time complexity

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

An algorithmic technique to traverse and array and process not only the value but the path taken to the value. This is specially useful modeling decisions as tree and comparing outcomes, for example in games.

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

Backtracking can be also implemented in a recursive manner, and sometimes can be a little easier to think of this algorithm this way. 

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