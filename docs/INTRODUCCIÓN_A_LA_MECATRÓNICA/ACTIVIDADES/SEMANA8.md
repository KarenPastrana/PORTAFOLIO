# Actividad 8: Captura y Visualización de Video en Tiempo Real con OpenCV

## Captura y visualización de video:

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

## Realización de linea cruzada:

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

## Realización de rectángulo y linea:

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

## Realización de rectángulo, linea y círculo:

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
