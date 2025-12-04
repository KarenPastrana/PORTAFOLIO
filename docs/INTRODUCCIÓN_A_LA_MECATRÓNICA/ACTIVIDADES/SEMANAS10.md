# Actividad 10: Continuación Captura, Visualización de Video en Tiempo Real e iniciación del poryecto

## Idenntificar pelota verde:

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

<div align="center">
<img src="../../assets/imgs//S10I1.png" alt="/Servo" width="310">
</div>

---

## Detallado de identificación de contorno de pelota:

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

<div align="center">
<img src="../../assets/imgs//S10I2.png" alt="/Servo" width="310">
</div>

---

## Detallado de identificación de contorno de pelota y asignación de errores:

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

<div align="center">
<img src="../../assets/imgs//S10I3.png" alt="/Servo" width="310">
</div>

---

## Código para conectar ESP32 mediante Bluetooth:

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
