# Actividad 11: Continuación del poryecto

## Código con OpenCV y Arduino

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
        sock.connect(("68:FE:71:0D:4B:CA", port))
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

```cpp
#include "BluetoothSerial.h"
 
String device_name = "ESP32_ROBOT";
 
// Check if Bluetooth is available
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
 
// Check Serial Port Profile
#if !defined(CONFIG_BT_SPP_ENABLED)
#error Serial Port Profile for Bluetooth is not available or not enabled. It is only available for the ESP32 chip.
#endif
 
BluetoothSerial SerialBT;
 
void setup() {
  Serial.begin(115200);
  SerialBT.begin(device_name);  //Bluetooth device name
  //SerialBT.deleteAllBondedDevices(); // Uncomment this to delete paired devices; Must be called after begin
  Serial.printf("The device with name \"%s\" is started.\nNow you can pair it with Bluetooth!\n", device_name.c_str());
}
 
void loop() {
  if (Serial.available()) {
    SerialBT.write(Serial.read());
  }
  if (SerialBT.available()) {
    Serial.write(SerialBT.read());
  }
  delay(20);
}
 

```


<div align="center">
<img src="../../assets/imgs//S11I1.jpeg" alt="/Servo" width="310">
</div>

---

## Código para verificar posición de pelota y para Arduino:

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
        sock.connect(("68:FE:71:0D:4B:CA", port))
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
 
    errorx=int(x-(w/2))
    errory=int(y-(h/2))
   
 
    mensaje = str(errorx) + ',' + str(errory) + '\n'
    print(mensaje)
 
    try:
        sock.send(mensaje.encode())  # encode the string to bytes
        print("Sent:", mensaje)
    except Exception as e:
        print("Error sending data:", e)
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    #cv2.imshow("MASK", mask)
    #cv2.imshow("RESULT", result)
 
    #Salida del bucle
    time.sleep(0.1)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

```cpp
#include "BluetoothSerial.h"
float Kp=0.2;
String device_name = "ESP32_ROBOT";
 
// Check if Bluetooth is available
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
 
// Check Serial Port Profile
#if !defined(CONFIG_BT_SPP_ENABLED)
#error Serial Port Profile for Bluetooth is not available or not enabled. It is only available for the ESP32 chip.
#endif
 
BluetoothSerial SerialBT;
String msj="";
void setup() {
  Serial.begin(115200);
  SerialBT.begin(device_name);  //Bluetooth device name
  //SerialBT.deleteAllBondedDevices(); // Uncomment this to delete paired devices; Must be called after begin
  Serial.printf("The device with name \"%s\" is started.\nNow you can pair it with Bluetooth!\n", device_name.c_str());
}
 
void loop() {
  if (Serial.available()) {
    msj = Serial.readStringUntil('\n');
    150,120
    String errorx=msj.subString(0,msj.indexOf(','));
    String errory=msj.subString(msj.indexOf(',')+1);
    int x=errorx.toInt();
    int y=errory.toInt();
  }
 
  Serial.println(x);
  Serial.println(y);
 
 
  delay(20);
}
 
```

<div align="center">
<img src="../../assets/imgs//S11I2.png" alt="/Servo" width="310">
</div>

---

## Código con posición de pelota y mover servos de acuerdo al error:

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
        sock.connect(("F0:24:F9:57:56:52", port))
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
 
    errorx=int(x-(w/2))
    errory=int(y-(h/2))
   
 
    mensaje = str(errorx) + ',' + str(errory) + '\n'
    print(mensaje)
 
    try:
        sock.send(mensaje.encode())  # encode the string to bytes
        print("Sent:", mensaje)
    except Exception as e:
        print("Error sending data:", e)
 
    #mostrar img
    cv2.imshow("ORIGINAL", frame)
    #cv2.imshow("MASK", mask)
    #cv2.imshow("RESULT", result)
 
    #Salida del bucle
    time.sleep(0.1)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
video.release()
```

``` cpp
#include <BluetoothSerial.h>
#include <ESP32Servo.h>
 
BluetoothSerial SerialBT;
 
Servo servoX;
Servo servoY;
 
int minAngle = 60;
int maxAngle = 120;
 
float smoothFactor = 0.2;
 
void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32_STEWART");
 
  servoX.attach(27);
  servoY.attach(14);
 
  servoX.write(90);
  servoY.write(90);
 
  Serial.println("Plataforma inicializada.");
}
 
void loop() {
  if (SerialBT.available()) {
    String msg = SerialBT.readStringUntil('\n');
    msg.trim();
 
    int commaIndex = msg.indexOf(',');
    if (commaIndex == -1) return;
 
    int x = msg.substring(0, commaIndex).toInt();
    int y = msg.substring(commaIndex + 1).toInt();
 
    Serial.print("Error recibido → X:");
    Serial.print(x);
    Serial.print(" Y:");
    Serial.println(y);
 
    int angX = map(x, -200, 200, 0, 180);
    int angY = map(y, -200, 200, 0, 180);
 
    angX = constrain(angX, minAngle, maxAngle);
    angY = constrain(angY, minAngle, maxAngle);
 
    int currentX = servoX.read();
    int currentY = servoY.read();
 
    int smoothX = currentX + (angX - currentX) * smoothFactor;
    int smoothY = currentY + (angY - currentY) * smoothFactor;
 
    servoX.write(smoothX);
    servoY.write(smoothY);
 
    Serial.print("ServoX = ");
    Serial.print(smoothX);
    Serial.print("  ServoY = ");
    Serial.println(smoothY);
  }
}
```
