# Pollo, Coyote, Granos de Maíz y Granjero Cruzando el Río

## Solución:
### Transportar los elementos cruzando el río sin que se coman entre sí.

### Cruza el pollo:
```
[C, G, M] → [P]
```
### Regresa el granjero:
```
[C, G, M] ← [P]
```
### Cruza el coyote:
```
[G, M] → [P, C]
```
### Regresa con el pollo:
```
[G, M, P] ← [C]
```
### Cruza los granos de maíz:
```
[G, P] → [C, M]
```
### Regresa el granjero solo:
```
[G, P] ← [C, M]
```
### Cruza el pollo:
```
[G] → [P, C, M]
```