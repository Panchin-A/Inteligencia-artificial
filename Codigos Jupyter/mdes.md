# Clasif haarascade
1. **Importación de librerías:**
   ```
   import numpy as np
   import cv2 as cv
   ```
   Estas líneas importan las librerías necesarias para trabajar con matrices y con imágenes/vídeos respectivamente.

2. **Cargar el clasificador de rostros:**
   ```
   rostro = cv.CascadeClassifier('C:\\Users\\david\\Desktop\\IA\\haarcascade_frontalface_alt.xml')
   ```
   Esta línea carga el archivo XML que contiene los parámetros para la detección de rostros usando el clasificador Haar.

3. **Iniciar la captura de vídeo:**
   ```
   cap = cv.VideoCapture(0)
   ```
   Esto inicializa la captura de vídeo desde la cámara web (índice 0).

4. **Definir variables iniciales:**
   ```
   x=y=w=h= 0 
   img = 0
   count = 0
   i=0
   ```
   Estas son variables iniciales que se usarán en el procesamiento de la imagen y para contar los rostros detectados.

5. **Primer bucle `while` para detección de rostros:**
   ```
   while True:
       ret, frame = cap.read()
       gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
       rostros = rostro.detectMultiScale(gray, 1.3, 5)
       for(x, y, w, h) in rostros:
           m = int(h / 2)
           frame = cv.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
       
       cv.imshow('rostros', frame)
       i = i + 1
       k = cv.waitKey(1)
       if k == 27:
           break
   cap.release()
   cv.destroyAllWindows()
   ```

   - Captura un fotograma de la cámara.
   - Convierte el fotograma a escala de grises.
   - Usa el clasificador Haar para detectar rostros en el fotograma.
   - Dibuja un rectángulo verde alrededor de cada rostro detectado.
   - Muestra el fotograma con los rostros detectados en una ventana llamada 'rostros'.
   - Sale del bucle si se presiona la tecla `Esc` (código 27).

6. **Reiniciar la captura de vídeo:**
   ```
   cap = cv.VideoCapture(0)
   i = 0
   ```

   - Reinicia la captura de vídeo desde la cámara.
   - Reinicia el contador de imágenes.

7. **Segundo bucle `while` para detección de rostros y guardado de imágenes:**
   ```
   while True:
       ret, frame = cap.read()
       gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
       rostros = rostro.detectMultiScale(gray, 1.3, 5)
       for(x, y, w, h) in rostros:
           frame = cv.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)
           frame2 = frame[y:y+h, x:x+w]
           cv.imshow('rostro2', frame2)
           cv.imwrite('C:\\Users\\david\\Desktop\\IA\\Rostros\\Cara'+str(i)+'.png', frame2)
       cv.imshow('rostros', frame)
       i = i + 1
       k = cv.waitKey(1)
       if k == 27:
           break
   cap.release()
   cv.destroyAllWindows()
   ```

   - Captura un fotograma de la cámara.
   - Convierte el fotograma a escala de grises.
   - Usa el clasificador Haar para detectar rostros en el fotograma.
   - Dibuja un rectángulo verde alrededor de cada rostro detectado.
   - Extrae el área del rostro del fotograma y la muestra en una ventana llamada 'rostro2'.
   - Guarda el área del rostro extraída como una imagen PNG en el directorio especificado.
   - Muestra el fotograma con los rostros detectados en una ventana llamada 'rostros'.
   - Incrementa el contador de imágenes y sale del bucle si se presiona la tecla `Esc` (código 27).

# Cod_IMG_VD_21_02

### Primer bloque de código:
Este código carga una imagen, la convierte al espacio de color HSV, crea máscaras para ciertos rangos de color, y aplica estas máscaras a la imagen original.

1. **Carga de la imagen:**
   ```
   img = cv.imread('C:\\Users\\david\\Pictures\\Saved Pictures\\tr.PNG', 1)
   ```
   Esta línea carga una imagen desde la ruta especificada.

2. **Conversión de la imagen a HSV:**
   ```
   img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
   ```
   Convierte la imagen de BGR (formato predeterminado de OpenCV) a HSV (Hue, Saturation, Value).

