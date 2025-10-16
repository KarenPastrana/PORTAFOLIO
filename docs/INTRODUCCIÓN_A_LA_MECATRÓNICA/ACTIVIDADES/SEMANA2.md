# Actividad 2: Encendido y Apagado de un LED

## **Objetivo**
Comprender el funcionamiento básico de la placa ESP32 mediante el control de un diodo LED, aplicando conceptos de programación en Arduino IDE y el uso de componentes electrónicos como la resistencia y el protoboard.

---

## **Marco Teórico**
El LED (Light Emitting Diode) es un componente electrónico que emite luz cuando una corriente eléctrica pasa a través de él. Para evitar que el LED se queme por exceso de corriente, se utiliza una resistencia que limita el paso de electricidad.

La ESP32 es una placa de desarrollo basada en un microcontrolador que incluye WiFi y Bluetooth. Puede programarse con Arduino IDE, utilizando código en lenguaje C/C++. Uno de los primeros ejercicios al aprender a usar una placa microcontroladora es hacer parpadear un LED, lo que permite comprobar la correcta configuración del entorno de desarrollo y la comprensión de las funciones básicas:

- **pinMode(pin, OUTPUT)** define un pin como salida.
- **digitalWrite(pin, HIGH/LOW)** envía una señal de encendido o apagado.
- **delay(tiempo)** pausa la ejecución del programa por un tiempo determinado (en milisegundos).

  ---

## **Metodología**

Se utilizó una metodología experimental. Se conectó el LED a la placa ESP32 mediante un protoboard y una resistencia, y se cargó un código desde Arduino IDE que permite que el LED se encienda y apague cada segundo. El experimento permitió observar de forma práctica el uso de salidas digitales.

---

## **Materiales**
- 1 placa ESP32 (DOIT ESP32 DEVKIT V1)
- 1 LED 
- 1 resistencia de 220 Ω
- 1 protoboard
- Computadora con Arduino IDE instalado
- Cable USB para conexión y carga del código

---

## **Procedimiento**
1. Se conectó la ESP32 a la computadora mediante el cable USB.
1. Se abrió Arduino IDE y se configuró la placa “DOIT ESP32 DEVKIT V1”.
1. En el protoboard, se conectó el ánodo del LED al pin 13 de la ESP32 mediante una resistencia, y el cátodo al GND.
1. Se escribió y cargó el siguiente código:
```
cpp
const int led = 13;

void setup() {
  Serial.begin(115200);
  pinMode(led, OUTPUT);
}

void loop() {
  digitalWrite(led, 1);
  delay(1000);
  digitalWrite(led, 0);
  delay(1000);
}
```
</pre>
5. Se observó el parpadeo del LED, que se encendía y apagaba con intervalos de un segundo.

---

## **Resultados**
El LED encendió y apagó correctamente, demostrando el funcionamiento del código y las conexiones. La ESP32 respondió adecuadamente a las instrucciones digitales y se verificó la correcta comunicación entre el entorno de programación y el hardware.

---

## **Conclusiones**
La práctica permitió comprobar el funcionamiento básico de la placa ESP32 al controlar el encendido y apagado de un LED, aplicando conceptos fundamentales de electrónica y programación. Se comprendió el uso de los pines digitales como salidas, la importancia de la resistencia para proteger el LED y el papel de las funciones básicas del lenguaje Arduino, como pinMode(), digitalWrite() y delay(). Además, se verificó la correcta comunicación entre el hardware y el entorno Arduino IDE, sentando una base sólida para el desarrollo de proyectos más avanzados con esta placa.


---

## **Bibliografía**





