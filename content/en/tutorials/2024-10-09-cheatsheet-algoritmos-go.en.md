---
title: "Golang (Go) Algorithms Cheatsheet"
slug: "algorithms-cheatsheet-golang"
date: "2024-10-09"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Brief explanations and Go code snippets to have on hand when solving problems and for a quick refresh before coding interviews"
tags: ["algorithms"]
---

# Algorithms in Golang

## Arrays

Data structure that arranges data sequentially in contiguous memory.

- O(1) indexed read
- O(n) random insert/delete
- O(n) lookup

{{< highlight go "linenos=table" >}}
mylist := []int{1, 2, 3}
size := len(mylist)

for _, val := range mylist {
    // pass
}

for index, val := range mylist {
    // pass
}

mylist = append(mylist, 4)        // -> [1, 2, 3, 4]
mylist = append(mylist, 5, 6)     // -> [1, 2, 3, 4, 5, 6]
{{< / highlight >}}

## Sorting

Sorting is the process of rearranging the data in an array, so it is stored in ascending or descending order. Many algorithms for sorting exist, but best complexity is O(nlogn).

{{< highlight go "linenos=table" >}}
// Built-in implementation of sorting
import "sort"

sort.Ints(mylist)

sort.Slice(mylist, func(i, j int) bool {
    return mylist[i][0] < mylist[j][0]
})
{{< / highlight >}}

### Quicksort

Most common sorting algorithm, it is the built-in implementation in many languages. This algorithm is very suitable for sorting arrays in place.

This technique uses divide and conquer to divide the problem into smaller subproblems by selecting a pivot in the array, and comparing the rest to the pivot.

- O(nlogn) - complexity

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

It is a recursive technique to sort an array, it is very common for parallel sorting because it can partition the data amongst many processes.

This algorithm uses a divide and conquer technique and it works very well because merging already sorted arrays is an O(n) operation.

- O(nlogn) - complexity

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

### Binary Search

This algorithm improves lookup in lists that are sorted. Generally, looking for a value in a list has O(n) complexity but if the list is already sorted, this technique can do it in O(logn) which is way better.

This technique uses a divide and conquer approach to reduce the space of lookup by skipping irrelevant values.

- O(logn) - complexity

If the item is not found, the item should be in the position annotated with left.

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

### Two pointers

Technique to traverse an array and find a subarray, it is useful in problems where the naive solution is a double loop. Common problems are related to subarrays or intervals, leveraging a sorted array.

{{< highlight go "linenos=table" >}}
// Find subarray with two pointers
left, right := 0, len(mylist)-1
for left < right {
    // do something
    left++
    right--
}
{{< / highlight >}}

### Intervals

Intervals represent ranges defined by a start and end point, commonly used in scheduling and range problems. A common approach is to **sort intervals** by start or end times to simplify comparisons and reduce time complexity.

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

    // Sort intervals by start
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

## Linked List

A LinkedList is a data structure that stores elements sequentially, but with nodes scattered randomly in memory. Each node contains a value and a pointer to the next node.

LinkedLists allow efficient insertion/deletion at both ends but do not support indexed access.

{{< highlight go "linenos=table" >}}
type ListNode struct {
    value int
    next  *ListNode
}

head := &ListNode{value: 0}
head.next = &ListNode{value: 1}
head.next.next = &ListNode{value: 3}
{{< / highlight >}}

Linked list can be doubly linked and store a pointer to the previous element too.

To iterate through a list, it can check next nodes until it can’t.

{{< highlight go "linenos=table" >}}
curr := head
for curr != nil {
    // Process value
    curr = curr.next
}
{{< / highlight >}}

### Stack and Queue

These are data structures focused on accessing one end. Stack is a Last-in First-Out type collection, whereas a Queue is a First-in First-out collection. They can be easily implemented, and Go provides the necessary data structures.

- O(1) - add or remove from the ends
- O(n) - lookup and indexed access

{{< highlight go "linenos=table" >}}
// Queue
var queue []int

queue = append(queue, 1)
queue = append(queue, 2)

var element int
element, queue = queue[0], queue[1:] // element = 1

// Stack
var stack []int

stack = append(stack, 1)
stack = append(stack, 2)

element = stack[len(stack)-1]
stack = stack[:len(stack)-1] // element = 2
{{< / highlight >}}

## Hashmap

Hashmap is a data structure that stores key/value pairs. The values can be efficiently read using the key. It works by hashing the key to store values in a table.

- O(1) - read and write if key is provided
- O(n) - if key is not provided

{{< highlight go "linenos=table" >}}
hashmap := make(map[string]string)
hashmap["key"] = "value"

_, exists := hashmap["key"]       // exists == true
_, exists = hashmap["other key"]  // exists == false

value := hashmap["key"] // value == "value"

// Iteration formats
for key, value := range hashmap {
    // pass
}
for _, value := range hashmap {
    // pass
}
for key := range hashmap {
    // pass
}
{{< / highlight >}}

### Hashset

It’s a special case of hashmap where the values are empty structs. This data structure is used to represent sets and generally comes with efficient set algebra operations.

- O(1) - read and write a particular value
- O(n) - most of the set operations

{{< highlight go "linenos=table" >}}
// Initialize sets
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
// AuB contains 1, 2, 3, 4, 5

AnB := make(map[int]struct{})
for k := range A {
    if _, exists := B[k]; exists {
        AnB[k] = struct{}{}
    }
}
// AnB contains 3
{{< / highlight >}}

## Priority Queue

Priority Queue, also known as PQ, is a data structure whose objective is to give access to elements based on their assigned priority. The queue maintains an order of priority such that elements with the highest priority (or lowest, depending on the implementation) are always accessed first.

This data structure is commonly implemented with heaps, a special type of binary tree that ensures the highest priority element is efficiently accessible after add or delete operations.