3. **Definición de rangos de color:**
   ```
   ubb = (0, 60, 60)
   uba = (19, 255, 255)
   ubb1 = (170, 60, 60)
   uba1 = (180, 255, 255)
   ```
   Define dos rangos de color en el espacio HSV. Estos rangos se usan para detectar colores específicos en la imagen.

4. **Creación de máscaras:**
   ```
   mask1 = cv.inRange(img2, ubb, uba)
   mask2 = cv.inRange(img2, ubb1, uba1)
   mask3 = mask1 + mask2
   ```
   `mask1` y `mask2` son máscaras binarias que indican qué píxeles de la imagen están dentro de los rangos de color definidos. `mask3` es la suma de ambas máscaras.

5. **Aplicación de la máscara a la imagen:**
   ```
   img3 = cv.bitwise_and(img, img, mask=mask3)
   ```
   Aplica la máscara `mask3` a la imagen original, mostrando solo las áreas que están dentro de los rangos de color definidos.

6. **Mostrar las imágenes:**
   ```
   cv.imshow('img2', img2)
   cv.imshow('img', img)
   cv.imshow('img3', img3)
   cv.imshow('mask1', mask1)
   cv.imshow('mask2', mask2)
   cv.imshow('mask3', mask3)
   ```
   Muestra las imágenes y máscaras en ventanas separadas.

7. **Esperar y cerrar ventanas:**
   ```
   cv.waitKey(0)
   cv.destroyAllWindows()
   ```
   Espera a que se presione una tecla y luego cierra todas las ventanas.

### Segundo bloque de código:
Este código captura vídeo de la cámara web y muestra los diferentes canales de color (rojo, verde, azul) en ventanas separadas.

1. **Importar librería y capturar vídeo:**
   ```
   import cv2 as cv 
   cap = cv.VideoCapture(0)
   ```
   Importa la librería OpenCV y inicializa la captura de vídeo desde la cámara web.

2. **Bucle para procesar cada fotograma:**
   ```
   while(True):
       ret, img = cap.read()
       r, g, b = cv.split(img)
       if ret == True:
           cv.imshow('video', img)
           cv.imshow('r', r)
           cv.imshow('g', g)
           cv.imshow('b', b)
           k = cv.waitKey(1) & 0xFF
           if k == 27:
               break
       else:
           break
   cap.release()
   cv.destroyAllWindows()
   ```
   - **Captura y procesamiento de fotogramas:**
     - `ret, img = cap.read()` captura un fotograma de la cámara.
     - `r, g, b = cv.split(img)` divide el fotograma en sus componentes de color rojo, verde y azul.
   
   - **Mostrar fotogramas y componentes de color:**
     - `cv.imshow('video', img)` muestra el fotograma original.
     - `cv.imshow('r', r)` muestra el canal rojo.
     - `cv.imshow('g', g)` muestra el canal verde.
     - `cv.imshow('b', b)` muestra el canal azul.
   
   - **Esperar por la tecla `Esc` para salir:**
     - `k = cv.waitKey(1) & 0xFF` espera una tecla por 1 ms.
     - Si se presiona la tecla `Esc` (código 27), el bucle se rompe y se cierra el programa.

   - **Liberar recursos y cerrar ventanas:**
     - `cap.release()` libera la captura de vídeo.
     - `cv.destroyAllWindows()` cierra todas las ventanas de OpenCV.

# Cod_VID_22_02

## Este código captura vídeo en tiempo real desde la cámara web, filtra la imagen para mostrar solo los colores dentro de un rango específico en el espacio de color HSV, y muestra tanto la imagen original como la imagen filtrada.

1. **Importación de la librería y configuración de la captura de vídeo:**
   ```
   import cv2 as cv

   cap = cv.VideoCapture(0)
   ```
   Estas líneas importan la librería OpenCV y configuran la captura de vídeo desde la cámara web (índice 0).

