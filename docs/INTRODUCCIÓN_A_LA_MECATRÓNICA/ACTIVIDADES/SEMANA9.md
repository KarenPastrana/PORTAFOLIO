# Actividad 9: Continuación Captura, Visualización de Video en Tiempo Real con OpenCV y Creación de Máscaras

## 1. Texto y animación de círculo:
Se dibuja una línea, un rectángulo y un círculo cuya posición cambia en cada frame para simular movimiento. Además, se añade un texto fijo.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    cv2.line(dibujo, (0,0), (640, 480), (0,0,225), thickness=3, lineType=cv2.LINE_AA)
    cv2.rectangle(dibujo, (0,0), (640, 480), (255,0,0), thickness=10, lineType=cv2.LINE_AA)
    cv2.circle(dibujo, (cx,cy), 100, (255,0,0), thickness=10, lineType=cv2.LINE_AA)
    cv2.putText(dibujo, "texto", (320,240), cv2.FONT_HERSHEY_SIMPLEX, 2, (255,0,0), thickness=1, lineType=cv2.LINE_AA)
 
    cx=cx+1
    cy=cy+1
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

1. **Variables de Posición** `(cx=0, cy=0)`: Estas variables se inicializan en cero y guardan las coordenadas donde se debe dibujar el círculo en el frame actual.
2. **Dibujo con Movimiento** `(cv2.circle(dibujo, (cx, cy), ...))`:Se dibuja el círculo, pero su centro está definido por las variables `cx` y `cy`.
3. **Texto Superpuesto** `cv2.putText(...)`: Esta función añade la palabra "texto" al centro de la imagen. A diferencia del círculo, las coordenadas `(320, 240)` son fijas, por lo que el texto no se mueve.
4. **Simulación de Animación** `cx=cx+1` **y** `cy=cy+1`: Estas líneas aumentan los valores de `cx` y `cy` en 1. En el siguiente ciclo, el círculo se dibujará en la posición `(1, 1)`, luego `(2, 2)`, y así sucesivamente. Como el ciclo es muy rápido, esto crea la ilusión de que el círculo se está moviendo en diagonal a través de la pantalla.

<div align="center">
<img src="../../assets/imgs//S9I1.png" alt="/Servo" width="500">
</div>

---

## 2. Conversión a blanco y negro:
Toma el video de la cámara y aplica un filtro de color para convertirlo en blanco y negro (escala de grises) antes de mostrarlo.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
   
    dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2GRAY)
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

**Filtro de Escala de Grises:**
* `dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2GRAY)`: Quita el color a la imagen.

  * `cv2.cvtColor` cambia el color.
  * `cv2.COLOR_BGR2GRAY` toma la imagen en el formato de color BGR y la convierte a gris.

La imagen `dibujo` ahora solo tiene información de brillo (blanco, negro y tonos de gris), perdiendo los tres canales de color originales.

<div align="center">
<img src="../../assets/imgs//S9I2.png" alt="/Servo" width="500">
</div>

---

## 3. Alteración de colores hacia tonalidades azules:
El espacio de color del *frame* se transforma de **BGR** a **RGB**, produciendo un efecto visual donde predominan tonos azules.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
   
    dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
```

**Filtro Azul:**

* `dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)`: Cambia el orden de BGR a RGB.
    * Al cambiar el orden de los canales, el programa interpreta el canal **azul** como **rojo**, y el canal **rojo** como **azul**. Esto hace que los colores se vean invertidos en estos dos tonos, haciendo que la imagen se vea más azul.

<div align="center">
<img src="../../assets/imgs//S9I3.png" alt="/Servo" width="500">
</div>

---

## 4. Filtro amarillo desactivando el canal azul:
Se eliminan los valores del canal azul, generando una imagen en donde predominan los tonos amarillos.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
   
    #dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)
    dibujo[:,:,0]=0 # ":" aplica para todos
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
```

**Filtro Amarillo:**

- `dibujo[:,:,0]=0`: Instrucción que aplica el filtro.
  - `[:,:,0]`: Los primeros `:` y los segundos `:` significan seleccionar todos los píxeles (todas las filas y todas las columnas).
  - El `0` al final significa seleccionar el canal en el índice `0`. En el formato BGR, el canal 0 es el azul.
  - `=0`: Asigna el valor cero a todos los píxeles de ese canal azul.

Al eliminar el azul, solo quedan el **rojo** y el **verde**. Cuando el rojo y el verde se mezclan con luz, el resultado es el color **amarillo**.

<div align="center">
<img src="../../assets/imgs//S9I4.png" alt="/Servo" width="500">
</div>

---


