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
Tomaa el video de la cámara y aplica un filtro de color para convertirlo en blanco y negro (escala de grises) antes de mostrarlo.

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
- `dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2GRAY)`: Quita el color a la imagen.
  - `cv2.cvtColor` cambia el color.
  - `cv2.COLOR_BGR2GRAY` toma la imagen en el formato de color **BGR** y la convierte a gris.

La imagen `dibujo` ahora solo tiene información de brillo (blanco, negro y tonos de gris), perdiendo los tres canales de color originales.

<div align="center">
<img src="../../assets/imgs//S9I2.png" alt="/Servo" width="500">
</div>

---

## 3. Alteración de colores hacia tonalidades azules:
El espacio de color del *frame* se transforma de **BGR** a **RGB**, produciendo un efecto visual donde predominan **tonos azules**.

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
- `dibujo = cv2.cvtColor(dibujo, cv2.COLOR_BGR2RGB)`: Cambia el orden de **BGR** a **RGB**.

Al cambiar el orden de los canales, el programa interpreta el canal **Azul** como **Rojo**, y el canal **Rojo** como **Azul**. Esto hace que los colores se vean invertidos en estos dos tonos, dando un resultado donde las imágenes suelen tener una fuerte dominante azul.

<div align="center">
<img src="../../assets/imgs//S9I3.png" alt="/Servo" width="500">
</div>

---

## 4. Filtro amarillo desactivando el canal azul:
Se eliminan los valores del canal azul, generando una imagen con predominancia de tonos amarillos.

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
  - El `0` al final significa seleccionar el canal en el índice `0`. En el formato **BGR**, el canal 0 es el Azul.
  - `=0`: Asigna el valor cero a todos los píxeles de ese canal azul.

Al **eliminar** el **Azul**, solo quedan el **Rojo** y el **Verde**. Cuando el Rojo y el Verde se mezclan con luz, el resultado visible es el color **Amarillo**.

<div align="center">
<img src="../../assets/imgs//S9I4.png" alt="/Servo" width="500">
</div>

---


## 5. Tonos rosas desactivando el canal verde:
Se anula el canal verde de la imagen, haciendo que los colores Rojo y Azul se mezclen y modifiquen hacia gamas rosadas .

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
- `dibujo[:,:,1]=0`: Aplica el filtro.
  - `[:,:,1]`: Selecciona todos los píxeles (todos los `[:,:]`) en el canal índice 1 (`1`). El canal 1 es el Verde.
  - `=0`: Elimina todo el color verde de la imagen.

Sin el color Verde, solo quedan el Azul y el Rojo para formar todos los colores. Cuando el azul y el rojo se combinan, el color resultante que vemos es el Magenta (una forma de rosado intenso).

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
- `dibujo[:,:,1]=0:` Elimina el canal Verde.
  - El `1` es el índice del canal Verde en el formato **BGR**.
- `dibujo[:,:,0]=0`: Elimina el canal Azul.
  - El `0` es el índice del canal Azul en el formato **BGR**.

Al dejar los canales Azul y Verde, el único color que queda en la imagen es el canal Rojo (índice 2). Esto crea una imagen que solo tiene tonos de rojo, sin mezclas de color

<div align="center">
<img src="../../assets/imgs//S9I6.png" alt="/Servo" width="310">
</div>

---

## 7. División de la cámara por secciones de color:

Descripción: Se manipulan regiones específicas del frame asignando cambios en los canales de color, creando cuadrantes con efectos distintos.

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

<div align="center">
<img src="../../assets/imgs//S9I7.jpeg" alt="/Servo" width="310">
</div>

---

## 8. Creación de máscara para detección de color azul:

Descripción: La imagen se convierte a HSV y se genera una máscara que detecta el color azul dentro del rango establecido, mostrando imagen original, máscara y resultado.

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

<div align="center">
<img src="../../assets/imgs//S9I8.png" alt="/Servo" width="310">
</div>

---


## 9. Creación de máscara con rango ampliado de color:

Descripción: Se aplica una máscara con un rango más amplio en HSV, permitiendo detectar más tonalidades del color objetivo y mostrando la segmentación resultante.

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

<div align="center">
<img src="../../assets/imgs//S9I9.png" alt="/Servo" width="310">
</div>

---

## 10. Detección de caras con red neuronal (DNN):

Descripción: Se utiliza un modelo entrenado (Caffe SSD) para detectar rostros en tiempo real, dibujando un recuadro y mostrando la confianza de detección.

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

<div align="center">
<img src="../../assets/imgs//S9I10.png" alt="/Servo" width="310">
</div>

---

## 11. Detección de color verde:

Descripción: Se convierte la imagen a HSV y se crea una máscara que identifica tonos verdes dentro del rango especificado, mostrando la segmentación del color en tiempo real.

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

<div align="center">
<img src="../../assets/imgs//S9I11.png" alt="/Servo" width="310">
  <img src="../../assets/imgs//S9I11-2.png" alt="/Servo" width="310">
</div>

---
