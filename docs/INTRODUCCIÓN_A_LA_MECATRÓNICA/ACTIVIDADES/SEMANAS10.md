# Actividad 10: Continuación Captura, Visualización de Video e Iniciación del Proyecto

## 1. Identificación de pelota verde mediante máscara HSV:
Se encuentra el **objeto verde más grande** dentro del video (simulando una pelota) y luego **identifica sus coordenadas** exactas en tiempo real.

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
   - `hsv = cv2.cvtColor(...)`: Convierte la imagen a **HSV** para facilitar la detección.
   - `bajo=np.array([40,...])` / `alto=np.array([80,...])`: Define el rango de tonalidades que se considera **color Verde**.
   - `mask = cv2.inRange(...)`: Crea la **máscara** (imagen blanco y negro) que aísla solo el color Verde.
2. **Detección del Contorno más Grande (La Pelota):**
   - `lista_cont, herarquia = cv2.findContours(mask, ...)`: Busca todos los contornos (los bordes o líneas cerradas) en la imagen de la máscara. La `lista lista_cont` guarda todas las formas verdes encontradas.
3. **Bucle de Búsqueda:** El código utiliza un bucle (`for contn in lista_cont:`) para revisar cada forma encontrada:
   - `area_n = cv2.contourArea(contn)`: Calcula el **área** de la forma actual.
   - `if area_grande < area_n:`: Si el área actual es **más grande** que la más grande que hemos encontrado hasta ahora, la guarda como la nueva `contorno_pelota`.
4. **Localización:**
   - `(x,y),radio=cv2.minEnclosingCircle(contorno_pelota)`: Cuando termina el bucle, esta función toma el **contorno más grande** (`contorno_pelota`) y calcula el círculo más pequeño que puede rodear esa forma. Esto nos da la **posición central** (`x, y`) y el radio.
   - `print(x, y)`: Imprime las coordenadas exactas de la pelota en la pantalla.

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

**Marcación Gráfica:**
Una vez que se tienen las coordenadas y el radio de la pelota, se dibujan dos elementos:

* **Dibujo del Círculo Envolvente:** `cv2.circle(frame, (int(x), int(y)), int(radio), (0, 0, 255), 3)` dibuja un círculo grande.
    * `frame`: Se dibuja sobre la imagen original.
    * `(int(x), int(y))`: Usa el centro calculado de la pelota.
    * `int(radio)`: Usa el radio calculado, asegurando que el círculo encierre perfectamente la pelota.
    * `(0, 0, 255)`: Define el color Rojo para el borde.
* **Dibujo del Punto Central:** `cv2.circle(frame, (int(x), int(y)), 3, (0, 0, 255), 3)` dibuja un círculo muy pequeño (de radio `3`) en la misma coordenada central. Esto actúa como un **marcador de punto** en el centro exacto del objeto rastreado.
  
<div align="center">
<img src="../../assets/imgs//S10I2.jpeg" alt="/Servo" width="500">
</div>

---

## 3. Detección de pelota con cálculo de errores de posición:
Combina la **detección de color** y el **rastreo de contornos** para localizar un objeto verde (la pelota) y calcula en tiempo real qué tan lejos se encuentra ese objeto del **centro exacto de la imagen**. Este valor de distancia se conoce como **"error de posición"**.

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

1. **Preparación del Frame:**
   - `frame = cv2.flip(frame, 1)`: Rota la imagen en el eje Y (`1`). Esto hace que el video se vea como si te vieras en un espejo, lo cual es útil para que el movimiento de la pelota sea intuitivo para el usuario (si mueves la mano a la derecha, la imagen se mueve a la derecha).
   - `h = frame.shape[0]`: Obtiene el tamaño de la dimensión **vertical** (altura) del *frame* de video.
   - `w = frame.shape[1]`: Obtiene el tamaño de la dimensión **horizontal** (ancho) del *frame* de video.
 2. **Cálculo del Error de Posición:** Determina el **desplazamiento** del centro de la pelota (`x, y`) respecto al centro de la pantalla (`w/2, h/2`).
    - `errorx = x - (w/2)`: Calcula la diferencia entre la **posición X** de la pelota (`x`) y el **punto central horizontal** de la pantalla (`w/2`). Un resultado de `0` significa que la pelota está centrada horizontalmente.
    - `errory = y - (h/2)`: Calcula la diferencia entre la **posición Y** de la pelota (`y`) y el **punto central vertical** de la pantalla (`h/2`). Un resultado de `0` significa que la pelota está centrada verticalmente.
3. **Lógica de Dirección:** Interpretar los valores de error e indica en qué dirección se encuentra la pelota.
   - `if errorx > 0`: Si el valor del error en X es **positivo** (la pelota está a la derecha del centro), imprime "IZQUIERDA".
   - `elif errorx < 0`: Si el valor del error en X es **negativo** (la pelota está a la izquierda del centro), imprime "DERECHA".
   - `if errory > 0`: Si el error en Y es **positivo** (la pelota está debajo del centro), imprime "ARRIBA".
   - `elif errory < 0`: Si el error en Y es **negativo** (la pelota está arriba del centro), imprime "ABAJO".
4. **Control de Tiempo:**
   - `time.sleep(0.5)`: Detiene la ejecución del código por **0.5 segundos** en cada ciclo del `while`. Esto se hace para que el usuario pueda leer los mensajes de `print(errorx, errory)` antes de que la pantalla se actualice de nuevo, ya que el video normalmente se ejecuta demasiado rápido.

   
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

1. **Conexión Bluetooth:**
    * `import bluetooth`: Importar librería.
    * `sock = bluetooth.BluetoothSocket()`: Crea un **punto de conexión virtual** (`sock` o `socket`) que actúa como un cable de red entre la computadora y el ESP32 a través de Bluetooth.
    * `sock.connect(("10:06:1C:97:72:DA", port))`: Establece enlace mediante:
        1. La **dirección MAC** (`"10:06:..."`) única del ESP32.
        2. El **puerto** de comunicación (`port = 1`). Este bloque se repite hasta que la conexión sea exitosa.
    * `sock.settimeout(20)`: Establece un tiempo límite (20 segundos) para esperar una respuesta de la conexión antes de asumir que falló.

2. **Envío de Comandos:**
    * `if errorx > 0:`: Si la pelota está a la derecha del centro (error positivo), se ejecuta el código de envío.
    * `message = "Arriba"`: Define el texto que se quiere enviar al ESP32.
    * `sock.send(message.encode())`: Envía el `message` a través del socket Bluetooth.
    * `try... except`: Si la conexión Bluetooth se cae o hay un problema al enviar el mensaje, el programa no se detiene, sino que captura el error e intenta continuar.