## 5. Tonos rosas desactivando el canal verde:
Se anula el canal verde de la imagen, haciendo que los colores rojo y azul se mezclen y modifiquen hacia tonos rosados .

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
   
    #dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)
    dibujo[:,:,1]=0 # ":" aplica para todos
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
```

**Filtro Rosado:**
* `dibujo[:,:,1]=0`: Aplica el filtro.

    * `[:,:,1]`: Selecciona todos los píxeles (todos los `[:,:]`) en el canal índice 1 (`1`). El canal 1 es el verde.
    * `=0`: Elimina todo el color verde de la imagen.

Sin el color verde, solo quedan el azul y el rojo para formar todos los colores, y cuando estos se combinan, el color resultante que vemos es el rosa.

<div align="center">
<img src="../../assets/imgs//S9I5.jpeg" alt="/Servo" width="500">
</div>

---

## 6. Tonos rojos desactivando los canales azul y verde:
Se desactivan los canales azul y verde, dejando únicamente el canal rojo para mostrar un efecto monocromático rojizo.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
   
    #dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)
    dibujo[:,:,1]=0 # ":" aplica para todos
    dibujo[:,:,0]=0
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
```

**Filtro Rojo:**
* `dibujo[:,:,1]=0`: Elimina el canal verde.
    * El `1` es el índice del canal verde en el formato BGR.
* `dibujo[:,:,0]=0`: Elimina el canal azul.
    * El `0` es el índice del canal azul en el formato BGR.

Al dejar los canales azul y verde, el único color que queda en la imagen es el canal rojo (índice 2). Esto crea una imagen que solo tiene tonos de rojo.

<div align="center">
<img src="../../assets/imgs//S9I6.png" alt="/Servo" width="500">
</div>

---

## 7. División de la cámara por secciones de color:
Toma el video de la cámara y aplica filtros de color distintos a diferentes áreas de la imagen. Esto genera un efecto visual donde el video se divide en regiones (cuadrantes), y cada región tiene una dominante de color particular.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
   
    #dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)
    dibujo[0:240,0:320,1]=0 # ":" aplica para todos
    dibujo[240:480,320:640,0]=0
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
 
