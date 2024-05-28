# 3 Caníbales y 3 Monjes Cruzando el Río

## Solución:
### Transportar los caníbales y los monjes cruzando el río sin que los caníbales superen en número a los monjes en ningún lado.

### Nivel 1:
### Cruzan un monje y un caníbal.
```
[C, C] & [M, M] | [M, C] | [C]
```
### Nivel 2:
### Monje regresa.
```
[C, C, M] & [M] | [C] | [C]
```
### Nivel 3:
### Dos caníbales cruzan.
```
[M] & [M, C, C] | [C] | [C]
```
### Nivel 4:
### Un caníbal regresa.
```
[C, M] & [M, C] | [C] | [C]
```
### Nivel 5:
### Dos monjes cruzan.
```
[C, C] & [] | [M, C] | [M, M, C]
```
### Nivel 6:
### Un caníbal regresa.
```
[C] & [C] | [M] | [M, M, C, C]
```
### Nivel 7:
### Dos caníbales cruzan.
```
[] & [C] | [] | [M, M, M, C, C, C]
```