2. **Bucle para procesamiento de vídeo en tiempo real:**
   ```
   while(True):
       res , img = cap.read()
       img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
       vb = (30, 85, 90)
       va = (110, 255, 255)
       mask = cv.inRange(img2, vb, va)
       res = cv.bitwise_and(img, img, mask=mask)
       cv.imshow('captura', res)
       cv.imshow('real', img)
       if cv.waitKey(1) & 0xFF == ord('s'):
           break
       
   cap.release()
   cv.destroyAllWindows()
   ```

   Vamos a desglosar cada parte del bucle:

   - **Captura de fotograma:**
     ```
     res, img = cap.read()
     ```
     Captura un fotograma de la cámara web. `res` es un valor booleano que indica si la captura fue exitosa, y `img` es el fotograma capturado.

   - **Conversión a espacio de color HSV:**
     ```
     img2 = cv.cvtColor(img, cv.COLOR_BGR2HSV)
     ```
     Convierte el fotograma de BGR a HSV, que es útil para la detección de colores.

   - **Definición de rangos de color:**
     ```
     vb = (30, 85, 90)
     va = (110, 255, 255)
     ```
     Define los límites inferior y superior para un rango de colores en el espacio HSV. Estos valores corresponden a un rango específico de colores que se desea detectar (por ejemplo, tonos de verde/azul).

   - **Creación de la máscara:**
     ```
     mask = cv.inRange(img2, vb, va)
     ```
     Crea una máscara binaria donde los píxeles dentro del rango de color definido son blancos (255) y los demás son negros (0).

   - **Aplicación de la máscara:**
     ```
     res = cv.bitwise_and(img, img, mask=mask)
     ```
     Aplica la máscara a la imagen original usando una operación bit a bit, lo que resulta en una imagen donde solo los píxeles dentro del rango de color se conservan.

   - **Mostrar las imágenes:**
     ```
     cv.imshow('captura', res)
     cv.imshow('real', img)
     ```
     Muestra la imagen filtrada (`res`) en una ventana llamada 'captura' y la imagen original (`img`) en una ventana llamada 'real'.

   - **Esperar a la tecla 's' para salir:**
     ```
     if cv.waitKey(1) & 0xFF == ord('s'):
         break
     ```
     Espera 1 ms a que se presione una tecla. Si se presiona la tecla 's', el bucle se rompe y se cierra el programa.

3. **Liberación de recursos y cierre de ventanas:**
   ```
   cap.release()
   cv.destroyAllWindows()
   ```
   - `cap.release()` libera la captura de vídeo.
   - `cv.destroyAllWindows()` cierra todas las ventanas de OpenCV.

# Ejercicio Willy

### Función `rota`

1. **Definición de la función `rota`:**
   ```
   def rota(img, i):
   ```
   Define una función llamada `rota` que toma dos argumentos: una imagen (`img`) y un índice (`i`).

2. **Obtención de las dimensiones de la imagen:**
   ```
   h, w = img.shape[:2]
   ```
   Extrae la altura (`h`) y el ancho (`w`) de la imagen.

3. **Creación de la matriz de rotación:**
   ```
   mw = cv.getRotationMatrix2D((h//2, w//2), 280, -1)
   ```
   Calcula la matriz de rotación (`mw`) para rotar la imagen alrededor de su centro. La rotación es de 280 grados y el factor de escala es -1 (invierte la imagen).

4. **Aplicación de la transformación afín:**
   ```
   img2 = cv.warpAffine(img, mw, (h, w))
   ```
   Aplica la transformación afín (rotación) a la imagen utilizando la matriz de rotación calculada.

5. **Guardado de la imagen rotada:**
   ```
   cv.imwrite('C://Users//david//Desktop//IA//Wally//Originnal' + str(i) + '.png', img2)
   ```
   Guarda la imagen rotada en la ubicación especificada con un nombre modificado que incluye el índice `i`.

### Procesamiento de imágenes en el directorio

6. **Inicialización del índice:**
   ```
   i = 0
   ```
   Inicializa el índice `i` en 0.

7. **Obtención de la lista de archivos en el directorio:**
   ```
   imgPaths = "C://Users//david//Desktop//IA//Wally//Originnal"
   nomFiles = os.listdir(imgPaths)
   ```
   Define la ruta del directorio donde se encuentran las imágenes y obtiene una lista de todos los archivos en ese directorio.

