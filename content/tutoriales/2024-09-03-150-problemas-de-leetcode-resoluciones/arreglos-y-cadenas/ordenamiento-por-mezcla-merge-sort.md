---
title: "Ordenamiento por mezcla (Merge sort)"
date: "2024-08-22"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
tags: ["algoritmos", "entrevistas"]
---

## Descripción

Se te dan dos arreglos de enteros `nums1` y `nums2`, ordenados en **orden no decreciente**, y dos enteros $m$ y $n$, que representan el número de elementos en `nums1` y `nums2` respectivamente.

**Fusiona** `nums1` y `nums2` en un solo arreglo ordenado en **orden no decreciente**.

El arreglo final ordenado no debe ser devuelto por la función, sino que debe *almacenarse dentro del arreglo* `nums1`. 

Para esto, `nums1` tiene una longitud de $m + n$, donde los primeros $m$ elementos denotan los elementos que deben fusionarse, y los últimos $n$ elementos se establecen en $0$ y deben ignorarse. `nums2` tiene una longitud de $n$.

¿Puedes proponer un algoritmo que se ejecute en tiempo $O(m + n)$?

**Entrada** 

- `nums1 = [1,2,3,0,0,0]`
- `m = 3`
- `nums2 = [2,5,6]`
- `n = 3`

**Salida**

Por referencia, el valor de `num1` queda `[1,2,2,3,5,6]`

**Explicación:** 

Los arreglos que estamos fusionando son `[1,2,3]` y `[2,5,6]`.
El resultado de la fusión es `[1,2,2,3,5,6]` con los elementos **1**, **2**, **3** provenientes de `nums1`.

## Solución

Para resolver este problema de manera eficiente, utilizaremos un enfoque de "fusión hacia atrás". Comenzaremos desde el final de ambos arreglos y colocaremos los elementos más grandes al final de `nums1`.

1. Inicializamos tres punteros

   - `i`: apunta al último elemento válido en `nums1` (índice `m - 1`)

   - `j`: apunta al último elemento en `nums2` (índice `n - 1`)

   - `currentPos`: apunta a la última posición en `nums1` donde escribiremos (índice `m + n - 1`)

2. Iteramos mientras `currentPos` sea mayor que -1, lo que nos asegura que procesamos todos los elementos

   - Comprobamos cual de las listas `nums1` o `nums2` tiene el siguiente mayor elemento.

   - Colocamos el elemento en la lista resultante `nums1`.

   - Decrementamos el puntero de la lista de la cual tomamos el elemento, `i` o `j`.

   - Decrementamos el puntero `currentPos` de la lista resultante.

## Enlaces

* [Problema original en LeetCode](https://leetcode.com/problems/merge-sorted-array/)
* [Código de la solución en Github](https://github.com/cdgn-coding/leetcode-practice-guide/blob/main/arrays_strings/merge_sort/solution.py)