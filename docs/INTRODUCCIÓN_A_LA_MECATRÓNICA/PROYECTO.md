# Proyecto Carro Robot Controlado por Bluetooth

## Introducción
El objetivo del proyecto fue construir un **carro robot controlado por Bluetooth** capaz de **participar en una competencia de futbol de robots**, moviendo una pelota pequeña e intentando marcar goles en las porterías.  

El equipo se dividió en **tres grupos**:  

- **Electrónica:** cableado, motores, puente H, fusibles, baterías, etc.  

- **Programación:** desarrollo del código para controlar el robot mediante un control PS4, conexión bluetooth del ESP32 al control PS4.  

- **Mecánica:** diseño del chasis, carcasa del robot y palita para interactuar con la pelota.  


Antes de iniciar, realizamos una **sesión de brainstorming** para planificar roles, materiales y estrategia de diseño.

---

## Materiales Utilizados

Lista de los materiales necesarios para el proyecto:

| Material |
|----------|
| ESP32 |
| Puente H |
| Jumpers |
| Porta fusible + Fusible |
| Switch |
| Motores grises (tipo DC) |
| Motores amarillos (base) |
| Llantas |
| Control PS4 |
| Cable de datos |
| Soporte para electrónica |
| Pilas 3.7V / 2600 mAh + Porta pilas |
| Adaptadores llantas a motores |
| Programa final de control |
| MDF (para chasis y carcasas) |
| Filamento para impresión 3D (carcasa y palitas) |

---

## Procedimiento General 

### Paso 1: Planificación y asignación de tareas 
- Sesión de **brainstorming** para definir roles y responsabilidades.  
- Realización de **bocetos y diagramas** iniciales del circuito y del chasis.

### Paso 2: Trabajo por equipos

#### Electrónica
- Diseño de **diagrama del circuito** para ESP32 y motores.  
- Ensamblaje de toda la **parte electrónica**: puente H, fusibles, switch, pilas, cables y motores.  
- Entrega de la electrónica al equipo de programación para integración.

#### Programación
- Conexión del ESP32 al **control PS4 mediante Bluetooth**.  
- Desarrollo del **código de control de motores**, incluyendo:
  - Movimientos hacia adelante y atrás
  - Giros a izquierda y derecha
  - Movimientos diagonales
- Pruebas de **velocidad y respuesta** a los joysticks.  
- Código final listo para integrarse al robot.

#### Mecánica
- Colocación de **carcasa del robot** y palita diseñada previamente para mover la pelota.  
- Verificación del **chasis y llantas**, asegurando compatibilidad con electrónica y programación.

### Paso 3: Integración y pruebas finales 
- Combinación de **electrónica + programación + mecánica** en el robot completo.  
- Pruebas de **movimiento, giro y recolección de la pelota**.  
- Ajustes de velocidad, calibración de motores y revisión de conexiones.

---

## Programación del Carro 

El siguiente código permite controlar el carro usando un **control PS4**. Incluye movimientos hacia adelante, atrás, izquierda, derecha y diagonales combinadas con los joysticks.

```cpp
#include <Arduino.h>
#include <PS4Controller.h>

// Variables y pines
int Speed = 210;
#define R 0
#define L 1
int enA = 25;
int enB = 14;
int IN1 = 26;
int IN2 = 27;
int IN3 = 32;
int IN4 = 33;
int threshold = 10;

// Setup
void setup() {
  Serial.begin(115200);
  PS4.begin("98:3b:8f:fc:0c:82");
  Serial.println("Esperando control PS4...");
  ledcAttachChannel(enA, 5000, 8, R);
  ledcAttachChannel(enB, 5000, 8, L);
  pinMode(IN1, OUTPUT); pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT); pinMode(IN4, OUTPUT);
  stop();
}

// Loop principal
void loop() {
  if (PS4.isConnected()) {
    Speed = map(PS4.R2Value(), 0, 255, 210, 255);
    if (PS4.Up()) forward();
    else if (PS4.Down()) backward();
    else if (PS4.Left()) left();
    else if (PS4.Right()) right();
    else {
      int lx = PS4.LStickX(); int ly = PS4.LStickY();
      int rx = PS4.RStickX(); int ry = PS4.RStickY();
      if (abs(ly) > threshold || abs(lx) > threshold || abs(ry) > threshold || abs(rx) > threshold) {
        int forwardSpeed = map(-ly, -128, 127, -Speed, Speed);
        int turnSpeed = map(lx, -128, 127, -Speed, Speed);
        int diagX = map(rx, -128, 127, -Speed, Speed);
        int diagY = map(-ry, -128, 127, -Speed, Speed);
        int leftMotor = constrain(forwardSpeed + turnSpeed + diagY + diagX, -255, 255);
        int rightMotor = constrain(forwardSpeed - turnSpeed + diagY - diagX, -255, 255);
        setMotor(leftMotor, rightMotor);
      } else stop();
    }
    delay(50);
  }
}

// Funciones de movimiento
void forward() {
ledcWrite(R, Speed);
ledcWrite(L, Speed);
digitalWrite(IN1, LOW);
digitalWrite(IN2, HIGH);
digitalWrite(IN3, HIGH);
digitalWrite(IN4, LOW);
}

void backward() {
ledcWrite(R, Speed);
ledcWrite(L, Speed);
digitalWrite(IN1, HIGH);
digitalWrite(IN2, LOW);
digitalWrite(IN3, LOW);
digitalWrite(IN4, HIGH);
}
void left() {
ledcWrite(R, Speed);
ledcWrite(L, Speed);
digitalWrite(IN1, LOW);
digitalWrite(IN2, HIGH);
digitalWrite(IN3, LOW);
digitalWrite(IN4, HIGH);
}
void right() {
ledcWrite(R, Speed);
ledcWrite(L, Speed);
digitalWrite(IN1, HIGH);
digitalWrite(IN2, LOW);
digitalWrite(IN3, HIGH);
digitalWrite(IN4, LOW);
}
void stop() {
ledcWrite(R, 0);
ledcWrite(L, 0);
digitalWrite(IN1, LOW);
digitalWrite(IN2, LOW);
digitalWrite(IN3, LOW);
digitalWrite(IN4, LOW);
}
void setMotor(int leftMotor, int rightMotor) {
  if (leftMotor >= 0) {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  ledcWrite(L, leftMotor);
}else {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  ledcWrite(L, -leftMotor);
  }
  if (rightMotor >= 0) {
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  ledcWrite(R, rightMotor);
}else {
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  ledcWrite(R, -rightMotor);
  }
}
```

---

## Resultados

- **Movimiento correcto del robot** usando el control PS4.  
- Pruebas exitosas de **velocidad, giros y movimientos diagonales**.  
- El robot **interactuó con la pelota y pudo marcar goles** durante las pruebas.  
- Integración efectiva de los tres equipos (electrónica, programación y mecánica) para un **ensamblaje completo y funcional**.  

**Nota:** Las pruebas ayudaron a calibrar la velocidad y la respuesta de los motores, así como a ajustar la palita de recolección de la pelota.

---

## Conclusión

- Se integraron **electrónica, programación y mecánica** en un proyecto práctico.  
- Se logró una **sincronización hardware-software**, resultando en un robot funcional.  
- El proyecto fortaleció habilidades de **trabajo en equipo, planificación y resolución de problemas**.  
- Se comprobó que el robot podía **jugar futbol y competir con éxito**, cumpliendo el objetivo del proyecto.

💡 **Reflexión final:** Este proyecto demostró la importancia de la colaboración entre diferentes disciplinas y permitió crear un producto funcional y educativo, combinando creatividad, ingeniería y programación.



