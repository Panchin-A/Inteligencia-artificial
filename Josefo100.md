# Problema de 100 personas de flavio josefo

## Secuencia de percepcion

### Supongamos que hay un círculo de personas numeradas de 1 a n. Comenzando por una persona específica, se eliminan cada cierto número de personas en sentido horario hasta que solo queda una persona. El problema consiste en determinar qué posición ocupa esa persona final.

## Secuencia paso a paso para rsolver el problema:

### 1.-Identifica el número total de personas, que en este caso es 40.
### 2.-Determina el número de personas que se eliminarán en cada paso. Este número se suele denotar como k. Puede ser cualquier valor, pero para este ejemplo, asumiremos k = 1.
### 3.-Comienza con la primera persona en el círculo.
### 4.-Elimina la tercera persona en el sentido de las agujas del reloj.
### 5.-Continúa eliminando cada tercera persona en el sentido de las agujas del reloj hasta que quede una sola persona.

## Medida de rendimiento

### A continuación, te mostraré la secuencia de eliminación para resolver el problema de Flavio Josefo con 40 personas y k = 1:

### 1.-Comenzamos con 40 personas: 

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 

### 2.-Eliminamos a la primera persona:

 1 3 5 7 9 11 13 15 17 19 21 23 25 27 29 31 33 35 37 39 

### 3.-Continuamos eliminando cada primera persona:

1 5 9 13 17 21 25 29 33 37  

### 4.-Seguimos eliminando:

 1 9 17 25 33  

### 5.-Seguimos eliminando:

1 17 33 

### 6.-Seguimos eliminando:

17 

## Si se inicia en 1 el que sobrevive es 17

​

