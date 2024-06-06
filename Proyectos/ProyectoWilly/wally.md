# Codigos para correr wally

# Código 1:
```
import numpy as np
import cv2 as cv
import os
import math

image = cv.imread(r'D:\\IA\\Proyecto wally\\wally_12.jpg')
gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)

wally = cv.CascadeClassifier(r'D:\\IA\\Proyecto wally\\cascade1.xml')

waldo_detections = wally.detectMultiScale(gray, 1.1, 20, minSize=(40,40))

for (x, y, w, h) in waldo_detections:
    cv.rectangle(image, (x, y), (x+w, y+h), (0, 255, 0), 2)
    cv.putText(image, 'Wally', (x, y-10), cv.FONT_HERSHEY_SIMPLEX, 0.9, (0, 255, 0), 2)

cv.imshow('¿Donde esta Wally?', image)
cv.waitKey(0)
cv.destroyAllWindows()
```
## Este código realiza los siguientes pasos:

1. **Importaciones**:
   ### - Importa las librerías necesarias (`numpy`, `cv2`, `os`, y `math`).

2. **Carga y conversión de la imagen**:
   ### - Lee una imagen de un archivo y la convierte a escala de grises.
   
3. **Carga del clasificador en cascada**:
   ### - Carga un clasificador en cascada específico para detectar a Wally desde un archivo XML.

4. **Detección de Wally**:
   ### - Utiliza el clasificador para detectar regiones en la imagen que pueden contener a Wally.
   ### - `detectMultiScale` realiza la detección de objetos en la imagen en escala de grises con los siguientes parámetros:
   ### - `scaleFactor=1.1`: El factor por el cual la imagen se reduce en cada escala de la pirámide.
   ### - `minNeighbors=20`: Especifica cuántos vecinos deben estar presentes para retener una detección.
   ### - `minSize=(40, 40)`: Tamaño mínimo de la región para ser considerada una detección.

5. **Dibujo de rectángulos y etiquetado**:
   ### - Recorre cada detección y dibuja un rectángulo verde alrededor de la región detectada.
   ### - Añade la etiqueta 'Wally' encima de cada rectángulo.

6. **Mostrar la imagen**:
   ### - Muestra la imagen con las detecciones y espera a que se presione una tecla para cerrar la ventana.

# Código 2:
```
import cv2 as cv

# Cargar la imagen
image = cv.imread(r'D:\IA\Proyecto wally\wally_10.png')

# Convertir la imagen a escala de grises
gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)

# Cargar el clasificador en cascada
wally = cv.CascadeClassifier(r'D:\IA\Proyecto wally\cascade.xml')

# Detectar a Wally
waldo_detections = wally.detectMultiScale(gray, scaleFactor=1.5, minNeighbors=2, minSize=(5, 5))

# Dibujar rectángulos alrededor de las detecciones
for (x, y, w, h) in waldo_detections:
    cv.rectangle(image, (x, y), (x + w, y + h), (0, 255, 0), 2)
    cv.putText(image, 'Wally', (x, y - 10), cv.FONT_HERSHEY_SIMPLEX, 0.9, (0, 255, 0), 2)

# Obtener el tamaño de la pantalla
screen_res = 1280, 720  # Puedes ajustar estas dimensiones según la resolución de tu pantalla
scale_width = screen_res[0] / image.shape[1]
scale_height = screen_res[1] / image.shape[0]
scale = min(scale_width, scale_height)

# Calcular el nuevo tamaño de la imagen
window_width = int(image.shape[1] * scale)
window_height = int(image.shape[0] * scale)

# Redimensionar la imagen
resized_image = cv.resize(image, (window_width, window_height))

# Mostrar la imagen redimensionada
cv.imshow('¿Dónde está Wally?', resized_image)
cv.waitKey(0)
cv.destroyAllWindows()
```
## Este código realiza los siguientes pasos:

1. **Importaciones**:
   ### - Importa la librería `cv2`.

2. **Carga y conversión de la imagen**:
   ### - Lee una imagen de un archivo y la convierte a escala de grises.

3. **Carga del clasificador en cascada**:
   ### - Carga un clasificador en cascada específico para detectar a Wally desde un archivo XML.

4. **Detección de Wally**:
   ### - Utiliza el clasificador para detectar regiones en la imagen que pueden contener a Wally.
   ### - `detectMultiScale` realiza la detección de objetos en la imagen en escala de grises con los siguientes parámetros:
   ###  - `scaleFactor=1.5`: El factor por el cual la imagen se reduce en cada escala de la pirámide.
   ###  - `minNeighbors=2`: Especifica cuántos vecinos deben estar presentes para retener una detección.
   ###  - `minSize=(5, 5)`: Tamaño mínimo de la región para ser considerada una detección.

5. **Dibujo de rectángulos y etiquetado**:
   ### - Recorre cada detección y dibuja un rectángulo verde alrededor de la región detectada.
   ### - Añade la etiqueta 'Wally' encima de cada rectángulo.

6. **Redimensionamiento de la imagen para ajustarla a la pantalla**:
   ### - Calcula la escala de redimensionamiento basada en la resolución de la pantalla (1280x720).
   ### - Redimensiona la imagen según la escala calculada.

7. **Mostrar la imagen**:
   ### - Muestra la imagen redimensionada con las detecciones y espera a que se presione una tecla para cerrar la ventana.