8. **Iteración sobre los archivos y aplicación de la rotación:**
   ```
   for nomFiles in nomFiles:
       i = i + 1
       imgPath = imgPaths + "/" + nomFiles
       img = cv.imread(imgPath)
       rota(img, i)
   ```

   - **Iteración sobre cada archivo en el directorio:**
     - `for nomFiles in nomFiles:` itera sobre cada archivo en la lista de archivos `nomFiles`.
   - **Incremento del índice:**
     - `i = i + 1` incrementa el índice en 1.
   - **Construcción de la ruta completa del archivo:**
     - `imgPath = imgPaths + "/" + nomFiles` construye la ruta completa de la imagen.
   - **Lectura de la imagen:**
     - `img = cv.imread(imgPath)` lee la imagen desde la ruta construida.
   - **Llamada a la función `rota`:**
     - `rota(img, i)` llama a la función `rota` para rotar y guardar la imagen.

### Resumen

El código realiza las siguientes acciones:

1. Define una función `rota` que rota una imagen en 280 grados y la guarda con un nombre modificado.
2. Itera sobre todos los archivos en un directorio específico.
3. Para cada archivo, incrementa un índice, lee la imagen, llama a la función `rota` para rotar la imagen y guarda la imagen rotada.

Este proceso continúa hasta que todas las imágenes en el directorio han sido procesadas y rotadas.

# RF_26

### Desglose del Código:

1. **Cargar el clasificador Haar para la detección de rostros:**
   ```
   rostro = cv.CascadeClassifier('C:\\Users\\david\\Desktop\\IA\\haarcascade_frontalface_alt.xml')
   ```
   Esta línea carga el clasificador Haar desde el archivo XML especificado. Este clasificador se utiliza para detectar rostros en las imágenes.

2. **Inicializar la captura de vídeo desde la cámara web:**
   ```
   cap = cv.VideoCapture(0)
   ```
   Esta línea inicializa la captura de vídeo desde la cámara web (el índice `0` se refiere a la cámara predeterminada).

3. **Bucle para procesar cada fotograma capturado:**
   ```
   while True:
       ret, frame = cap.read()
       gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
       rostros = rostro.detectMultiScale(gray, 1.3, 5)
       for(x, y, w, h) in rostros:
           frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
       cv.imshow('rostros', frame)
       k = cv.waitKey(1)
       if k == 27:
           break
   cap.release()
   cv.destroyAllWindows()
   ```

   Vamos a desglosar cada parte del bucle:

   - **Captura de fotograma:**
     ```
     ret, frame = cap.read()
     ```
     Captura un fotograma de la cámara web. `ret` es un valor booleano que indica si la captura fue exitosa, y `frame` es el fotograma capturado.

   - **Conversión a escala de grises:**
     ```
     gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
     ```
     Convierte el fotograma de BGR a escala de grises. La detección de rostros se realiza de manera más eficiente en imágenes en escala de grises.

   - **Detección de rostros:**
     ```
     rostros = rostro.detectMultiScale(gray, 1.3, 5)
     ```
     Utiliza el clasificador Haar para detectar rostros en la imagen en escala de grises. La función `detectMultiScale` devuelve una lista de rectángulos (x, y, w, h) que indican la ubicación y el tamaño de cada rostro detectado. Los parámetros `1.3` y `5` son el factor de escala y el número mínimo de vecinos, respectivamente, que afectan la precisión y la sensibilidad de la detección.

   - **Dibujar rectángulos alrededor de los rostros detectados:**
     ```
     for (x, y, w, h) in rostros:
         frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
     ```
     Para cada rostro detectado, dibuja un rectángulo verde alrededor del rostro en el fotograma original. Las coordenadas (x, y) representan la esquina superior izquierda del rectángulo, y (x+w, y+h) representan la esquina inferior derecha. El color del rectángulo es verde (0, 255, 0) y el grosor es 2 píxeles.

   - **Mostrar el fotograma con los rostros detectados:**
     ```
     cv.imshow('rostros', frame)
     ```
     Muestra el fotograma con los rectángulos dibujados alrededor de los rostros en una ventana llamada 'rostros'.

   - **Esperar a que se presione la tecla `Esc` para salir:**
     ```
     k = cv.waitKey(1)
     if k == 27:
         break
     ```
     Espera 1 milisegundo para una tecla. Si se presiona la tecla `Esc` (código 27), se rompe el bucle y el programa finaliza.

