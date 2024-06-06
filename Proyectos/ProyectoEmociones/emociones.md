# Codigos para proyecto emociones

# Código 1:
```
import cv2 as cv 
import numpy as np 
import os

dataSet = 'D:\\IA\\Emociones3'
faces  = os.listdir(dataSet)
print(faces)

labels = []
facesData = []
label = 0 
for face in faces:
    facePath = dataSet+'\\'+face
    for faceName in os.listdir(facePath):
        labels.append(label)
        facesData.append(cv.imread(facePath+'\\'+faceName,0))
    label = label + 1
#print(np.count_nonzero(np.array(labels)==0)) 

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.train(facesData, np.array(labels))
faceRecognizer.write('D:\\IA\\xml\\Emociones.xml')
```
## Este código realiza los siguientes pasos:
1. **Importaciones y configuración del directorio**:
   ### - Importa las librerías necesarias (`cv2`, `numpy`, y `os`).
   ### - Define el directorio donde se almacenan las imágenes de emociones.

2. **Lectura de imágenes y etiquetas**:
   ### - Obtiene una lista de subdirectorios en `dataSet`, que representan diferentes categorías de emociones.
   ### - Inicializa listas para etiquetas (`labels`) y datos de caras (`facesData`).
   ### - Recorre cada subdirectorio y lee las imágenes en escala de grises, almacenando las imágenes y asignando una etiqueta numérica a cada categoría.

3. **Entrenamiento del reconocedor de caras**:
   ### - Crea un modelo de reconocimiento de caras utilizando el algoritmo LBPH (Local Binary Patterns Histograms).
   ### - Entrena el modelo con los datos de caras y sus etiquetas.
   ### - Guarda el modelo entrenado en un archivo XML.

# Código 2:
```
import numpy as np
import cv2 as cv
import math 
rostro = cv.CascadeClassifier('C:\\Users\\kickb\\Music\\Jupyter\\haarcascade_frontalface_alt.xml')
cap = cv.VideoCapture("D:\\IA\\Jupyter\\VideoRostro\\PanchoS1.mp4")
i=0
while True:
    ret, frame = cap.read()
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    rostros = rostro.detectMultiScale(gray, 1.3, 5)
    for(x, y, w, h) in rostros:
        frame = cv.rectangle(frame, (x,y), (x+w, y+h), (255, 0, 0), 5)
        frame2=frame[y:y+h, x:x+w]
        cv.imshow('rostros2',frame2)
        frame2 = cv.resize(frame2, (100,100), interpolation = cv.INTER_AREA)
        cv.imwrite('D:\\IA\\Jupyter\\rostro\\Cara'+str(i)+'.png',frame2)
    cv.imshow('rostros', frame)
    i=i+1
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```
## Este código realiza los siguientes pasos:
1. **Importaciones y configuración del detector de rostros**:
   ### - Importa las librerías necesarias (`numpy`, `cv2`).
   ### - Carga un clasificador Haar para detección de rostros.

2. **Captura de video y detección de rostros**:
   ### - Abre un archivo de video para la captura de cuadros.
   ### - En un bucle, lee cada cuadro del video, convierte el cuadro a escala de grises, y detecta rostros en el cuadro.
   ### - Dibuja un rectángulo alrededor de cada rostro detectado, extrae el área del rostro, redimensiona la imagen del rostro a 100x100 píxeles y guarda la imagen redimensionada.
   ### - Muestra los cuadros originales y recortados en ventanas separadas.
   ### - Finaliza el bucle cuando se presiona la tecla 'ESC'.

# Código 3:
```
import cv2 as cv
import os

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('D:\\IA\\xml\\Emociones.xml')
dataSet = 'D:\\IA\\Emociones3'
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
## Este código realiza los siguientes pasos:
1. **Importaciones y configuración del reconocedor de rostros**:
   ### - Importa las librerías necesarias (`cv2`, `os`).
   ### - Carga un modelo LBPH previamente entrenado.
   ### - Carga un clasificador Haar para detección de rostros.
   ### - Abre la cámara web para captura de video.

2. **Detección y reconocimiento de emociones en tiempo real**:
   ### - En un bucle, lee cada cuadro de la cámara web, lo convierte a escala de grises y detecta rostros.
   ### - Extrae y redimensiona la región del rostro detectado.
   ### - Utiliza el modelo LBPH para predecir la emoción del rostro.
   ### - Dibuja un rectángulo alrededor del rostro y muestra la emoción predicha o "Desconocido" si no se reconoce.
   ### - Muestra el cuadro con anotaciones en una ventana.
   ### - Finaliza el bucle cuando se presiona la tecla 'ESC'.

# Código 4:
```
import cv2 as cv
import os 

faceRecognizer = cv.face.LBPHFaceRecognizer_create()
faceRecognizer.read('D:\\IA\\xml\\Emociones.xml')
dataSet = 'D:\\IA\\Emociones3'
faces  = os.listdir(dataSet)
cap = cv.VideoCapture(0)
rostro = cv.CascadeClassifier('C:\\Users\\kickb\\Music\\Jupyter\\haarcascade_frontalface_alt.xml')
while True:
    ret, frame = cap.read()
    if ret == False: break
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cpGray = gray.copy()
    rostros = rostro.detectMultiScale(gray, 1.3, 3)
    for(x, y, w, h) in rostros:
        frame2 = cpGray[y:y+h, x:x+w]
        frame2 = cv.resize(frame2,  (100,100), interpolation=cv.INTER_CUBIC)
        result = faceRecognizer.predict(frame2)
        cv.putText(frame, '{}'.format(result), (x,y-20), 1,3.3, (255,255,0), 1, cv.LINE_AA)
        if result[1] < 100:
            cv.putText(frame,'{}'.format(faces[result[0]]),(x,y-25),2,1.1,(0,255,0),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,255,0),2)
        else:
            cv.putText(frame,'Desconocido',(x,y-20),2,0.8,(0,0,255),1,cv.LINE_AA)
            cv.rectangle(frame, (x,y),(x+w,y+h),(0,0,255),2) 
    cv.imshow('frame', frame)
    k = cv.waitKey(1)
    if k == 27:
        break
cap.release()
cv.destroyAllWindows()
```
## Este código es similar al código 3 con algunas diferencias menores:
1. **Importaciones y configuración del reconocedor de rostros**:
   ### - Importa las librerías necesarias (`cv2`, `os`).
   ### - Carga un modelo LBPH previamente entrenado con otro archivo XML.
   ### - Carga un clasificador Haar para detección de rostros.
   ### - Abre la cámara web para captura de video.

2. **Detección y reconocimiento de emociones en tiempo real**:
   ### - En un bucle, lee cada cuadro de la cámara web, lo convierte a escala de grises y detecta rostros.
   ### - Extrae y redimensiona la región del rostro detectado.
   ### - Utiliza el modelo LBPH para predecir la emoción del rostro.
   ### - Dibuja un rectángulo alrededor del rostro y muestra la emoción predicha o "Desconocido" si no se reconoce.
   ### - Muestra el cuadro con anotaciones en una ventana.
   ### - Finaliza el bucle cuando se presiona la tecla 'ESC'.
