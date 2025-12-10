# Actividad 10: Continuación Captura, Visualización de Video e Iniciación del Proyecto

## 1. Identificación de pelota verde mediante máscara HSV:
Se encuentra el objeto verde más grande dentro del video (simulando una pelota) y luego identifica sus coordenadas exactas en tiempo real.

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
 
    bajo= np.array([40,30,60], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([80,200,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
    #Selecciona solo el contorno mas grande
    lista_cont, herarquia = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    area_grande = 0
 
    for contn in lista_cont:
        area_n = cv2.contourArea(contn)
        if area_grande < area_n:
            area_grande = area_n
            contorno_pelota = contn
        else:
            continue
    (x,y),radio=cv2.minEnclosingCircle(contorno_pelota)
 
    print(x,y)
 
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

1. **Detección de Color (Máscara):**
    * `hsv = cv2.cvtColor(...)`: Convierte la imagen a HSV.
    * `bajo=np.array([40,...])` / `alto=np.array([80,...])`: Define el rango de tonalidades que se considera color verde.
    * `mask = cv2.inRange(...)`: Crea la máscara que aísla solo el color verde.
2. **Detección del Contorno más Grande:**
    * `lista_cont, herarquia = cv2.findContours(mask, ...)`: Busca todos los contornos en la imagen de la máscara.
3. **Área más grande:**
    * `area_n = cv2.contourArea(contn)`: Calcula el área de la forma actual.
    * `if area_grande < area_n:`: Si el área actual es más grande que la más grande que hemos encontrado hasta ahora, la guarda como la nueva `contorno_pelota`.
4. **Localización:**
    * `(x,y),radio=cv2.minEnclosingCircle(contorno_pelota)`: Toma `contorno_pelota` y calcula el círculo más pequeño que puede rodear esa forma. Esto nos da la posición central (`x, y`) y el radio.
    * `print(x, y)`: Imprime las coordenadas exactas de la pelota en la pantalla.

<div align="center">
<img src="../../assets/imgs//S10I1.jpeg" alt="/Servo" width="500">
</div>

---

## 2. Detección de contorno de pelota con marcación gráfica:
Se calcula el contorno más grande y se dibuja un círculo alrededor de la pelota, además de un punto en su centro, realizando un rastreo visual.

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
 
    bajo= np.array([40,30,60], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([80,200,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
    #Selecciona solo el contorno mas grande
    lista_cont, herarquia = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    area_grande = 0
 
    for contn in lista_cont:
        area_n = cv2.contourArea(contn)
        if area_grande < area_n:
            area_grande = area_n
            contorno_pelota = contn
        else:
            continue
    (x,y),radio=cv2.minEnclosingCircle(contorno_pelota)
 
    print(x,y)
 
    cv2.circle(frame,(int(x),int(y)),int(radio),(0,0,255), 3)
    cv2.circle(frame,(int(x),int(y)),3,(0,0,255), 3)
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

Una vez que se tienen las coordenadas y el radio de la pelota, se dibujan dos elementos:

* **Dibujo del Círculo Envolvente:** `cv2.circle(frame, (int(x), int(y)), int(radio), (0, 0, 255), 3)` dibuja un círculo grande.
    * `frame`: Se dibuja sobre la imagen original.
    * `(int(x), int(y))`: Usa el centro calculado de la pelota.
    * `int(radio)`: Usa el radio calculado, asegurando que el círculo encierre bien la pelota.
    * `(0, 0, 255)`: Define el color rojo para el borde.
* **Dibujo del Punto Central:** `cv2.circle(frame, (int(x), int(y)), 3, (0, 0, 255), 3)` dibuja un círculo muy pequeño (de radio `3`) en la misma coordenada central.
  
<div align="center">
<img src="../../assets/imgs//S10I2.jpeg" alt="/Servo" width="500">
</div>

---

## 3. Detección de pelota con cálculo de errores de posición:
Combina la detección de color y el rastreo de contornos para localizar un objeto verde (la pelota) y calcula en tiempo real qué tan lejos se encuentra ese objeto del centro exacto de la imagen (**error de posición**).

```cpp
import cv2
import numpy as np #librería numérica
import time
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    frame= cv2.flip(frame, 1)
    if not ret:
        break
    dibujo = frame.copy()
    hsv = cv2.cvtColor(dibujo,cv2.COLOR_BGR2HSV)
    #2 Variables: rango alto y bajo
 
    bajo= np.array([40,30,60], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([80,200,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
    #Selecciona solo el contorno mas grande
    lista_cont, herarquia = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    area_grande = 0
 
    for contn in lista_cont:
        area_n = cv2.contourArea(contn)
        if area_grande < area_n:
            area_grande = area_n
            contorno_pelota = contn
        else:
            continue
    (x,y),radio=cv2.minEnclosingCircle(contorno_pelota)
 
 
    cv2.circle(frame,(int(x),int(y)),int(radio),(190,130,255), 3)
    cv2.circle(frame,(int(x),int(y)),3,(190,130,255), 3)
 
 
    h = frame.shape[0]
    w = frame.shape[1]
 
    errorx=x-(w/2)
    errory=y-(h/2)
    print(errorx,errory)
 
    if errorx>0:
        print("IZQUIERDA")
    elif errorx<0:
        print("DERECHA")
    else:
        print("X OK")
 
 
    if errory>0:
        print("ARRUBA")
    elif errory<0:
        print("ABAJO")
    else:
        print("Y OK")
 
    time.sleep(0.5)
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

1. **Preparación del Video:** Se utiliza `cv2.flip` para invertir el video, logrando un efecto espejo. También se obtienen las dimensiones exactas (ancho `w` y alto `h`) del frame.
2. **Cálculo del Error:** El sistema calcula el desplazamiento de la pelota en X y Y (`errorx` y `errory`). Este error es la diferencia entre la posición actual de la pelota y el centro ideal de la pantalla. Un error igual a cero significa que la pelota está perfectamente centrada.
3. **Dirección:** Si el error es positivo o negativo, el código interpreta la dirección de la pelota (ej. si `errorx > 0`, la pelota está a la derecha).
4. **Control de Flujo:** Se añade una pausa (`time.sleep(0.5)`) para ralentizar el procesamiento y permitir que los mensajes de error se lean fácilmente.

   
<div align="center">
<img src="../../assets/imgs//S10I3.png" alt="/Servo" width="500">
</div>

---

## 4. Conexión Bluetooth con ESP32 para envío de datos:
Se establece conexión Bluetooth con un ESP32 y se envían comandos basados en la posición de la pelota detectada en la cámara.

```cpp
import cv2
import numpy as np #librería numérica
 
import bluetooth
import time
 
# FUNCTION TO CONNECT TO ESP32
port = 1
sock = bluetooth.BluetoothSocket()  # Use the default constructor (no arguments)
sock.settimeout(20)
 
print("Attempting to connect to ESP32...")
while True:
    try:
        sock.connect(("10:06:1C:97:72:DA", port))
        print("Connected to ESP32!")
        break
    except Exception as e:
        print("Error in connection... retrying:", e)
    time.sleep(1)
 
video = cv2.VideoCapture(0)
 
cx=0
cy=0
 
while True:
    ret, frame = video.read()
    frame= cv2.flip(frame, 1)
    if not ret:
        break
    dibujo = frame.copy()
    hsv = cv2.cvtColor(dibujo,cv2.COLOR_BGR2HSV)
    #2 Variables: rango alto y bajo
 
    bajo= np.array([40,30,60], dtype=np.uint8) #"u" solo positivos y van del 0 al 255
    alto= np.array([80,200,255], dtype=np.uint8)
 
    mask = cv2.inRange(hsv,bajo,alto)
    result= cv2.bitwise_and(frame,frame,mask=mask) #sobre la imagen original vamos a ahcer la op de AND con la mask y lo vamos ea guardar en result
 
    #Selecciona solo el contorno mas grande
    lista_cont, herarquia = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    area_grande = 0
 
    for contn in lista_cont:
        area_n = cv2.contourArea(contn)
        if area_grande < area_n:
            area_grande = area_n
            contorno_pelota = contn
        else:
            continue
    (x,y),radio=cv2.minEnclosingCircle(contorno_pelota)
 
 
    cv2.circle(frame,(int(x),int(y)),int(radio),(190,130,255), 3)
    cv2.circle(frame,(int(x),int(y)),3,(190,130,255), 3)
 
 
    h = frame.shape[0]
    w = frame.shape[1]
 
    errorx=x-(w/2)
    errory=y-(h/2)
    print(errorx,errory)
 
    if errorx>0:
        try:
            message = "Arriba"
            sock.send(message.encode())  # encode the string to bytes
            print("Sent:", message)
        except Exception as e:
            print("Error sending data:", e)
    elif errorx<0:
        print("DERECHA")
    else:
        print("X OK")
 
 
    if errory>0:
        print("ARRIBA")
    elif errory<0:
        print("ABAJO")
    else:
        print("Y OK")
 
    time.sleep(0.5)
 
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    cv2.imshow("MASK", mask)
    cv2.imshow("RESULT", result)
 
    #Salida del bucle
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
 
 
```

1. **Establecimiento de la Conexión:**
    - Se crea un punto de conexión virtual para enlazar los dispositivos por Bluetooth.
    - La conexión se establece usando la dirección MAC única del ESP32 y el puerto de comunicación definido.
    - Se define un tiempo límite para la conexión para manejar posibles fallos.

2. **Envío de Comandos de Control:**
    - Cuando el programa detecta un cambio en el error de posición, se genera un mensaje de texto con el comando de dirección (ej., "Arriba").
    - Este mensaje se codifica y se envía a través del socket (`sock.send(message.encode())`) al ESP32.
    - `try...except` asegura que si la conexión se interrumpe durante el envío, el programa no se detenga de forma inesperada.
