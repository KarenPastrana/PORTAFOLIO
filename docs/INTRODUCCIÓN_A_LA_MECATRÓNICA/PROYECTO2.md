# Proyecto2: Plataforma Stewart de 2 Servos para Balancear Pelota 

Este proyecto combina la **Visión por Computadora (OpenCV)** con el control del **ESP32** para crear una plataforma que rastrea y equilibra una pelota de color verde en tiempo real. La cámara "ve" la pelota, calcula su posición y envía órdenes a dos servomotores para mantenerla siempre en el centro.

## **I. Fase 1: Visión y Detecciión (Python)**
Aqui se localiza la pelota con precisión en el video.
1. **Configuración de la Cámara:** Capturamos el video en vivo y aplicamos un filtro de desenfoque para eliminar el ruido visual.
2. **Detección de Color (Máscara):** Convertimos la imagen a HSV y definimos el rango de color verde (`greenLower` / `greenUpper`) para crear una máscara binaria (donde la pelota es blanca y todo lo demás es negro).
3. **Localización Precisa:** Usamos los contornos y el centroide para encontrar el punto central exacto de la pelota en la pantalla.

## **Fase 2: Diseño Físico y Electrónica**
Elegimos usar dos servomotores MG995 para controlar los movimientos horizontal (X) y vertical (Y), para mover la plataforma con más torque, suficiente velocidad y estabilidad. Usamos esta imagen de referencia para el diseño de la plataforma y la colocación de los servomotores.

  <div align="center">
<img src="../../assets/imgs/PLATREF.jpg" alt="Plataforma" width="400">
</div> 
   

La estructura de la plataforma se construyó combinando corte láser e impresión 3D. La base y la superficie fueron impresas en MDF y los soportes que sostienen la plataforma, así como la parte central que permite girar la plataforma, fueron impresas en 3D. Pra un mayor movimiento de la plataforma, se hizo una especie de "L" con varillas de MDF que fueron atornilladas en los servos y en la parte superior de la plataforma. 

 <div align="center">
<img src="../../assets/imgs/2.png" alt="Plataforma" width="400">
</div> 
   
<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/SOPORTE CENTRAL.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-SLDPRT-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar SLDPRT">
  </a>
</div>

 <div align="center">
<img src="../../assets/imgs/3.png" alt="Plataforma" width="400">
</div> 
   
<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/BRAZOS X2.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-SLDPRT-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar SLDPRT">
  </a>
</div>


 <div align="center">
<img src="../../assets/imgs/7.png" alt="Plataforma" width="400">
</div> 
   
<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/SOPORTE CENTRAL.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-SLDPRT-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar SLDPRT">
  </a>
</div>


### Conexión y Comunicación Serial
Dado que el ESP32 aparece como un puerto COM en la computadora, se eligió la comunicación **Serial Virtual** (vía Bluetooth SPP o USB) para una alta velocidad y simplicidad.
- **Python (`pySerial`):** Se utilizó la librería `pySerial` para abrir y gestionar el puerto COM (`ser.write(comando.encode('utf-8'))`), enviando los datos continuamente.
- **ESP32 (`BluetoothSerial`):** El ESP32 se configuró para escuchar ese puerto serial, listo para recibir el comando de ángulos.

## **Fase 3: Fase de Control (PID) y Calibración**
Esta fue la fase más crítica, donde se implementó el algoritmo de **Control Proporcional-Integral-Derivativo (PID)** para asegurar el balanceo dinámico.

### A. Implementación del Algoritmo PID (Python)
1. **Cálculo de Error:** Se definió el punto objetivo (`target = 300`) y se calculó el error (`error = target - center`).
2. **Fórmula PID:** Se calcularon las tres componentes:
    - **Proporcional (P):** Responde al error actual (la fuerza inmediata para corregir).
    - **Integral (I):** Acumula el **error a lo largo del tiempo**, esencial para corregir errores pequeños y constantes (como el arrastre por fricción).
    - **Derivativo (D):** Responde a la **velocidad de cambio del error**, utilizado para amortiguar el sistema y evitar oscilaciones o movimientos excesivamente rápidos.
3. **Conversión a Ángulo:** El valor de `output` del PID se utilizó para calcular el ángulo final del servo `(servo_azul = FLAT_AZUL - output_y)`, convirtiendo la señal de corrección en una orden física. El código de Python no envía el error, sino directamente los **ángulos calculados y limitados** (ej. `95,112\n`) al ESP32.