4. **Liberar recursos y cerrar ventanas:**
   ```
   cap.release()
   cv.destroyAllWindows()
   ```
   - `cap.release()` libera la captura de vídeo.
   - `cv.destroyAllWindows()` cierra todas las ventanas de OpenCV.

   # Untiled

### Primer Bloque de Código:

1. **Inicialización de la captura de vídeo:**
   ```
   cap = cv.VideoCapture(0)
   ```

   Inicializa la captura de vídeo desde la cámara web (índice `0` se refiere a la cámara predeterminada).

2. **Bucle para procesamiento de vídeo en tiempo real:**
   ```
   while True:
       ret, frame = cap.read()
       gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
       rostros = rostro.detectMultiScale(gray, 1.3, 5)
       for (x, y, w, h) in rostros:
           frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
           # frame2 = frame[y:y+h, x:x+w]
           # cv.imshow('rostros', frame2)
           # frame2 = cv.resize(frame2, (100, 100), interpolation=cv.INTER_LINEAR)
           # cv.imwrite('C:\\ruta\\deseada\\nombre_archivo.png', frame2)
       cv.imshow('rostros', frame)
       k = cv.waitKey(1)
       if k == 27:
           break
   cap.release()
   cv.destroyAllWindows()
   ```

   - **Captura de fotograma:**
     ```
     ret, frame = cap.read()
     ```

     Captura un fotograma de la cámara web. `ret` es un valor booleano que indica si la captura fue exitosa, y `frame` es el fotograma capturado.

   - **Conversión a escala de grises:**
     ```
     gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
     ```

     Convierte el fotograma de BGR a escala de grises.

   - **Detección de rostros:**
     ```
     rostros = rostro.detectMultiScale(gray, 1.3, 5)
     ```

     Utiliza el clasificador Haar para detectar rostros en la imagen en escala de grises. La función `detectMultiScale` devuelve una lista de rectángulos (x, y, w, h) que indican la ubicación y el tamaño de cada rostro detectado.

   - **Dibujar rectángulos alrededor de los rostros detectados:**
     ```
     for (x, y, w, h) in rostros:
         frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
     ```

     Para cada rostro detectado, dibuja un rectángulo verde alrededor del rostro en el fotograma original.

   - **Mostrar la imagen con los rectángulos:**
     ```
     cv.imshow('rostros', frame)
     ```

     Muestra el fotograma con los rectángulos dibujados alrededor de los rostros en una ventana llamada 'rostros'.

   - **Esperar a la tecla `Esc` para salir:**
     ```
     k = cv.waitKey(1)
     if k == 27:
         break
     ```

     Espera 1 ms para detectar una tecla. Si se presiona la tecla `Esc` (código 27), se rompe el bucle y el programa finaliza.

   - **Liberar recursos y cerrar ventanas:**
     ```
     cap.release()
     cv.destroyAllWindows()
     ```

     - `cap.release()` libera la captura de vídeo.
     - `cv.destroyAllWindows()` cierra todas las ventanas de OpenCV.

### Segundo Bloque de Código:

Este código también captura vídeo en tiempo real, detecta rostros y dibuja rectángulos alrededor de los rostros detectados. Adicionalmente, extrae y guarda las regiones de los rostros detectados como imágenes separadas en un directorio específico.

1. **Inicialización de la captura de vídeo y el contador de imágenes:**
   ```
   cap = cv.VideoCapture(0)
   i = 0
   ```

   Inicializa la captura de vídeo desde la cámara web y un contador (`i`) para nombrar las imágenes guardadas.

