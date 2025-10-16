# Actividad 3: Encendido y apagado de un LED Mediante un Botón

## **Objetivos**
Comprender el funcionamiento de una entrada digital mediante un botón y una salida digital mediante un LED, utilizando una placa ESP32. El objetivo específico es lograr que el LED se encienda al presionar el botón y se apague al soltarlo.

---

## **Marco Teórico**
En los microcontroladores, las entradas digitales permiten detectar dos estados: alto (HIGH) y bajo (LOW), correspondientes a los niveles lógicos 1 y 0. Un botón pulsador es un componente que cambia su estado al ser presionado, cerrando o abriendo el circuito.

Las salidas digitales, por otro lado, permiten activar o desactivar dispositivos como LEDs, motores o relés. Un LED (diodo emisor de luz) emite luz cuando circula corriente eléctrica en el sentido correcto, por lo que siempre se utiliza una resistencia limitadora para evitar daños por exceso de corriente.

En esta práctica, se utiliza una ESP32, un microcontrolador con pines configurables como entrada o salida, programado mediante el entorno Arduino IDE. Al detectar una señal HIGH proveniente del botón, el microcontrolador activa el pin de salida, encendiendo el LED.



---

## **Metodología**
Se desarrolló un circuito básico de control digital en una protoboard, utilizando una ESP32 como unidad de control. Se programó el comportamiento lógico en Arduino IDE y se verificó su funcionamiento físico conectando los componentes de manera segura y ordenada. El enfoque principal fue comprender la interacción entre hardware (componentes electrónicos) y software (código de control).


---

## **Materiales**
- 1 placa ESP32 (DOIT ESP32 DEVKIT V1)
- 1 LED
- 1 resistencia de 220 Ω
- 1 botón pulsador
- Protoboard
- Cables jumpers
- Computadora con Arduino IDE
- Cable USB para conexión y carga del código

---

## **Procedimiento**
1. Se conectó la ESP32 a la protoboard.
2. Se colocó el LED y se conectó una resistencia en serie para limitar la corriente.
3. Se conectó el botón pulsador a un pin digital configurado como entrada.
4. Se realizó el cableado con jumpers conectando el botón a 3.3V y GND, y el pin de lectura digital.
5. Se escribió el siguiente código en Arduino IDE:

const int led=33;
const int btn=32;

void setup() {
  Serial.begin(115200);
  pinMode(led, OUTPUT);
  pinMode(btn, INPUT);
}

void loop() {
  int estado = digitalRead(btn);
  if (estado == 1) {
    digitalWrite(led, 1);
  } else {
    digitalWrite(led, 0);
  }
}

6. Se verificó y cargó el programa a la ESP32.
7. Finalmente, se observó el funcionamiento: el LED se enciende al presionar el botón y se apaga al soltarlo.


---

## **Resultados**
El circuito funcionó correctamente. Al presionar el botón, la entrada digital cambió a estado alto y encendió el LED. Cuando se soltó el botón, el pin volvió a estado bajo, apagando el LED.
Esto demuestra la correcta configuración de los pines de entrada y salida, así como la comprensión del flujo lógico entre hardware y software.


---

## **Conclusiones**
La práctica permitió comprender el funcionamiento básico de las entradas y salidas digitales en la ESP32 al controlar un LED mediante un botón. Se logró que el LED se encendiera al presionar el botón y se apagara al soltarlo, demostrando la correcta programación y conexión del circuito. Además, se reforzó la importancia del uso de resistencias para proteger los componentes y se adquirió una mejor comprensión sobre la relación entre el hardware y el software en proyectos de control digital.