### B. Herramientas de Calibración
Se implementaron dos herramientas esenciales para la calibración y el ajuste fino:
- **Ventana de Sliders:** Se creó una interfaz (`cv2.namedWindow("Ajustes PID")`) con trackbars para modificar los coeficientes **Kp, Ki y Kd en tiempo real**. Este método fue crucial para sintonizar el sistema hasta que la pelota se estabilizara.
- **Límites Físicos:** Se establecieron límites de ángulo (`max(60, min(120, servo_azul))`) en el código de Python y límites de seguridad más amplios en Arduino (`if angulo < 40`) para proteger los servos durante la calibración.

### C. Control de Bajos Niveles (Arduino/ESP32)
Para garantizar un movimiento preciso y rápido de los servos, se implementó el control sin librerías:
1. **Eliminación de Librería:** Se descartó la librería `ESP32Servo` debido a problemas de precisión y velocidad de refresco.
2. **Bit-Banging (Control Directo):** Se creó la función `moverServo` que genera el **pulso PWM** (modulación por ancho de pulso) manualmente, utilizando `delayMicroseconds`.
3. **Refresco Constante:** El bucle `loop()` llama a moverServo continuamente, asegurando que el motor mantenga su posición y esté listo para reaccionar inmediatamente al siguiente comando serial.

## Códigos finales

### A. Código Final en Python (Visión, PID y Envío de Ángulos)

```python
import cv2
import numpy as np
import serial
import time
 
# --- CONFIGURACIÓN ---
COM_PORT = 'COM7'  # <--- puerto
BAUD_RATE = 115200
 
# CONSTANTES INICIALES
FLAT_AZUL = 90
FLAT_VERDE = 110
 
# Rango de color VERDE
greenLower = (40, 70, 50)
greenUpper = (85, 255, 255)
 
# --- CONEXIÓN SERIAL ---
try:
    ser = serial.Serial(COM_PORT, BAUD_RATE, timeout=1)
    print(f"Conectado a {COM_PORT}")
    time.sleep(1)
except:
    print("ERROR: No se pudo conectar al Bluetooth.")
    exit()
 
# Buscar cámara 
cap = cv2.VideoCapture(1, cv2.CAP_DSHOW)
if not cap.isOpened():
    cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)
 
# --- VENTANA DE AJUSTES (SLIDERS) ---
def nothing(x): pass
 
cv2.namedWindow("Ajustes PID")
cv2.resizeWindow("Ajustes PID", 400, 200)
 
# Sliders.
# Formato: ("Nombre", "Ventana", ValorInicial, ValorMaximo, Funcion)
# NOTA: Los sliders solo manejan enteros. Dividiremos en el código.
cv2.createTrackbar("Kp x100", "Ajustes PID", 35, 200, nothing)  # 35 -> 0.35
cv2.createTrackbar("Ki x1000", "Ajustes PID", 0, 100, nothing)  # 0 -> 0.000
cv2.createTrackbar("Kd x100", "Ajustes PID", 15, 200, nothing)  # 15 -> 0.15
 
# Variables PID
prev_error_x = 0; prev_error_y = 0
integral_x = 0; integral_y = 0
 
print("--- SISTEMA INICIADO ---")
print("Usa la ventana 'Ajustes PID' para calibrar.")
print("Presiona 'q' para salir.")
 
while True:
    ret, frame = cap.read()
    if not ret: break
 
    frame = cv2.resize(frame, (600, 600))
    blurred = cv2.GaussianBlur(frame, (11, 11), 0)
    hsv = cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV)
 
    mask = cv2.inRange(hsv, greenLower, greenUpper)
    mask = cv2.erode(mask, None, iterations=2)
    mask = cv2.dilate(mask, None, iterations=2)
 
    cnts, _ = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
   
    # --- LEER VALORES DE LOS SLIDERS ---
    # Leemos el valor entero y lo convertimos a decimal
    val_p = cv2.getTrackbarPos("Kp x100", "Ajustes PID")
    val_i = cv2.getTrackbarPos("Ki x1000", "Ajustes PID")
    val_d = cv2.getTrackbarPos("Kd x100", "Ajustes PID")
 
    Kp = val_p / 100.0      # Ej: 35 / 100 = 0.35
    Ki = val_i / 1000.0     # Ej: 5 / 1000 = 0.005
    Kd = val_d / 100.0      # Ej: 15 / 100 = 0.15
 
    # Dibujar valores en pantalla para referencia
    cv2.putText(frame, f"P: {Kp:.2f}  I: {Ki:.3f}  D: {Kd:.2f}", (10, 30),
                cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 0, 255), 2)
 
    center = None
    servo_azul = FLAT_AZUL
    servo_verde = FLAT_VERDE
 
    if len(cnts) > 0:
        c = max(cnts, key=cv2.contourArea)
        ((x, y), radius) = cv2.minEnclosingCircle(c)
        M = cv2.moments(c)
       
        if M["m00"] > 0:
            center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"]))
           
            # Visuales
            cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), 2)
            #cv2.line(frame, center, (300, 300), (255, 0, 0), 2)
 
            # --- PID ---
            target = 300
            error_x = target - center[0]
            error_y = target - center[1]
 
            # PID X
            integral_x += error_x
            derivative_x = error_x - prev_error_x
            output_x = (Kp * error_x) + (Ki * integral_x) + (Kd * derivative_x)
            prev_error_x = error_x
 
            # PID Y
            integral_y += error_y
            derivative_y = error_y - prev_error_y
            output_y = (Kp * error_y) + (Ki * integral_y) + (Kd * derivative_y)
            prev_error_y = error_y
 
            # --- MAPEO ---
            servo_verde = int(FLAT_VERDE - output_x)
            servo_azul = int(FLAT_AZUL - output_y)
 
    else:
        # Si se pierde la bola, resetear integrales para evitar "golpes" al volver
        integral_x = 0
        integral_y = 0
        cv2.putText(frame, "CENTERING", (10, 60), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 2)
 
    # Limites físicos
    servo_azul = max(60, min(120, servo_azul))
    servo_verde = max(80, min(140, servo_verde))
 
    # Enviar al ESP32
    try:
        comando = f"{servo_azul},{servo_verde}\n"
        ser.write(comando.encode('utf-8'))
    except:
        pass
 
    cv2.imshow("Robot Vision", frame)
   
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
 
cap.release()
cv2.destroyAllWindows()
ser.close()
```

