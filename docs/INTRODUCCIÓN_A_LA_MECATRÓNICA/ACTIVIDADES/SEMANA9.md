# Actividad 9: Continuación Captura, Visualización de Video en Tiempo Real con OpenCV y Creación de Máscaras

## Texto y animación de círculo:

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

<div align="center">
<img src="../../assets/imgs//S9I1.png" alt="/Servo" width="310">
</div>

---

## Blanco y negro:

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

<div align="center">
<img src="../../assets/imgs//S9I2.png" alt="/Servo" width="310">
</div>

---

## Colores azules:

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

<div align="center">
<img src="../../assets/imgs//S9I3.png" alt="/Servo" width="310">
</div>

---

## Colores amarillos:

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

<div align="center">
<img src="../../assets/imgs//S9I4.png" alt="/Servo" width="310">
</div>

---


## TColores rosas:

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

<div align="center">
<img src="../../assets/imgs//S9I5.png" alt="/Servo" width="310">
</div>

---

## Colores rojos:

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

<div align="center">
<img src="../../assets/imgs//S9I6.png" alt="/Servo" width="310">
</div>

---

## División de cámara y pedazos por colores:

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
<img src="../../assets/imgs//S9I7.png" alt="/Servo" width="310">
</div>

---

## Creación de máscara:

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


## Creación de máscara coon difrenete color:

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

## Detectar caras:

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

## Detectar color verde:

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
<img src="../../assets/imgs//S9I1.png" alt="/Servo" width="310">
  <img src="../../assets/imgs//S9I1-2.png" alt="/Servo" width="310">
</div>

---
