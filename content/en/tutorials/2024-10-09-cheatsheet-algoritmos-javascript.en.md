---
title: "JavaScript Algorithms Cheatsheet"
slug: "algorithms-cheatsheet-javascript"
date: "2024-10-09"
toc: false
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Brief explanations and JavaScript code snippets to have on hand when solving problems and for a quick refresh before coding interviews"
tags: ["algorithms"]
---

# Algorithms in JavaScript

## Arrays

Data structure that arranges data sequentially in contiguous memory.

- O(1) indexed read
- O(n) random insert/delete
- O(n) lookup

{{< highlight javascript "linenos=table" >}}
let mylist = [1, 2, 3];
let size = mylist.length;

for (let val of mylist) {
    // pass
}

for (let [index, val] of mylist.entries()) {
    // pass
}

mylist.push(4); // -> [1, 2, 3, 4]
mylist.push(5, 6); // -> [1, 2, 3, 4, 5, 6]
{{< / highlight >}}

## Sorting

Sorting is the process of rearranging the data in an array, so it is stored in ascending or descending order. Many algorithms for sorting exist, but best complexity is O(nlogn).

{{< highlight javascript "linenos=table" >}}
// Built-in implementation of sorting
mylist.sort((a, b) => a - b);

mylist.sort((a, b) => a[0] - b[0]);
{{< / highlight >}}

### Quicksort

Most common sorting algorithm, it is the built-in implementation in many languages. This algorithm is very suitable for sorting arrays in place.

This technique uses divide and conquer to divide the problem into smaller subproblems by selecting a pivot in the array, and comparing the rest to the pivot.

- O(nlogn) - complexity

{{< highlight javascript "linenos=table" >}}
function quickSort(arr) {
    if (arr.length <= 1) {
        return arr;
    }

    // Choosing the pivot (we'll use the last element here)
    const pivot = arr[arr.length - 1];
    const less_than_pivot = [];
    const greater_than_pivot = [];
    const equal_to_pivot = [];

    // Partitioning the array into three parts
    for (const element of arr) {
        if (element < pivot) {
            less_than_pivot.push(element);
        } else if (element > pivot) {
            greater_than_pivot.push(element);
        } else {
            equal_to_pivot.push(element);
        }
    }

    // Recursively sort the left and right parts
    return quickSort(less_than_pivot).concat(equal_to_pivot, quickSort(greater_than_pivot));
}
{{< / highlight >}}

### Merge Sort

It is a recursive technique to sort an array, it is very common for parallel sorting because it can partition the data amongst many processes.

This algorithm uses a divide and conquer technique and it works very well because merging already sorted arrays is an O(n) operation.

- O(nlogn) - complexity

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

### Binary Search

This algorithm improves lookup in lists that are sorted. Generally, looking for a value in a list has O(n) complexity but if the list is already sorted, this technique can do it in O(logn) which is way better.

This technique uses a divide and conquer approach to reduce the space of lookup by skipping irrelevant values.

- O(logn) - complexity

If the item is not found, the item should be in the position annotated with left.

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

### Two pointers

Technique to traverse an array and find a subarray, it is useful in problems where the naive solution is a double loop. Common problems are related to subarrays or intervals, leveraging a sorted array.

{{< highlight javascript "linenos=table" >}}
// Find subarray with two pointers
let left = 0,
    right = mylist.length - 1;
while (left < right) {
    // do something
    left++;
    right--;
}
{{< / highlight >}}

### Intervals

Intervals represent ranges defined by a start and end point, commonly used in scheduling and range problems. A common approach is to **sort intervals** by start or end times to simplify comparisons and reduce time complexity.

{{< highlight javascript "linenos=table" >}}
function merge_intervals(intervals) {
    // Sort intervals by start
    intervals.sort((a, b) => a[0] - b[0]);

    // Initialize with the first interval
    const merged = [intervals[0]];
    for (const [start, end] of intervals.slice(1)) {
        // Latest merged interval
        const last = merged[merged.length - 1];
        const [s, e] = last;
        // If current interval overlaps
        if (start <= e) {
            last[1] = Math.max(e, end);
        } else {
            merged.push([start, end]);
        }
    }
    return merged;
}
{{< / highlight >}}

