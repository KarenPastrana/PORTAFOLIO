# Actividad 8: Captura y Visualización de Video en Tiempo Real con OpenCV

## 1. Captura básica y visualización de video:

Descripción: Se captura video desde la cámara y se muestra en una ventana en tiempo real, además, se imprime la resolución de cada frame.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    cv2.line(dibujo, )
    print(" ")
    print(frame.shape[0])
    print(frame.shape[1])
 
    #mostrar img
    cv2.imshow("Video", frame)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

<div align="center">
<img src="../../assets/imgs//S8I1.jpg" alt="/Servo" width="310">
</div>

---

## 2. Dibujo de línea cruzada sobre el video

Descripción: Se obtiene el video en tiempo real y se dibuja una línea diagonal sobre una copia del frame, mostrando el video original y el modificado.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    cv2.line(dibujo, (0,0), (640, 480), (0,0,225), thickness=3, lineType=cv2.LINE_AA)
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

<div align="center">
<img src="../../assets/imgs//S812.png" alt="/Servo" width="310">
</div>

---

## 3. Dibujo de línea y rectángulo:

Descripción: Se agrega una línea diagonal y un rectángulo que enmarca toda la imagen, lo cual genera una vista con figuras geométricas superpuestas al video.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    cv2.line(dibujo, (0,0), (640, 480), (0,0,225), thickness=3, lineType=cv2.LINE_AA)
    cv2.rectangle(dibujo, (0,0), (640, 480), (255,0,0), thickness=3, lineType=cv2.LINE_AA)
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

<div align="center">
<img src="../../assets/imgs//S8I3.png" alt="/Servo" width="310">
</div>

---

## 4. Dibujo de línea, rectángulo y círculo:

Descripción: Se combinan tres figuras: una línea diagonal, un rectángulo y un círculo centrado, todos dibujados sobre el video en tiempo real.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    dibujo = frame.copy()
    cv2.line(dibujo, (0,0), (640, 480), (0,0,225), thickness=3, lineType=cv2.LINE_AA)
    cv2.rectangle(dibujo, (0,0), (640, 480), (255,0,0), thickness=10, lineType=cv2.LINE_AA)
    cv2.circle(dibujo, (320,240), 100, (255,0,0), thickness=10, lineType=cv2.LINE_AA)
 
    #mostrar img
    cv2.imshow("Video", frame)
    cv2.imshow("VIDEO2", dibujo)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

<div align="center">
<img src="../../assets/imgs//S8I4.png" alt="/Servo" width="310">
</div>

---
