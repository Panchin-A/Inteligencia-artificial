# Para resolver el problema de los 100 prisioneros y 100 cajas con la estrategia óptima,se siguen estos pasos:

## Problema:
### 100 prisioneros: Cada uno tiene un número del 1 al 100.
### 100 cajas: Cada caja contiene un número del 1 al 100, de forma aleatoria.
### Regla: Cada prisionero puede abrir 50 cajas. Si todos encuentran su número, se salvan; si uno falla, todos mueren.

## Estrategia Óptima:
### Cada prisionero empieza abriendo la caja con su propio número.
### Dentro de esa caja, hay un número que indica la siguiente caja a abrir.
### Continúa abriendo cajas siguiendo los números encontrados hasta 50 intentos.

## Ejemplo:
### Prisionero 1: Abre la caja 1, encuentra el número 45.
### Prisionero 1: Abre la caja 45, encuentra el número 20.
### Prisionero 1: Continúa este proceso hasta 50 intentos o hasta encontrar su propio número.

## Ciclos en Permutaciones:
### Permutaciones crean "ciclos". Un ciclo es una serie de números que referencian otros números en una secuencia cerrada.
### Ejemplo de ciclo: 1 → 45 → 20 → 1 (si prisionero 1 encuentra su número en este ciclo).

## Probabilidad de Éxito:
### La clave es que si todos los ciclos en la permutación de números tienen una longitud de 50 o menos, todos los prisioneros encontrarán su número.
### Matemáticamente, la probabilidad de que todos los ciclos sean de longitud 50 o menos es aproximadamente 31%.

## Conclución:
### Cada prisionero sigue una cadena de números empezando por su propio número.
### La probabilidad de éxito depende de la estructura de los ciclos en la permutación de los números.
### El éxito ocurre si todos los ciclos tienen longitud ≤ 50, con una probabilidad del 31%.
### Esta estrategia maximiza la probabilidad de éxito en comparación con la búsqueda aleatoria, que tiene una probabilidad extremadamente baja.