## Linked List

A LinkedList is a data structure that stores elements sequentially, but with nodes scattered randomly in memory. Each node contains a value and a pointer to the next node.

LinkedLists allow efficient insertion/deletion at both ends but do not support indexed access.

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

Linked list can be doubly linked and store a pointer to the previous element too.

To iterate through a list, it can check next nodes until it can’t.

{{< highlight javascript "linenos=table" >}}
let curr = head;
while (curr !== null) {
    // Process value
    curr = curr.next_node;
}
{{< / highlight >}}

### Stack and Queue

These are data structures focused in accessing one end. Stack is a Last-in First-Out type collection, whereas a Queue is a First-in First-out collection. They can be easily implemented but JavaScript already has an implementation.

- O(1) - add or remove from the ends
- O(n) - lookup and indexed access

{{< highlight javascript "linenos=table" >}}
// Queue
const queue = [];

queue.push(1);
queue.push(2);
queue.shift(); // -> 1

// Stack
const stack = [];

stack.push(1);
stack.push(2);
stack.pop(); // -> 2
{{< / highlight >}}

## Hashmap

Hashmap is a data structure that stores key/value pairs. The values can be efficiently read using the key. It works by hashing the key to store values in a table.

- O(1) - read and write if key is provided
- O(n) - if key is not provided

{{< highlight javascript "linenos=table" >}}
const hashmap = {};
hashmap['key'] = 'value';

'key' in hashmap; // -> true
'other key' in hashmap; // -> false

hashmap['key']; // -> 'value'

// Iteration formats
for (const [key, value] of Object.entries(hashmap)) {
    // pass
}
for (const value of Object.values(hashmap)) {
    // pass
}
for (const key in hashmap) {
    // pass
}
{{< / highlight >}}

### Hashset

It’s a special case of hashmap in which the values are of type booleans. This data structure is used to represent sets and generally comes with efficient set algebra operations.

- O(1) - read and write a particular value
- O(n) - most of the set operations

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

### Ordered Dictionary

Regular dictionaries do not guarantee any particular order when iterating its values. However, an Ordered Dictionary is a specialized Hashmap that maintains the order of insertion, allowing iteration over the elements in the same sequence in which they were added.

This is achieved by using additional memory to store the insertion order, typically using a linked list.

{{< highlight javascript "linenos=table" >}}
const ordered_dict = new Map();

ordered_dict.set('a', 1);
ordered_dict.set('b', 2);
ordered_dict.set('c', 3);

// Iterating over the Map
for (const [key, value] of ordered_dict.entries()) {
    console.log(key, value);
}
{{< / highlight >}}

## Priority Queue

Priority Queue also known as PQ, is a data structure whose objective is to give access to elements based on their assigned priority. The queue maintains an order of priority such that elements with highest priority (or lowest, depending on the implementation) are always accessed first.

This data structure is commonly implemented with heaps, a special type of binary tree that ensures the highest priority element is efficiently accessible after add or delete operations.

- O(1) - read highest priority element
- O(logn) - insert or delete an element

JavaScript does not have a built-in Priority Queue in its standard library, but you can implement one using a custom class or use third-party libraries.

{{< highlight javascript "linenos=table" >}}
class PriorityQueue {
    constructor() {
        this.heap = [];
    }

    push(element) {
        this.heap.push(element);
        this.heap.sort((a, b) => a[0] - b[0]); // Sort based on priority
    }

    pop() {
        return this.heap.shift();
    }
}

const pq = new PriorityQueue();

// Tuples of priority, values
pq.push([1, 'Charly']);
pq.push([0, 'David']);
pq.push([3, 'John']);

pq.pop(); // -> [0, 'David']
pq.pop(); // -> [1, 'Charly']
{{< / highlight >}}

## Bitmaps

Data structure that efficiently stores bit values in an array, the bits can represent boolean values and the array itself can represent sets. This data structure is less flexible than a set but can be very memory and time efficient.

- O(1) - write and read a binary value
- O(1) - algebra of sets