2. **Bucle para procesamiento de vídeo en tiempo real:**
   ```
   while True:
       ret, frame = cap.read()
       gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
       rostros = rostro.detectMultiScale(gray, 1.3, 5)
       for (x, y, w, h) in rostros:
           frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
           frame2 = frame[y:y+h, x:x+w]
           cv.imshow('rostro2', frame2)
           cv.imwrite('C:\\Users\\david\\Desktop\\IA\\Rostros\\Pancho' + str(i) + '.png', frame2)
       cv.imshow('rostros', frame)
       i = i + 1
       k = cv.waitKey(1)
       if k == 27:
           break
   cap.release()
   cv.destroyAllWindows()
   ```

   - **Captura de fotograma:**
     ```
     ret, frame = cap.read()
     ```

     Captura un fotograma de la cámara web.

   - **Conversión a escala de grises:**
     ```
     gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
     ```

     Convierte el fotograma a escala de grises.

   - **Detección de rostros:**
     ```
     rostros = rostro.detectMultiScale(gray, 1.3, 5)
     ```

     Detecta rostros en la imagen en escala de grises.

   - **Dibujar rectángulos y extraer regiones de rostros:**
     ```
     for (x, y, w, h) in rostros:
         frame = cv.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
         frame2 = frame[y:y+h, x:x+w]
         cv.imshow('rostro2', frame2)
         cv.imwrite('C:\\Users\\david\\Desktop\\IA\\Rostros\\Pancho' + str(i) + '.png', frame2)
     ```

     - Para cada rostro detectado, dibuja un rectángulo verde alrededor del rostro en el fotograma original.
     - Extrae la región del rostro detectado (`frame2`).
     - Muestra la región del rostro en una ventana llamada 'rostro2'.
     - Guarda la región del rostro como una imagen en la ruta especificada con un nombre que incluye el índice `i`.

   - **Mostrar la imagen con los rectángulos:**
     ```
     cv.imshow('rostros', frame)
     ```

     Muestra el fotograma con los rectángulos dibujados alrededor de los rostros en una ventana llamada 'rostros'.

   - **Incrementar el contador y esperar a la tecla `Esc` para salir:**
     ```
     i = i + 1
     k = cv.waitKey(1)
     if k == 27:
         break
     ```

     - Incrementa el contador `i` en 1.
     - Espera 1 ms para detectar una tecla. Si se presiona la tecla `Esc`, se rompe el bucle y el programa finaliza.

   - **Liberar recursos y cerrar ventanas:**
     ```
     cap.release()
     cv.destroyAllWindows()
     ```

     - `cap.release()` libera la captura de vídeo.
     - `cv.destroyAllWindows()` cierra todas las ventanas de OpenCV.

# wallySearch


### Primer Bloque de Código:

Este bloque de código carga una imagen, crea una nueva imagen más grande, y realiza una transformación geométrica para rotar la imagen.

1. **Importación de bibliotecas y carga de imagen:**
   ```
   import cv2 as cv
   import math
   import numpy as np 

   img = cv.imread('C://Users//david//Desktop//IA//Wally//Originnal//Captura1.png', 1)
   ```

   Importa las bibliotecas necesarias y carga la imagen desde la ruta especificada.

2. **Obtener dimensiones de la imagen y crear una imagen vacía:**
   ```
   h, w = img.shape[:2]
   img2 = np.zeros((h*3, w*3), dtype="uint8")
   ```

   Obtiene las dimensiones de la imagen original (alto y ancho) y crea una imagen vacía tres veces más grande.

3. **Aplicar transformación geométrica:**
   ```
   for i in range(h):
       for j in range(w):
           img2[int(i*math.cos(math.radians(30))-j*math.sin(math.radians(30)))+200,
                int(i*math.sin(math.radians(30))+j*math.cos(math.radians(30)))+50] = img[i, j]
   ```

   Recorre cada píxel de la imagen original y aplica una transformación geométrica para rotar la imagen. La transformación utiliza ángulos en radianes y las funciones `cos` y `sin`.

4. **Mostrar las imágenes:**
   ```
   cv.imshow('imagen1', img)
   cv.imshow('imagen2', img2)
   cv.waitKey(0)
   cv.destroyAllWindows()
   ```

   Muestra la imagen original y la imagen transformada, esperando a que se presione una tecla para cerrar las ventanas.

### Segundo Bloque de Código:

Este bloque define una función para rotar una imagen en múltiples ángulos y guardarla.

1. **Función para rotar la imagen:**
   ```
   def rota(img, i):
       for j in range(10, 361, 20):
           h, w = img.shape[:2]
           mw = cv.getRotationMatrix2D((h // 2, w // 2), j, -1)
           img2 = cv.warpAffine(img, mw, (h, w))
           cv.imwrite('C:\\Users\\david\\Desktop\\IA\\Wally\\Originnal' + str(i) + '_' + str(j) + '.png', img2)
   ```

   Esta función rota la imagen en ángulos de 10 a 360 grados (incrementos de 20 grados), y guarda cada imagen rotada con un nombre diferente.

