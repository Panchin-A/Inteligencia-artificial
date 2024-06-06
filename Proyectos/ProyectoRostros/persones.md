

# Código 1:
## Este código genera un archivo XML que contiene un modelo de reconocimiento facial utilizando el algoritmo LBPH (Local Binary Patterns Histograms).

```
import cv2 as cv 
import numpy as np 
import os

dataSet = 'D:\\IA\\Persones'
faces = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet + '\\' + face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath + '\\' + faceName, 0))
    label = label + 1

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('D:\\IA\\xml\\PersonesRostros.xml')
```

**Pasos del código:**

1. **Importaciones**:
   ### - Importa las librerías necesarias (`cv2`, `numpy`, `os`).

2. **Definición del dataset**:
   ### - Define la ruta del dataset que contiene las imágenes de las personas.

3. **Listar carpetas**:
   ### - Obtiene la lista de carpetas dentro del directorio del dataset, donde cada carpeta representa una persona diferente.

4. **Preparar datos y etiquetas**:
   ### - Inicializa listas para las etiquetas (`labels`) y los datos de las caras (`facesData`).
   ### - Itera sobre cada carpeta (persona) y, dentro de cada carpeta, sobre cada imagen.
   ### - Para cada imagen, lee la imagen en escala de grises y la añade a `facesData`.
   ### - Añade una etiqueta correspondiente a `labels`.

5. **Entrenamiento del modelo**:
   ### - Crea un reconocedor de caras usando el método LBPH.
   ### - Entrena el reconocedor con los datos de las caras y las etiquetas.

6. **Guardar el modelo**:
   ### - Guarda el modelo entrenado en un archivo XML.

# Código 2:
## Este código utiliza el archivo XML generado para reconocer caras en tiempo real usando la cámara.

```
import cv2 as cv
import os

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('D:\\IA\\xml\\PersonesRostros.xml')
dataSet = 'D:\\IA\\Persones'
faces = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\kickb\\Music\\Jupyter\\haarcascade_frontalface_alt.xml')

while True:
    ret, frame = cap.read()
    if ret == False: 
        break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    
    for (x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2, (100, 100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        
        if result[1] < 100 and result[0] < len(faces):
            emotion = faces[result[0]]
            cv.putText(frame, emotion, (x, y-25), 2, 1.1, (0, 255, 0), 1, cv.LINE_AA)
            cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
        else:
            cv.putText(frame, 'Desconocido', (x, y-20), 2, 0.8, (0, 0, 255), 1, cv.LINE_AA)
            cv.rectangle(frame, (x, y), (x+w, y+h), (0, 0, 255), 2)
    
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break

cap.release()
cv.destroyAllWindows()
```

**Pasos del código:**

1. **Importaciones**:
   ### - Importa las librerías necesarias (`cv2`, `os`).

2. **Carga del modelo entrenado**:
   ### - Crea un reconocedor de caras usando LBPH.
   ### - Carga el modelo entrenado desde el archivo XML.

3. **Definición del dataset**:
   ### - Define la ruta del dataset y obtiene la lista de carpetas, donde cada carpeta representa una persona diferente.

4. **Captura de video**:
   ### - Inicia la captura de video desde la cámara.

5. **Cargado del clasificador Haar**:
   ### - Carga el clasificador Haar para detectar caras.

6. **Proceso en bucle**:
   ### - Lee fotogramas de la cámara en un bucle.
   ### - Si la lectura es exitosa, convierte el fotograma a escala de grises.
   ### - Detecta las caras en el fotograma.
   ### - Para cada cara detectada:
   ###  - Recorta la región de la cara y la redimensiona.
   ###  - Usa el reconocedor de caras para predecir la identidad.
   ###  - Si la predicción es confiable (valor de confianza bajo) y el índice está dentro del rango de la lista de personas, muestra la etiqueta de la persona.
   ###  - Si la predicción no es confiable, muestra "Desconocido".
   ###  - Dibuja un rectángulo alrededor de la cara.

7. **Mostrar el fotograma**:
  ### - Muestra el fotograma con las detecciones.
  ### - Si se presiona la tecla 'Esc' (código 27), rompe el bucle y finaliza el programa.

8. **Liberar recursos**:
  ### - Libera la cámara y cierra todas las ventanas.