- O(1) - read highest priority element
- O(logn) - insert or delete an element

Go's standard library provides a `container/heap` package that can be used to implement a priority queue.

{{< highlight go "linenos=table" >}}
import (
    "container/heap"
)

type Item struct {
    value    string // The value of the item; arbitrary.
    priority int    // The priority of the item.
    index    int    // The index of the item in the heap.
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
    // We want Pop to give us the lowest priority so we use less than here.
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
    old[n-1] = nil  // avoid memory leak
    item.index = -1 // for safety
    *pq = old[0 : n-1]
    return item
}

// Using the PriorityQueue
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

Data structure that efficiently stores bit values in an array; the bits can represent boolean values, and the array itself can represent sets. This data structure is less flexible than a set but can be very memory and time efficient.

- O(1) - write and read a binary value
- O(1) - algebra of sets

{{< highlight go "linenos=table" >}}
// Can be initialized as integer too.
bitmap := 0b0000

// Set bit to 1
i := 2
bitmap |= (1 << i)
// -> 0b100

// Set bit to 0
bitmap &= ^(1 << i)
// -> 0b000

// Toggle bit
bitmap ^= (1 << i)
// -> 0b100

// Check bit
(bit >> i) & 1

// Set operations
A := 0b1010
B := 0b1101

// Intersect
A & B // -> 0b1000

// Union
A | B // -> 0b1111

// Difference
A & ^B

// Check all bits
numBits := 4
A & ((1 << numBits) - 1)
{{< / highlight >}}

## Graphs

Data structure that arranges data in terms of its relations.

### Tree

Graph data structure that arranges data in hierarchical format and has no cycles. It is often represented as a struct with recursive pointers to its children.

{{< highlight go "linenos=table" >}}
type Node struct {
    val      int
    children []*Node
}

root := &Node{val: 0}
child := &Node{val: 1}
root.children = append(root.children, child)
{{< / highlight >}}

### Binary Tree

It is a special case of a tree where each node has at most 2 children. This data structure is usually represented with left and right children.

{{< highlight go "linenos=table" >}}
type Node struct {
    val   int
    left  *Node
    right *Node
}

root := &Node{val: 0}
root.left = &Node{val: 1}
{{< / highlight >}}

### Depth-First Search

Also known as DFS, it is an algorithmic technique to traverse a graph or tree visiting each path in depth before visiting other paths. It is useful when the problem requires a comparison of final scenarios of each path.

When traversing a tree, the process of the value can occur before visiting the rest of the path or after; this is called pre-order traversal or post-order traversal.

- O(n) - time complexity
- O(h) - memory complexity, where h is the height of the tree
    - O(n) - worst-case memory complexity if the tree is skewed
    - O(logn) - memory complexity if the tree is balanced

{{< highlight go "linenos=table" >}}
// Iterative implementation of DFS
stack := []*Node{}
stack = append(stack, root)

for len(stack) > 0 {
    // Pop the top element
    curr := stack[len(stack)-1]
    stack = stack[:len(stack)-1]

    // Process here if Pre-order traversal

    // Add children to the stack
    if curr.right != nil {
        stack = append(stack, curr.right)
    }
    if curr.left != nil {
        stack = append(stack, curr.left)
    }

    // Process here if Post-order traversal
}

// Recursive implementation
func DFS(node *Node) {
    if node == nil {
        return
    }
    // Process here if Pre-order traversal

    // Recursively visit children
    DFS(node.left)
    DFS(node.right)

    // Process here if Post-order traversal
}

// Start by traversing the root
DFS(root)
{{< / highlight >}}

### Breadth-First Search

Also called BFS, it is a technique to traverse a graph or tree in levels. It can be useful to find the shortest path from the root but can be less memory efficient for large paths.

- O(n) - time complexity

{{< highlight go "linenos=table" >}}
// Iterative implementation of BFS
queue := []*Node{}
queue = append(queue, root)

for len(queue) > 0 {
    // Dequeue the front element
    curr := queue[0]
    queue = queue[1:]

    // Process here

    // Add children to the queue
    if curr.left != nil {
        queue = append(queue, curr.left)
    }
    if curr.right != nil {
        queue = append(queue, curr.right)
    }
}
{{< / highlight >}}

### Backtracking

An algorithmic technique to traverse an array and process not only the value but the path taken to the value. This is especially useful when modeling decisions as a tree and comparing outcomes, for example, in games.

{{< highlight go "linenos=table" >}}
type StackItem struct {
    path []int
    node *Node
}

stack := []StackItem{}

// Add an empty array as the path and the root node
stack = append(stack, StackItem{path: []int{}, node: root})

for len(stack) > 0 {
    // Pop the top element
    curr := stack[len(stack)-1]
    stack = stack[:len(stack)-1]

    path, node := curr.path, curr.node

    // Process path and current node

    // Put the next path and node to the top of the stack
    if node.right != nil {
        stack = append(stack, StackItem{path: append(path, node.val), node: node.right})
    }
    if node.left != nil {
        stack = append(stack, StackItem{path: append(path, node.val), node: node.left})
    }
}
{{< / highlight >}}

Backtracking can also be implemented in a recursive manner, and sometimes it can be a little easier to think of this algorithm this way.

{{< highlight go "linenos=table" >}}
func BacktrackingWithDFS(path []int, node *Node) {
    // Process end of graph
    if node == nil {
        return
    }
    // Optionally process leaf nodes
    if node.left == nil && node.right == nil {
        // Process leaf node
    }
    // Process path and current node
    BacktrackingWithDFS(append(path, node.val), node.left)
    BacktrackingWithDFS(append(path, node.val), node.right)
}

BacktrackingWithDFS([]int{}, root)
{{< / highlight >}}