2. **Rotar y guardar múltiples imágenes:**
   ```
   i = 0
   imgPaths = 'C:\\Users\\david\\Desktop\\IA\\Wally\\Originnal'
   nomFiles = os.listdir(imgPaths)
   for nomFile in nomFiles:
       i = i + 1
       imgPath = imgPaths + "\\" + nomFile
       img = cv.imread(imgPath)
       rota(img, i)
   ```

   Recorre todos los archivos de la ruta especificada, carga cada imagen y llama a la función `rota` para rotar y guardar cada imagen.

### Tercer Bloque de Código:

Este bloque de código carga una imagen, duplica su tamaño y realiza una interpolación simple para llenar la nueva imagen.

1. **Importación de bibliotecas y carga de imagen:**
   ```
   import cv2 as cv
   import numpy as np

   img = cv.imread('C://Users//david//Desktop//IA//Wally//Originnal//Captura1.PNG', 1)
   ```

   Importa las bibliotecas necesarias y carga la imagen desde la ruta especificada.

2. **Obtener dimensiones y crear una imagen más grande:**
   ```
   h, w = img.shape[:2]
   print(h, w)
   img2 = np.zeros((h*2, w*2), dtype="uint8")
   ```

   Obtiene las dimensiones de la imagen original, imprime las dimensiones y crea una nueva imagen vacía dos veces más grande.

3. **Duplicar el tamaño de la imagen:**
   ```
   print("Valores " + str(img.shape[:2]))
   for i in range(h):
       for j in range(w):
           img2[int(i*2), int(j*2)] = img[i, j]
   ```

   Recorre cada píxel de la imagen original y lo coloca en la nueva imagen a doble de su posición original.

4. **Mostrar las imágenes:**
   ```
   cv.imshow('imagen', img)
   cv.imshow('imagen2', img2)
   cv.waitKey(0)
   cv.destroyAllWindows()
   ```

   Muestra la imagen original y la imagen ampliada, esperando a que se presione una tecla para cerrar las ventanas.

### Cuarto Bloque de Código:

Este bloque define dos funciones, una para rotar y otra para convertir imágenes a escala de grises, y aplica estas funciones a una serie de imágenes.

1. **Función para rotar la imagen:**
   ```
   def rotar(img, i):
       h, w = img.shape[:2]
       mw = cv.getRotationMatrix2D((h // 2, w // 2), 45, -1)
       img2 = cv.warpAffine(img, mw, (h, w))
       cv.imwrite(''+str(i)+'.png', img2)
   ```

   Rota la imagen 45 grados y la guarda con el índice `i` en el nombre del archivo.

2. **Función para convertir a escala de grises:**
   ```
   def escalagris(img, i):
       gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
       cv.imwrite(''+str(i)+'.png', gray)
   ```

   Convierte la imagen a escala de grises y la guarda con el índice `i` en el nombre del archivo.

3. **Aplicar las funciones a múltiples imágenes:**
   ```
   i = 0
   imgPaths = ""
   nomFiles = os.listdir(imgPaths)
   for nomFile in nomFiles:
       i = i + 1
       imgPath = imgPaths + "/" + nomFile
       img = cv.imread(imgPath)
       #rotar(img, i)
       escalagris(img, i)
   ```

   Recorre todos los archivos de la ruta especificada, carga cada imagen y llama a la función `escalagris` para convertirla a escala de grises y guardarla. La línea `rotar(img, i)` está comentada, por lo que no se aplica la rotación en este caso.

### Resumen

- **Primer bloque:** Realiza una transformación geométrica manual para rotar una imagen y mostrar el resultado.
- **Segundo bloque:** Rota una imagen en múltiples ángulos y guarda cada imagen rotada.
- **Tercer bloque:** Duplica el tamaño de una imagen mediante una interpolación simple y muestra el resultado.
- **Cuarto bloque:** Define funciones para rotar imágenes y convertirlas a escala de grises, aplicando estas funciones a múltiples imágenes en una carpeta.

### Unos no os subi al repo por que me marca warnings, por eso subi la cap de los que use.