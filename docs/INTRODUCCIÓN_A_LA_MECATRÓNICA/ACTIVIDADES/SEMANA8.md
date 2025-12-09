# Actividad 8: Captura y Visualización de Video en Tiempo Real con OpenCV

## 1. Captura básica y visualización de video:

El código utiliza la librería **OpenCV** (`cv2`) para hacer tres tareas principales: encender la cámara, mostrar lo que ve en la pantalla y apagarse cuando se le indique.

```cpp
import cv2
 
 
video = cv2.VideoCapture(0)
 
while True:
    ret, frame = video.read()
    if not ret:
        break
    cv2.imshow("Video", frame)
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

1. `video = cv2.VideoCapture(0)`: Se inicializa el objeto `video` que establece la comunicación con el hardware de la cámara por defecto (índice `0`). 
2. `ret, frame = video.read()`: Lee un *frame* nuevo, el cual es una matriz **NumPy** que contiene los datos de pixeles y `ret`verifica que la lectura fue exitosa.
3. `cv2.imshow(...)`: Muestra la matriz `frame` en una ventana gráfica.
4. `cv2.waitKey(1)`: Pausa la ejecución por 1 milisegundo, permitiendo que la ventana gráfica se actualice y que el programa detecte la pulsación de la tecla 'q' para salir del bucle.
5. `video.release()`: Una vez fuera del bucle, se libera el recurso de la cámara, permitiendo que otros programas puedan acceder a ella.

<div align="center">
<img src="../../assets/imgs//S8I1.jpg" alt="/Servo" width="500">
</div>


---

## 2. Dibujo de línea cruzada sobre el video

Este código obtiene el video en tiempo real y se dibuja una línea diagonal sobre una copia del frame, mostrando el video original y el modificado en ventanas separadas.

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

1. `dibujo = frame.copy()`: Se captura el *frame* original (`frame`) y se crea una copia exacta (`dibujo`).
2. `cv2.line(...)`: Dibuja una línea sobre la imagen indicada (`dibujo`).
   - `(0,0)` y `(640, 480)`: Coordenadas de inicio y fin de la línea. Va desde la esquina superior izquierda `(0, 0)` hasta la coordenada `(640, 480)`, que suele ser la esquina inferior derecha de una resolución estándar de video (como 640x480).
   - `(0, 0, 225)`: Define el color de la línea usando el formato BGR (Azul, Verde, Rojo) de OpenCV. El código `(0, 0, 225)` le da un color rojo, donde `255` es la intensidad máxima del color.
   - `thickness=3`: Indica que la línea tendrá un grosor de 3 píxeles.
3. `cv2.imshow("Video", frame)`: Muestra el video original sin modificar.
4. `cv2.imshow("VIDEO2", dibujo)`: Muestra la copia que tiene la línea dibujada.

<div align="center">
<img src="../../assets/imgs//S812.png" alt="/Servo" width="500">
</div>

---

## 3. Dibujo de línea y rectángulo:
El código muestra una línea diagonal y un rectángulo que enmarca toda la imagen, lo cual genera una vista con figuras geométricas superpuestas al video.

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

- `cv2.rectangle(dibujo, ...)`: Dibuja un rectángulo sobre la copia `dibujo`.
 - **Puntos:** Usa los mismos puntos, `(0, 0)` y `(640, 480)`, pero en lugar de trazar una línea, crea una caja que rodea toda la imagen (un marco).
 - **Color:** `(255, 0, 0)` le dal al marco un tono azul.
   
<div align="center">
<img src="../../assets/imgs//S8I3.png" alt="/Servo" width="500">
</div>

---

## 4. Dibujo de línea, rectángulo y círculo:
Se combinan tres figuras: una línea diagonal, un rectángulo y un círculo centrado, todos dibujados sobre el video en tiempo real.

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

- `cv2.circle(dibujo, ...)`: Agrega la tercera figura geométrica: círculo.
  - **Centro** `(320, 240)`: Esta coordenada se calcula como el centro exacto de una pantalla estándar de 640 x 480.
  - **Radio** (`100`): Define el tamaño del círculo (100 píxeles de radio).
  - **Color y Grosor:** Dibuja un círculo de color azul `(255, 0, 0)` con un grosor de 10 píxeles.

<div align="center">
<img src="../../assets/imgs//S8I4.png" alt="/Servo" width="500">
</div>

---
