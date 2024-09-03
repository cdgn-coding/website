---
title: "Resumen de Rangos"
date: "2023-09-03"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
tags: ["algoritmos", "arrays", "rangos"]
---

## Descripción

Se te proporciona un array de enteros `nums` **ordenado y con valores únicos**.

Un **rango** `[a,b]` es el conjunto de todos los enteros desde `a` hasta `b` (inclusive).

Tu tarea es devolver la **lista más pequeña y ordenada** de rangos que **cubran exactamente todos los números en el array**. Es decir, cada elemento de `nums` debe estar cubierto por exactamente uno de los rangos, y no debe haber ningún entero `x` que esté en uno de los rangos pero no en `nums`.

Cada rango `[a,b]` en la lista debe ser presentado como:
* `"a->b"` si `a != b`
* `"a"` si `a == b`

**Entrada**

`nums = [0,1,2,4,5,7]`

**Salida**

`["0->2","4->5","7"]`

**Explicación**

`[0,2] --> "0->2"`
`[4,5] --> "4->5"`
`[7,7] --> "7"`

## Solución

La solución a este problema implica recorrer el array una vez, identificando los inicios y finales de cada rango. El enfoque general es:

1. Inicializar el inicio del rango con el primer elemento del array.

2. Iterar a través del array, comparando cada elemento con el anterior.

3. Si la diferencia entre el elemento actual y el anterior es mayor que 1, hemos encontrado el final de un rango y el inicio de otro.

4. Formatear el rango según las reglas dadas y añadirlo a la lista de resultados.

5. Actualizar el inicio del nuevo rango.

Este enfoque tiene una complejidad temporal de O(n), donde n es la longitud del array de entrada, ya que solo necesitamos recorrer el array una vez. La complejidad espacial es O(n) en el peor caso, donde cada número forma su propio rango.

## Enlaces

* [Problema original en LeetCode](https://leetcode.com/problems/summary-ranges/)
* [Código fuente de la solución en Github](https://github.com/cdgn-coding/leetcode-practice-guide/tree/main/intervals/summary_ranges)