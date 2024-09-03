---
title: "Problemas de algoritmos explicados para entrevistas"
date: "2024-08-22"
toc: true
readTime: true
autonumber: true
math: true
showTags: false
hideBackToTop: true
draft: false
description: "Problemas de algoritmos resueltos con explicación para entrevistas de trabajo en programación"
tags: ["algoritmos", "entrevistas"]
---

Esta lista es una recopilación viva de problemas de algoritmos, curada para pruebas técnicas y para mejorar como programador.

La resolución y explicación de los ejercicios se subirá gradualmente, por lo que si quieres que aborde uno en particular, déjame saber en la caja de comentarios.

Los problemas están clasificados por técnicas y ordenados por dificultad. Cada una de las técnicas se explica conceptualmente en la sección que corresponda.

## Estructuras de datos fundamentales

Las estructuras de datos fundamentales son los bloques de construcción básicos utilizados en la programación. Dominar estas estructuras te  abrirá las puertas a resolver problemas complejos de forma eficiente.

### Arreglos y cadenas de texto

Los arreglos y las cadenas de texto son estructuras de datos lineales que almacenan elementos en posiciones contiguas de memoria. Son fundamentales en la programación y se utilizan en una amplia variedad de algoritmos.

* [Ordenamiento por mezcla (Merge sort)](/tutoriales/2024-09-03-150-problemas-de-leetcode-resoluciones/arreglos-y-cadenas/ordenamiento-por-mezcla-merge-sort)
* Eliminar elemento
* Eliminar duplicados de un arreglo ordenado
* Eliminar duplicados de un arreglo ordenado II

### Mapas de hash

Los mapas de hash, también conocidos como tablas hash o diccionarios, son estructuras de datos que permiten almacenar pares clave-valor y acceder a ellos de manera eficiente. Son excelentes para búsquedas rápidas y eliminación de duplicados.

* [Nota de rescate](/tutoriales/2024-09-03-150-problemas-de-leetcode-resoluciones/tablas-hash/notas-de-rescate)
* Cadenas isomórficas
* Patrón de palabras
* Agrupar anagramas
* Secuencia consecutiva más larga

### Matrices

Las matrices son estructuras de datos bidimensionales que organizan elementos en filas y columnas. Son útiles para representar datos tabulares, imágenes y problemas geométricos.

* Sudoku válido
* Matriz en espiral
* Rotar imagen
* Establecer ceros en la matriz
* Juego de la vida

### Pilas

Las pilas son estructuras de datos lineales que siguen el principio de "último en entrar, primero en salir" (LIFO). Son útiles para rastrear el historial de operaciones, validar expresiones y realizar búsquedas en profundidad.

* Paréntesis válidos
* Simplificar ruta
* Pila mínima
* Evaluar notación polaca inversa
* Calculadora básica

## Listas enlazadas

Las listas enlazadas son estructuras de datos lineales donde cada elemento (nodo) contiene un valor y un puntero al siguiente elemento. Son útiles cuando se necesita inserción y eliminación eficientes en cualquier posición de la lista.

* Ciclo en lista enlazada
* Sumar dos números
* Fusionar dos listas ordenadas
* Invertir lista enlazada II
* Eliminar duplicados
* Rotar lista
* Particionar lista
* Caché LRU
* Invertir nodos en grupos de k

## Técnicas de recorrido

Las técnicas de recorrido son métodos utilizados para explorar y procesar elementos en estructuras de datos de manera eficiente. Estas técnicas son fundamentales para resolver una amplia gama de problemas algorítmicos.

### Dos punteros

La técnica de dos punteros implica utilizar dos referencias para recorrer una estructura de datos, generalmente moviéndose a diferentes velocidades o direcciones. Es útil para problemas que involucran búsqueda, ordenamiento o manipulación de secuencias.

* Palíndromo válido
* Es subsecuencia
* Suma de dos II
* Contenedor con más agua
* 3Sum (Suma de tres)

### Ventanas deslizantes

La técnica de ventanas deslizantes implica mantener un subconjunto de elementos (una "ventana") mientras se recorre una secuencia más grande. Es útil para problemas que requieren encontrar subarreglos o subcadenas que cumplan ciertas condiciones.

* Subarreglo de tamaño mínimo con suma
* Subcadena más larga sin caracteres repetidos
* Subcadena con concatenación de todas las palabras
* Ventana mínima que contiene subcadena

### Intervalos

Los problemas de intervalos implican trabajar con rangos o períodos de tiempo. Estas técnicas son útiles para resolver problemas de programación, optimización de recursos y análisis de datos temporales.

* [Rangos de resumen](/tutoriales/2024-09-03-150-problemas-de-leetcode-resoluciones/intervalos/rangos-resumen)
* Fusionar intervalos
* Insertar intervalo
* Número mínimo de flechas para reventar globos

### Búsqueda binaria

La búsqueda binaria es un algoritmo eficiente para encontrar un elemento en una colección ordenada. Funciona dividiendo repetidamente el espacio de búsqueda a la mitad, lo que resulta en una complejidad logarítmica.

* Posición de inserción de búsqueda
* Buscar en una matriz 2D
* Encontrar elemento pico
* Buscar en arreglo ordenado rotado
* Encontrar primera y última posición en arreglo ordenado
* Encontrar mediana de dos arreglos ordenados

## Grafos

Los grafos son estructuras de datos que consisten en vértices (nodos) y aristas que conectan estos vértices. Son útiles para modelar relaciones entre objetos y resolver problemas de conectividad, rutas y flujos.

### Árbol binario

Un árbol binario es una estructura de datos jerárquica donde cada nodo tiene como máximo dos hijos. Son útiles para representar jerarquías, tomar decisiones binarias y optimizar búsquedas.