{{< highlight javascript "linenos=table" >}}
// Can be initialized as integer too.
let bitmap = 0b0000;

// Set bit to 1
const i = 2;
bitmap |= (1 << i);
// -> 0b100

// Set bit to 0
bitmap &= ~(1 << i);
// -> 0b000

// Toggle bit
bitmap ^= (1 << i);
// -> 0b100

// Check bit
((bitmap >> i) & 1);

// Set operations
let A = 0b1010;
let B = 0b1101;

// Intersect
A & B; // -> 0b1000

// Union
A | B; // -> 0b1111

// Difference
A & ~B;

// Check all bits
const numBits = 4;
A & ((1 << numBits) - 1);
{{< / highlight >}}

## Graphs

Data structure that arranges data in terms of its relations.

### Tree

Graph data structure that arranges data in hierarchical format and has no cycles. It is often represented as a class with recursive pointers to its children.

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

### Binary Tree

It is a special case of a tree where each node has at most 2 children. This data structure is usually represented with left and right children.

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

### Depth-First Search

Also known as DFS is an algorithm technique to traverse a graph or tree visiting each path in depth, then visiting other paths. It is useful when the problem requires a comparison of final scenarios of each path.

When traversing a tree, the process of the value can occur before visiting the rest of the path or after, this is called pre-order traversal or post-order traversal.

- O(n) - time complexity
- O(h) - memory complexity, where h is the height of the tree
    - O(n) - worst-case memory complexity, if the tree is skewed
    - O(logn) - memory complexity if the tree is balanced

{{< highlight javascript "linenos=table" >}}
// Iterative implementation of DFS
const stack = [];

// Add the root node to the stack
stack.push(root);

while (stack.length > 0) {
    // Get the current node from the top of the stack
    const curr = stack.pop();

    // Process here if Pre-order traversal

    // Add children to the top of the stack
    if (curr.right) stack.push(curr.right);
    if (curr.left) stack.push(curr.left);

    // Process here if Post-order traversal
}

// Recursive implementation
function DFS(node) {
    if (!node) return;

    // Process here if Pre-order traversal

    // Recursively visit children
    DFS(node.left);
    DFS(node.right);

    // Process here if Post-order traversal
}

// Start by traversing the root
DFS(root);
{{< / highlight >}}

### Breadth-First Search

Also called BFS is a technique to traverse a graph or tree in levels. It can be useful to find the shortest path from the root, but can also be less memory efficient for large paths.

- O(n) - time complexity

{{< highlight javascript "linenos=table" >}}
// Iterative implementation of BFS
const queue = [];

// Add the root node to the queue
queue.push(root);

while (queue.length > 0) {
    // Get the current node from the front of the queue
    const curr = queue.shift();

    // Process here

    // Add children to the end of the queue
    if (curr.left) queue.push(curr.left);
    if (curr.right) queue.push(curr.right);
}
{{< / highlight >}}

### Backtracking

An algorithmic technique to traverse an array and process not only the value but the path taken to the value. This is especially useful when modeling decisions as a tree and comparing outcomes, for example in games.

{{< highlight javascript "linenos=table" >}}
const stack = [];

// Add an empty array as the path and the root node
stack.push([[], root]);

while (stack.length > 0) {
    const [path, curr] = stack.pop();

    // Process path and current node

    // Put the next path and node to the top of the stack
    if (curr.right) {
        stack.push([path.concat(curr.val), curr.right]);
    }
    if (curr.left) {
        stack.push([path.concat(curr.val), curr.left]);
    }
}
{{< / highlight >}}

Backtracking can also be implemented in a recursive manner, and sometimes it can be a little easier to think of this algorithm this way.

{{< highlight javascript "linenos=table" >}}
function BacktrackingWithDFS(path, node) {
    // Process end of graph
    if (!node) return;
    // Optionally process leaf nodes
    // Process path and current node
    if (!node.left && !node.right) {
        // Process leaf node
    }
    BacktrackingWithDFS([...path, node.val], node.left);
    BacktrackingWithDFS([...path, node.val], node.right);
}

BacktrackingWithDFS([], root);
{{< / highlight >}}
