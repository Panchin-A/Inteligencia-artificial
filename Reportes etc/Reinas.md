# 8 Reinas en un Tablero de 8x8

## Solución:
### Colocar 8 reinas en un tablero de ajedrez de 8x8 de manera que ninguna se ataque.

## Procedimiento:
### Nivel 1:
### Coloca la primera reina en una columna de la primera fila.
´´´
[R][ ][ ][ ][ ][ ][ ][ ] 1
[ ][ ][ ][ ][ ][ ][ ][ ] 2
[ ][ ][ ][ ][ ][ ][ ][ ] 3
[ ][ ][ ][ ][ ][ ][ ][ ] 4
[ ][ ][ ][ ][ ][ ][ ][ ] 5
[ ][ ][ ][ ][ ][ ][ ][ ] 6
[ ][ ][ ][ ][ ][ ][ ][ ] 7
[ ][ ][ ][ ][ ][ ][ ][ ] 8
´´´
### Nivel 2:
### Coloca la segunda reina en una columna de la segunda fila que no esté atacada por la reina en la primera fila.
```
[R][ ][ ][ ][ ][ ][ ][ ] 1
[ ][ ][ ][ ][R][ ][ ][ ] 2
[ ][ ][ ][ ][ ][ ][ ][ ] 3
[ ][ ][ ][ ][ ][ ][ ][ ] 4
[ ][ ][ ][ ][ ][ ][ ][ ] 5
[ ][ ][ ][ ][ ][ ][ ][ ] 6
[ ][ ][ ][ ][ ][ ][ ][ ] 7
[ ][ ][ ][ ][ ][ ][ ][ ] 8
```
### Continuar hasta Nivel 8:
### Coloca una reina en cada fila, asegurándote de que no sea atacada por ninguna reina anterior.
```
[R][ ][ ][ ][ ][ ][ ][ ] 1
[ ][ ][ ][ ][R][ ][ ][ ] 2
[ ][ ][ ][ ][ ][ ][ ][R] 3
[ ][ ][ ][ ][ ][R][ ][ ] 4
[ ][ ][R][ ][ ][ ][ ][ ] 5
[ ][ ][ ][ ][ ][ ][ ][R] 6
[ ][R][ ][ ][ ][ ][ ][ ] 7
[ ][ ][ ][R][ ][ ][ ][ ] 8
```
# 4 Reinas en un Tablero de 4x4

## Solución:
### Colocar 4 reinas en un tablero de ajedrez de 4x4 de manera que ninguna se ataque.

## Procedimiento:
### Nivel 1:
### Coloca la primera reina en una columna de la primera fila.
```
[R][ ][ ][ ] 1
[ ][ ][ ][ ] 2
[ ][ ][ ][ ] 3
[ ][ ][ ][ ] 4
```
### Nivel 2:
### Coloca la segunda reina en una columna de la segunda fila que no esté atacada por la reina en la primera fila.
```
[R][ ][ ][ ] 1
[ ][ ][R][ ] 2
[ ][ ][ ][ ] 3
[ ][ ][ ][ ] 4
```
### Nivel 3:
### Coloca la tercera reina en una columna de la tercera fila que no esté atacada por las reinas en las filas anteriores.
```
[R][ ][ ][ ] 1
[ ][ ][R][ ] 2
[ ][ ][ ][ ] 3
[ ][ ][ ][ ] 4
```
### Nivel 4:
### Encuentra una posición para la cuarta reina que no esté atacada.
```
[ ][R][ ][ ] 1
[ ][ ][ ][R] 2
[R][ ][ ][ ] 3
[ ][ ][R][ ] 4
```