* [Iterador de árbol binario](/tutoriales/2024-09-03-150-problemas-de-leetcode-resoluciones/arboles-binarios/iterador-de-arbol-binario)
* [Profundidad máxima de un árbol binario](/tutoriales/2024-09-03-150-problemas-de-leetcode-resoluciones/arboles-binarios/profundidad-maxima-de-un-arbol-binario)
* Mismo árbol
* Invertir árbol binario
* Suma de camino
* [Construir árbol binario a partir de recorridos preorden e inorden](/tutoriales/2024-09-03-150-problemas-de-leetcode-resoluciones/arboles-binarios/construir-a-partir-de-preorden-e-inorden)
* Construir árbol binario a partir de recorridos inorden y postorden
* Aplanar árbol binario a lista enlazada
* Suma máxima de camino en árbol binario

### Grafos en general

Los grafos generales pueden tener diferentes formas y propiedades. Esta sección aborda problemas que involucran grafos dirigidos, no dirigidos, ponderados y no ponderados.

* Número de islas
* Regiones rodeadas
* Clonar grafo
* Horario de cursos
* Horario de cursos II

### Recorrido en anchura

El recorrido en anchura (BFS) es un algoritmo para explorar grafos o árboles nivel por nivel. Es útil para encontrar el camino más corto y resolver problemas que requieren explorar nodos cercanos primero.

* Serpientes y escaleras
* Mutación genética mínima
* Escalera de palabras
* Niveles promedio en árbol binario
* Vista derecha de árbol binario
* Zigzag de árbol binario

### Árboles binarios de búsqueda

Un árbol binario de búsqueda (BST) es un tipo especial de árbol binario donde los nodos están ordenados de manera que facilita la búsqueda, inserción y eliminación eficientes.

* Diferencia absoluta mínima en BST
* K-ésimo elemento más pequeño en un BST
* Validar árbol binario de búsqueda

## Estructuras avanzadas

Las estructuras de datos avanzadas son herramientas más sofisticadas que se utilizan para resolver problemas complejos de manera eficiente. Estas estructuras a menudo combinan o extienden las estructuras fundamentales para proporcionar funcionalidades específicas.

### Trie

Un Trie, también conocido como árbol de prefijos, es una estructura de datos de árbol utilizada para almacenar y buscar cadenas de manera eficiente. Es particularmente útil para problemas que involucran prefijos comunes y búsqueda de palabras.

* Implementar trie
* Diseñar estructura de datos para buscar palabras
* Búsqueda de palabras II

### Montículos (Heap)

Un montículo es una estructura de datos de árbol especializada que satisface la propiedad de montículo: si P es un nodo padre de C, entonces la clave de P es ordenada con respecto a la clave de C. Son útiles para problemas que involucran la búsqueda eficiente del elemento mínimo o máximo.

* K-ésimo elemento más grande en un arreglo
* Encontrar K pares con suma más pequeña
* IPO
* Encontrar mediana de flujo de datos

## Técnicas algorítmicas

Las técnicas algorítmicas son estrategias generales para diseñar soluciones eficientes a problemas complejos. Estas técnicas proporcionan marcos para abordar una amplia gama de desafíos de programación.

### Backtracking

El backtracking es una técnica algorítmica para resolver problemas de manera recursiva intentando construir una solución incrementalmente, una pieza a la vez, y abandonando las soluciones que no satisfacen las restricciones del problema.

* Combinaciones de letras de un número de teléfono
* Combinaciones
* Permutación
* Suma de combinaciones
* Búsqueda de palabras
* N-Reinas II

### Divide y conquistarás

Divide y conquistarás es una técnica que resuelve un problema dividiéndolo en subproblemas más pequeños, resolviendo estos subproblemas, y luego combinando las soluciones para resolver el problema original.

* Convertir arreglo ordenado a árbol binario de búsqueda
* Ordenar lista
* Árbol cuádruple
* Fusionar k listas ordenadas

### Programación dinámica

La programación dinámica es una técnica para resolver problemas complejos dividiéndolos en subproblemas más simples y almacenando los resultados de estos subproblemas para evitar cálculos repetidos.

* Subir escaleras
* Ladrón de casas
* Ruptura de palabras
* Cambio de monedas
* Triángulo
* Suma de camino mínimo
* Caminos únicos II
* Subcadena palindrómica más larga
* Entrelazado de cadenas
* Mejor momento para comprar y vender acciones III
* Mejor momento para comprar y vender acciones IV

### Algoritmo de Kadane

El algoritmo de Kadane es una técnica de programación dinámica utilizada para encontrar el subarreglo contiguo con la suma máxima dentro de un arreglo unidimensional de números.

* Subarreglo máximo
* Suma máxima de subarreglo circular

## Temas especiales

Los temas especiales cubren áreas específicas de la ciencia de la computación que requieren técnicas y conocimientos particulares para resolver problemas eficientemente.

### Manipulación de bits

La manipulación de bits implica la aplicación de operaciones a nivel de bit para resolver problemas de manera eficiente. Es útil en optimización de memoria, criptografía y problemas que involucran representaciones binarias.

* Suma binaria
* Invertir bits
* Número de bits 1
* Número único
* Número único II
* AND bit a bit de rango de números

### Matemáticas

Los problemas matemáticos en programación a menudo requieren la aplicación de conceptos matemáticos y propiedades numéricas para desarrollar algoritmos eficientes.

* Número palíndromo
* Más uno
* Ceros finales de factorial
* Raíz cuadrada(x)
* Potencia(x, n)
* Máximo de puntos en una línea