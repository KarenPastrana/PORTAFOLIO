# Actividad 11: Continuación del Proyecto (Seguimiento de Pelota y Control por Bluetooth)

## 1. Detección de Pelota Verde mediante Visión por Computadora:
Captura video, aplica una máscara *HSV* y localiza la pelota identificando su contorno, centro y radio.

**Código Python:**

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

El siguiente código se carga en el microcontrolador **ESP32**. Abre un canal de comunicación Bluetooth y recibe lo que la computadora le está enviando.

**Código Arduino:**

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

1. **Preparación del Bluetooth:**
    * `#include "BluetoothSerial.h"`: Librería de comunicación.
    * `BluetoothSerial SerialBT;`: Crea el objeto `SerialBT`, que es el canal que el ESP32 usará para comunicarse por Bluetooth.
    * `SerialBT.begin(device_name);`: Enciende el módulo Bluetooth del ESP32 y le asigna un nombre (`"ESP32_ROBOT"`) para que la computadora lo pueda encontrar y emparejar.
2. **Bucle Principal (`void loop()`):**
    * `if (SerialBT.available()) { ... }`: Comprueba si **hay datos disponibles** (mensajes) que la computadora ha enviado por Bluetooth (como el comando `"Arriba"`). Si hay datos, los lee.
    * `Serial.write(SerialBT.read());`: Lee el mensaje recibido por Bluetooth (`SerialBT.read()`) y lo imprime en el monitor serial.

El código de Python detecta una condición (`errorx > 0`) y envía una palabra (`"Arriba"`). El código del ESP32 recibe esa palabra y, aunque este ejemplo solo la imprime, en una aplicación real, el ESP32 usaría esa palabra para encender un motor, mover un brazo o realizar alguna acción física.

<div align="center">
<img src="../../assets/imgs//S11I1.jpeg" alt="/Servo" width="500">
</div>

---

## 2. Envío de Errores de Posición (X,Y) al ESP32 para Lectura en Arduino:
Obtiene el centro de la pelota, calcula el error en X y Y y lo envía al ESP32 como una cadena “errorX,errorY”.

**Código de Python:**

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

- Los errores `errorx` y `errory` se convierten a enteros (`int(...)`) antes de ser usados, lo que simplifica su manejo.


**Código de Arduino:**

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

**Lectura y Separación del Mensaje:**
* `msj = Serial.readStringUntil('\n');`: Recibe toda la cadena de texto enviada por Python, terminando solo cuando encuentra el carácter de nueva línea (`\n`).
* `String errorx = msj.subString(0, msj.indexOf(','));`:
    * `msj.indexOf(',')`: Encuentra la posición exacta de la coma (`,`) en la cadena.
    * `msj.subString(0, ...)`: Extracción de Error X.
    * `String errory = msj.subString(msj.indexOf(',') + 1);`: Extracción de Error Y.
    * `int x = errorx.toInt(); / int y = errory.toInt();`: Las porciones extraídas son cadenas de texto. Estas funciones las convierten en números enteros (`int`) para cálculos matemáticos.
      
La computadora calcula el desplazamiento de la pelota y lo empaqueta como `"X,Y\n"`. El ESP32 recibe esa cadena, la desempaqueta usando la coma como referencia, y obtiene los valores numéricos `x` e `y` listos para ser usados en un algoritmo de control.

<div align="center">
<img src="../../assets/imgs//S11I2.png" alt="/Servo" width="500">
</div>

---

## 3. Control de Servomotores según los Errores de la Pelota
El ESP32 recibe los errores enviados por Python, los convierte en ángulos y mueve suavemente los servos para seguir la pelota.

El código suministra las variables de control al ESP32.

**Código de Python:**

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

El siguiente código recibe los errores, los procesa matemáticamente y los traduce en movimientos suaves para los servomotores.

**Código de Arduino:**

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

1. **Preparación y Librerías:**
   - `#include <ESP32Servo.h>`: librería de servos.
   - `Servo servoX; / Servo servoY;`: Crea dos objetos virtuales para controlar los servomotores. `servoX` controlará el movimiento horizontal (eje X) y `servoY` el vertical (eje Y).
   - `minAngle = 60; / maxAngle = 120;`: Define un rango seguro de movimiento para los servos.
   - `smoothFactor = 0.2;`: Es un valor decimal que se utiliza para suavizar el **movimiento**. Un factor bajo (cercano a 0) hace que el movimiento sea lento; un factor alto (cercano a 1) hace que el movimiento sea rápido.
   - `servoX.attach(27); / servoY.attach(14);`: Conexión de Pines.
   - `servoX.write(90); / servoY.write(90);`: Mueve ambos servos a la posición central (90°) al inicio.
2. **Mapeo del Error a Ángulo**
   - **Mapeo horizontal** `angX = map(x, -200, 200, 0, 180);`: Transforma un número de un rango a otro de forma proporcional. Si el error `x` va de -200 (izquierda) a 200 (derecha), se mapea a un ángulo que va de 0° a 180°.
   - **Mapeo  vertical** `angY = map(y, -200, 200, 0, 180);`: Realiza la misma transformación para el error vertical y en su ángulo correspondiente.
   - `angX = constrain(angX, minAngle, maxAngle);`: Asegura que el ángulo calculado (`angX`) nunca exceda el rango seguro definido (`minAngle=60` y `maxAngle=120`). Esto evita que el mecanismo se mueva fuera de sus límites físicos.
3. **Suavizado de Movimiento:**
   - `currentX = servoX.read();`: Obtiene el **ángulo actual** en el que se encuentra el servo X.
   - `smoothX = currentX + (angX - currentX) * smoothFactor;`: El servo se mueve lentamente hacia la posición objetivo, generando un movimiento más suave.
   - `servoX.write(smoothX);`: Envía el ángulo suavizado al servo, moviendo la plataforma.

El resultado es un sistema de seguimiento activo donde la visión detecta la pelota, calcula su error y el ESP32 mueve la plataforma con los servos para intentar centrar ese error.
