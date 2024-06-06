## Objetivo: Crea una CNN (Convolutional Neural Networks) que sea capaz de detectar situaciones de riesgo.
### 1. Importación de las librerías necesarias
```
import numpy as np
import os
import re
import matplotlib.pyplot as plt
%matplotlib inline
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report
import tensorflow as tf
from tensorflow.keras.utils import to_categorical
from keras.models import Sequential, Model
from tensorflow.keras.layers import Input
from keras.layers import Dense, Dropout, Flatten
from tensorflow.keras.layers import (
    BatchNormalization, SeparableConv2D, MaxPooling2D, Activation, Flatten, Dropout, Dense, Conv2D
)
from keras.layers import LeakyReLU
```
#### Este bloque importa varias bibliotecas necesarias para el manejo de arrays, lectura de archivos, procesamiento de imágenes, creación y entrenamiento de redes neuronales.

### 2. Definición del directorio de imágenes y lectura de imágenes
```
dirname = os.path.join(os.getcwd(), r'D:\IA\Proyecto CNN\Imagenes')
imgpath = dirname + os.sep 
images = []
directories = []
dircount = []
prevRoot=''
cant=0

print("leyendo imagenes de ",imgpath)
```
#### Se define el directorio donde se almacenan las imágenes y se inicializan variables para almacenar las imágenes y la estructura de directorios.

### 3. Recorrido del directorio y lectura de las imágenes
```
for root, dirnames, filenames in os.walk(imgpath):
    for filename in filenames:
        if re.search("\.(jpg|jpeg|png|bmp|tiff)$", filename):
            cant=cant+1
            filepath = os.path.join(root, filename)
            image = plt.imread(filepath)
            if(len(image.shape)==3):
                images.append(image)
            if prevRoot != root:
                print(root, cant)
                prevRoot=root
                directories.append(root)
                dircount.append(cant)
                cant=0
dircount.append(cant)
dircount = dircount[1:]
dircount[0] = dircount[0]+1
print('Directorios leidos:', len(directories))
print("Imagenes en cada directorio", dircount)
print('suma Total de imagenes en subdirs:', sum(dircount))
```
#### Este bloque recorre el directorio especificado, lee las imágenes y almacena las rutas y conteos de imágenes por directorio.

### 4. Creación de etiquetas para las imágenes
```
labels = []
indice = 0
for cantidad in dircount:
    for i in range(cantidad):
        labels.append(indice)
    indice = indice + 1
print("Cantidad etiquetas creadas: ", len(labels))
```
#### Aquí se generan etiquetas para cada imagen en función del directorio en el que se encuentran.

### 5. Asignación de nombres a las etiquetas de riesgo
``` 
risk = []
indice = 0
for directorio in directories:
    name = directorio.split(os.sep)
    print(indice , name[len(name)-1])
    risk.append(name[len(name)-1])
    indice = indice + 1
```
#### Este bloque asigna nombres de categorías de riesgo a las etiquetas.

### 6. Conversión de listas a arrays de numpy
``` 
y = np.array(labels)
X = np.array(images, dtype=np.uint8)
classes = np.unique(y)
nClasses = len(classes)
print('Total number of outputs : ', nClasses)
print('Output classes : ', classes)
```
#### Se convierten las listas de imágenes y etiquetas a arrays de numpy y se imprimen las clases únicas.

### 7. División del dataset en conjuntos de entrenamiento y prueba
``` 
train_X, test_X, train_Y, test_Y = train_test_split(X, y, test_size=0.2)
print('Training data shape : ', train_X.shape, train_Y.shape)
print('Testing data shape : ', test_X.shape, test_Y.shape)
```
#### Se divide el conjunto de datos en conjuntos de entrenamiento y prueba con una proporción de 80-20.

### 8. Normalización de los datos
``` 
train_X = train_X.astype('float32')
test_X = test_X.astype('float32')
train_X = train_X / 255.
test_X = test_X / 255.
plt.imshow(test_X[0,:,:])
```
#### Las imágenes se normalizan al rango [0, 1].

### 9. Codificación one-hot de las etiquetas
``` 
train_Y_one_hot = to_categorical(train_Y)
test_Y_one_hot = to_categorical(test_Y)
print('Original label:', train_Y[0])
print('After conversion to one-hot:', train_Y_one_hot[0])
```
#### Las etiquetas se convierten a formato one-hot para ser usadas en el entrenamiento de la red.

### 10. División del conjunto de entrenamiento en entrenamiento y validación
``` 
train_X, valid_X, train_label, valid_label = train_test_split(train_X, train_Y_one_hot, test_size=0.2, random_state=13)
print(train_X.shape, valid_X.shape, train_label.shape, valid_label.shape)
```
#### El conjunto de entrenamiento se divide nuevamente para crear un conjunto de validación.

### 11. Definición y compilación del modelo de red neuronal
``` 
INIT_LR = 1e-3
epochs = 20
batch_size = 64
risk_model = Sequential()
risk_model.add(Input(shape=(28, 28, 3)))
risk_model.add(Conv2D(32, kernel_size=(3, 3), activation='linear', padding='same'))
risk_model.add(LeakyReLU(negative_slope=0.1))
risk_model.add(MaxPooling2D((2, 2), padding='same'))
risk_model.add(Dropout(0.5))
risk_model.add(Flatten())
risk_model.add(Dense(32, activation='linear'))
risk_model.add(LeakyReLU(negative_slope=0.1))
risk_model.add(Dropout(0.5))
risk_model.add(Dense(nClasses, activation='softmax'))
risk_model.summary()
risk_model.compile(
    loss=tf.keras.losses.categorical_crossentropy,
    optimizer=tf.keras.optimizers.SGD(learning_rate=INIT_LR),
    metrics=['accuracy']
)
```
#### Aquí se define la estructura del modelo CNN, se compila y se imprime un resumen de su arquitectura.

