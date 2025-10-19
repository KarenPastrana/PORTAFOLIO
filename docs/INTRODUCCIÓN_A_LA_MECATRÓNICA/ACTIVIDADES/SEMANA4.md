# Actividad 4: Control de un LED mediante conexión Bluetooth

## **Objetivo**
Implementar la comunicación inalámbrica entre una ESP32 y un dispositivo móvil mediante Bluetooth, con el propósito de encender y apagar un LED a través de comandos enviados desde la aplicación Serial Bluetooth Terminal.

---

## **Marco Teórico**
El Bluetooth es una tecnología de comunicación inalámbrica de corto alcance que permite el intercambio de datos entre dispositivos electrónicos. En los microcontroladores como la ESP32, esta función puede utilizarse para controlar periféricos externos mediante comandos enviados desde un dispositivo móvil u otro sistema compatible.

La ESP32 incluye un módulo Bluetooth integrado que puede configurarse como maestro o esclavo, permitiendo la creación de conexiones con aplicaciones de control remoto. En este proyecto, la placa recibe mensajes tipo texto (“ON” y “OFF”) desde la aplicación Serial Bluetooth Terminal para controlar un LED, que funciona como una salida digital. Este proceso permite entender la interacción entre comunicación inalámbrica y control de hardware mediante programación.

---

## **Metodología**
Se configuró un circuito sencillo que permitiera controlar un LED desde un dispositivo móvil mediante comunicación Bluetooth. La ESP32 fue programada en el entorno Arduino IDE usando la biblioteca BluetoothSerial.h para habilitar la conexión y comunicación con el móvil. Luego se enviaron comandos desde la aplicación Serial Bluetooth Terminal para verificar la respuesta del sistema.

---

## **Materiales**
- 1 ESP32 (DOIT ESP32 DEVKIT V1)
- 1 LED
- 1 Resistencia de 220 Ω 
- 1 Protoboard
- Dispositivo móvil
- Aplicación Serial Bluetooth Terminal
- Computadora con Arduino IDE
- Cable USB para conexión y carga del código

---

## **Procedimiento**

1. Se conectó la ESP32 en la protoboard.  
1. Se colocó el LED con su resistencia en serie para evitar sobrecorriente.  
1. Se conectó el pin del LED a un pin digital de la ESP32 configurado como salida.  
1. Se escribió y cargó el siguiente código al microcontrolador:

    ```cpp
    #include "BluetoothSerial.h"
    BluetoothSerial SerialBT;
    
    const int led = 33;
    
    void setup() {
      Serial.begin(115200);
      SerialBT.begin("Sam_ESP32"); // Nombre del dispositivo Bluetooth
      pinMode(led, OUTPUT);
    }
    
    void loop() {
      if (SerialBT.available()) {
        String mensaje = SerialBT.readString();
        Serial.println("Recibido: " + mensaje);
    
        if (mensaje == "ON") {
          digitalWrite(led, HIGH);
        } else if (mensaje == "OFF") {
          digitalWrite(led, LOW);
        }
      }
      delay(100);
    }
    ```

1. En el dispositivo móvil se abrió la aplicación **Serial Bluetooth Terminal**.  
1. Se buscó y emparejó la ESP32 con el nombre configurado en el código.  
1. Desde la aplicación se enviaron los mensajes **“ON”** y **“OFF”** para encender y apagar el LED.  
1. Se observó la respuesta física del circuito y los mensajes recibidos en el monitor serial


---

## **Resultados**
La comunicación Bluetooth se estableció correctamente entre la ESP32 y el dispositivo móvil. Al enviar el mensaje “ON” desde la aplicación, el LED se encendió, y al enviar “OFF”, el LED se apagó. Además, el monitor serial mostró los mensajes recibidos, confirmando la correcta recepción de datos. Esto validó el funcionamiento tanto del módulo Bluetooth como del control digital del LED.

<img src="../../assets/imgs/BLUELED.jpg" alt="BLUELED" width="450">

<video width="500" controls>
  <source src="../../assets/Videos/BLUELED.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>

---

## **Conclusiones**
En esta práctica se logró controlar un LED de manera inalámbrica mediante conexión Bluetooth entre la ESP32 y un dispositivo móvil, comprobando la capacidad del microcontrolador para recibir y procesar comandos externos. La actividad permitió comprender cómo se implementa la comunicación serial por Bluetooth y su integración con salidas digitales, demostrando el potencial de la ESP32 para aplicaciones de automatización y control remoto.

---

## **Bibliografía**