```

1. **Primer Cuadrante (Filtro Rosado):** `dibujo[0:240, 0:320, 1] = 0` Modifica el cuadrante superior izquierdo.
    * `0:240` **(Alto):** Selecciona las filas de píxeles desde arriba (`0`) hasta la mitad (`240`).
    * `0:320` **(Ancho):** Selecciona las columnas de píxeles desde la izquierda (`0`) hasta la mitad (`320`).
    * `1` **(Canal):** Elimina el color verde y esta sección se vuelve rosa.

2. **Segundo Cuadrante (Filtro Amarillo):** `dibujo[240:480, 320:640, 0] = 0` Modifica el cuadrante inferior derecho.
    * `240:480` **(Alto):** Selecciona las filas desde la mitad (`240`) hasta abajo (`480`).
    * `320:640` **(Ancho):** Selecciona las columnas desde la mitad (`320`) hasta la derecha (`640`).
    * `0` **(Canal):** Elimina el color azul y esta sección se vuelve amarilla.

Los otros dos cuadrantes (superior derecho e inferior izquierdo) no son modificados y mantienen sus colores originales.
<div align="center">
<img src="../../assets/imgs//S9I7.jpeg" alt="/Servo" width="500">
</div>

---

## 8. Creación de máscara para detección de color azul:
La imagen se convierte a HSV y se genera una máscara que detecta el color azul dentro del rango establecido, mostrando imagen original, máscara y resultado.

```cpp
import cv2
import numpy as np #librería numérica
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    hsv = cv2.cvtColor(dibujo,cv2.COLOR_BGR2HSV)
    #2 Variables: rango alto y bajo
 
    bajo= np.array([100,80,40], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([140,255,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
 
```

1. **Conversión de Color (HSV):** `hsv = cv2.cvtColor(dibujo, cv2.COLOR_BGR2HSV)` Cambia la imagen del formato **BGR** a **HSV** (Tonalidad, Saturación, Valor).
2. **Definición de Rango (Azul):** `bajo = np.array([100, 80, 40], ...)` y `alto = np.array([140, 255, 255], ...)` Define los límites para lo que el programa considerará "azul".
3. **Creación de Máscara:** `mask = cv2.inRange(hsv, bajo, alto)` Revisa la imagen **HSV** y genera una **Máscara** (`mask`) que es una imagen en blanco y negro donde:
    * **Blanco (255):** Es todo lo que está dentro del rango azul definido.
    * **Negro (0):** Es todo lo que no es azul.
4. **Aplicación de Máscara:** `result = cv2.bitwise_and(frame, frame, mask=mask)` Combina la imagen original (`frame`) con la máscara (`mask`).
    * Lo que es azul se muestra blanco y lo que no es azul aparece en negro.
5. **Triple Visualización:** Se muestra el `ORIGINAL` (a color), la `MASK` (blanco y negro) y el `RESULT` (solo el Azul de la escena).

<div align="center">
<img src="../../assets/imgs//S9I8.png" alt="/Servo" width="500">
</div>

---


## 9. Creación de máscara con rango ampliado de color:
Se aplica una máscara con un rango más amplio en HSV, detectando más tonalidades del color y permitiiendo incluir más objetos en la imagen final.

```cpp
import cv2
import numpy as np #librería numérica
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    hsv = cv2.cvtColor(dibujo,cv2.COLOR_BGR2HSV)
    #2 Variables: rango alto y bajo
 
    bajo= np.array([100,80,40], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([255,255,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
 
```

**Rango Ampliado:**
* `bajo = np.array([100, 80, 40], ...)`: En el límite **inferior** los valores `[0, 0, 0]` son los más bajos que se pueden.
* `alto = np.array([255, 255, 255], ...)`: En el límite **superior** los valores `[255, 255, 255]` son los más altos que se pueden.

Esto hace que la máscara se vuelva mucho menos específica y capture una mayor variedad de colores en el video, no solo un tono específico de azul.

<div align="center">
<img src="../../assets/imgs//S9I9.png" alt="/Servo" width="500">
</div>

---

## 10. Detección de caras con red neuronal (DNN):
Se utiliza un modelo para detectar rostros en tiempo real, dibujando un recuadro y mostrando la confianza de detección.

```cpp
import cv2
import numpy as np
 
 
# Parametros de la red
mean = [104, 117, 123]
scale = 1.0
in_width = 300
in_height = 300
 
# Poner el umbral de detección para considerar una cara valida
detection_threshold = 0.5
 
# Tipo de texto
font_style = cv2.FONT_HERSHEY_SIMPLEX
font_scale = 0.5
font_thickness = 1
 
# Crear la red a partir de los archivos de configuración y pesos
net = cv2.dnn.readNetFromCaffe('models/deploy.prototxt',
                               'models/res10_300x300_ssd_iter_140000.caffemodel')
 
def detect(frame, net, scale, mean, in_width, in_height):
    h = frame.shape[0]
    w = frame.shape[1]
   # Convertir a blob
    blob = cv2.dnn.blobFromImage(frame, scalefactor=scale,
                                 size=(in_width, in_height), mean=mean, swapRB=False, crop=False)
    # Pasar el blob a la red
    net.setInput(blob)
    # Pasra el blob a la red
    detections = net.forward()
 
    # Procesar las detecciones
    for i in range(detections.shape[2]):
        confidence = detections[0, 0, i, 2]
        if confidence > detection_threshold:
           
            #Extraer las coordenadas del bounding box de la detección
            box = detections[0, 0, i, 3:7] * np.array([w, h, w, h])
            (x1, y1, x2, y2) = box.astype('int')
           
            # Dibujar el bounding box y el texto
            cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
            label = 'Confidence: %.4f' % confidence
            label_size, base_line = cv2.getTextSize(label, font_style, font_scale, font_thickness)
            cv2.rectangle(frame, (x1, y1 - label_size[1]), (x1 + label_size[0], y1 + base_line),
                          (255, 255, 255), cv2.FILLED)
            cv2.putText(frame, label, (x1, y1), font_style, font_scale, (0, 0, 0))
    return frame
 
# Abrir camara, el 0 es el id de la camara si solo tienes una, si tienes mas de una puedes cambiar el id
cap = cv2.VideoCapture(0)
 
while True:
    # leer el cuadro
    ret, frame = cap.read()
    if not ret:
        break
 
    show = detect(frame, net, scale, mean, in_width, in_height)
    cv2.imshow('frame', show)
   
    # salimos del bucle si se presiona la tecla 'q'
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
# Release the camera and close the window
cap.release()
cv2.destroyAllWindows()
 
```

1. **Carga del Modelo:** El sistema carga un modelo de Inteligencia Artificial (IA) que ya sabe cómo reconocer caras. Se establece un umbral de confianza (por ejemplo, 50%) para asegurar que solo se detecten rostros de manera fiable.
3. **Detección y Filtro:** La red predice las ubicaciones de los posibles rostros y asigna una confianza a cada uno. Solo las detecciones que superan el umbral se consideran válidas.
4. **Visualización:** Se dibuja un recuadro sobre cada rostro detectado y superpone el valor de confianza en la pantalla.
  
<div align="center">
<img src="../../assets/imgs//S9I10.png" alt="/Servo" width="500">
</div>

---

## 11. Detección de color verde:
Se aisla y resalta un color específico (**el verde**) dentro del video en tiempo real. Esto se logra configurando un **rango de tonalidades** en el espacio de color **HSV** y creando una **máscara** que únicamente revela los objetos de ese color.

```cpp
import cv2
import numpy as np #librería numérica
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    hsv = cv2.cvtColor(dibujo,cv2.COLOR_BGR2HSV)
    #2 Variables: rango alto y bajo
 
    bajo= np.array([45,0,0], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([75,255,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
 
```

**Definición del Rango Verde:**
`bajo = np.array([45, 0, 0], ...)` y `alto = np.array([75, 255, 255], ...)` definen los límites numéricos que representan el color verde en el espacio HSV.

* El **Tono (H)** para el Verde se encuentra típicamente entre `35` y `85` en los valores de OpenCV. Aquí, se usa un rango de `45` a `75` para ser preciso con los tonos de Verde.
* El límite `bajo` establece el tono más oscuro/menos saturado del verde que se quiere detectar, y el límite `alto` (hasta 255) asegura que se detecten todos los verdes brillantes y saturados dentro de ese rango de tono.

<div align="center">
<img src="../../assets/imgs//S9I11.png" alt="/Servo" width="310">
  <img src="../../assets/imgs//S9I11-2.png" alt="/Servo" width="310">
</div>

---