### B. Código Final en Arduino/ESP32 (Recepción y Bit-Banging de Servos)

```cpp
#include <Arduino.h>
#include "BluetoothSerial.h"
 
BluetoothSerial SerialBT;
 
// --- PINES ---
const int PIN_AZUL = 27;  // Servo Y
const int PIN_VERDE = 14; // Servo X
 
// --- VARIABLES ---
int anguloAzul = 90;   // Inicia en Plano
int anguloVerde = 110; // Inicia en Plano
 
// --- FUNCIÓN MANUAL (Bit-Banging) ---
// Genera la señal PWM directamente sin la librería Servo.h
void moverServo(int pin, int angulo) {
  // Limites duros de seguridad
  if (angulo < 40) angulo = 40;
  if (angulo > 150) angulo = 150;
 
  // Matematica: 0deg=500us, 180deg=2400us
  // Convierte el ángulo al ancho de pulso en microsegundos (500us a 2400us)
  int pulso = 500 + ((long)angulo * 1900) / 180;

  // Generación del pulso
  digitalWrite(pin, HIGH);
  delayMicroseconds(pulso);
  digitalWrite(pin, LOW);
}
 
void setup() {
  Serial.begin(115200);
 
  // NOMBRE DEL BLUETOOTH
  SerialBT.begin("ESP32_BALL_PLATE");
  Serial.println("BLUETOOTH LISTO: Conecta a 'ESP32_BALL_PLATE'");
 
  pinMode(PIN_AZUL, OUTPUT);
  pinMode(PIN_VERDE, OUTPUT);
}
 
void loop() {
  // 1. RECEPCIÓN DE DATOS
  if (SerialBT.available()) {
    // Leer hasta el salto de línea que envía Python
    String paquete = SerialBT.readStringUntil('\n');
    paquete.trim(); // Quitamos espacios basura
 
    // Buscar la coma separadora "anguloY,anguloX" ("90,110")
    int coma = paquete.indexOf(',');
    if (coma > 0) {
      String sAzul = paquete.substring(0, coma);
      String sVerde = paquete.substring(coma + 1);
 
      // Convertir a entero
      anguloAzul = sAzul.toInt();
      anguloVerde = sVerde.toInt();
    }
  }
 
  // 2. REFRESCO CONSTANTE (Vital para que no se 'suelten' los servos)
  // Se llama la función continuamente para mantener la señal PWM de los servos
  moverServo(PIN_AZUL, anguloAzul);
  moverServo(PIN_VERDE, anguloVerde);
 
  // Pausa para completar ciclo
  delay(15);
}
```


## Resultados y Conclusión

El éxito del proyecto dependió de la **interacción precisa** entre la señal de error generada por el PID en Python y la ejecución rápida del movimiento de los servos en el ESP32. La fase de **calibración (ajuste de Kp, Ki, Kd)** fue la que consumió más tiempo, ya que cada cambio afectaba la velocidad, la suavidad y la estabilidad de la respuesta de la plataforma al rastrear la pelota. El resultado final fue una plataforma estable, robusta y con capacidad de rastreo que sirve como una base sólida para futuros proyectos de robótica y automatización.