### 12. Entrenamiento del modelo
``` 
risk_train = risk_model.fit(train_X, train_label, batch_size=batch_size, epochs=epochs, verbose=1, validation_data=(valid_X, valid_label))
risk_model.save(r"D:\IA\Proyecto CNN\modelo\risk_model.keras")
```
#### El modelo se entrena con los datos de entrenamiento y se guarda en un archivo.

### 13. Evaluación del modelo en el conjunto de prueba
``` 
test_eval = risk_model.evaluate(test_X, test_Y_one_hot, verbose=1)
print('Test loss:', test_eval[0])
print('Test accuracy:', test_eval[1])
```
#### Se evalúa el rendimiento del modelo en el conjunto de prueba.

### 14. Visualización del desempeño del modelo durante el entrenamiento
``` 
accuracy = risk_train.history['accuracy']
val_accuracy = risk_train.history['val_accuracy']
loss = risk_train.history['loss']
val_loss = risk_train.history['val_loss']
epochs = range(len(accuracy))
plt.plot(epochs, accuracy, 'bo', label='Training accuracy')
plt.plot(epochs, val_accuracy, 'b', label='Validation accuracy')
plt.title('Training and validation accuracy')
plt.legend()
plt.figure()
plt.plot(epochs, loss, 'bo', label='Training loss')
plt.plot(epochs, val_loss, 'b', label='Validation loss')
plt.title('Training and validation loss')
plt.legend()
plt.show()
```
#### Se grafican las curvas de precisión y pérdida tanto para entrenamiento como para validación.

### 15. Predicción y evaluación de las imágenes del conjunto de prueba
``` 
predicted_classes2 = risk_model.predict(test_X)
predicted_classes = []
for predicted_risk in predicted_classes2:
    predicted_classes.append(predicted_risk.tolist().index(max(predicted_risk)))
predicted_classes = np.array(predicted_classes)
print("Found %d correct labels" % len(np.where(predicted_classes == test_Y)[0]))
print(classification_report(test_Y, predicted_classes, target_names=[f"Class {i}" for i in range(nClasses)]))
```
#### Se realizan predicciones sobre el conjunto de prueba y se imprime un informe de clasificación.

### 16. Visualización de algunas predicciones correctas e incorrectas
``` 
correct = np.where(predicted_classes == test_Y)[0]
incorrect = np.where(predicted_classes != test_Y)[0]

for i, idx in enumerate(correct[:9]):
    plt.subplot(3, 3, i+1)
    plt.imshow(test_X[idx].reshape(28, 28, 3), cmap='gray')
    plt.title(f"Real: {risk[test_Y[idx]]} Pred: {risk[predicted_classes[idx]]}")
    plt.tight_layout()
    plt.show()

for i, idx in enumerate(incorrect[:9]):
    plt.subplot(3, 3, i+1)
    plt.imshow(test_X[idx].reshape(28, 28, 3), cmap='gray')
    plt.title(f"Real

: {risk[test_Y[idx]]} Pred: {risk[predicted_classes[idx]]}")
    plt.tight_layout()
    plt.show()
```
#### Se muestran ejemplos de predicciones correctas e incorrectas.

### 17. Prueba del modelo con nuevas imágenes
``` 
from tensorflow.keras.models import load_model
risk_model = load_model('D:\\IA\\Proyecto CNN\\modelo\\risk_model.keras')
risk = ['tornados', 'Asaltos', 'inundaciones', 'incendio', 'robo']
categoria = ['inundacion', 'incendio', 'robo']
filenames = [r'D:\IA\Proyecto CNN\prueba\inundacion6.jpg', r'D:\IA\Proyecto CNN\prueba\incendio6.jpg', r'D:\IA\Proyecto CNN\prueba\robo6.jpg']
images = []

for filepath in filenames:
    image = plt.imread(filepath, 0)
    image_resized = resize(image, (28, 28), anti_aliasing=True, clip=False, preserve_range=True)
    images.append(image_resized)

X = np.array(images, dtype=np.uint8) 
test_X = X.astype('float32')
test_X = test_X / 255.

predicted_classes = risk_model.predict(test_X)

for i, img_tagged in enumerate(predicted_classes):
    print(filenames[i], risk[img_tagged.tolist().index(max(img_tagged))])
    img = plt.imread(filenames[i])
    plt.figure(figsize=(6, 6))
    plt.imshow(img)
    plt.title(f"Real: {categoria[i]}  Predicción: {risk[img_tagged.tolist().index(max(img_tagged))]}")
    plt.show()
```
#### Finalmente, se prueba el modelo con nuevas imágenes para ver las predicciones y se visualizan los resultados.

#### Este conjunto de códigos cubre todo el ciclo de vida de un proyecto de clasificación de imágenes usando redes neuronales convolucionales, desde la lectura de imágenes, pasando por el preprocesamiento y el entrenamiento del modelo, hasta la evaluación y prueba del modelo con nuevas imágenes.