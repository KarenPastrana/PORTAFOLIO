# Actividad 6: Control de un Servo 

## **Objetivo**
Comprender el funcionamiento de un motor servo controlado mediante una señal PWM utilizando una placa ESP32. El objetivo específico es lograr que el servo se mueva a posiciones predeterminadas (0°, 90° y 180°) de manera secuencial y controlada.

---

## **Marco Teórico**
Los servomotores son actuadores que permiten controlar de manera precisa la posición angular de su eje, generalmente en un rango de 0° a 180°. Estos motores reciben una señal PWM (modulación por ancho de pulso) desde un microcontrolador, donde la duración del pulso determina el ángulo de giro del servo.

La ESP32 es un microcontrolador que permite generar señales PWM con resolución y frecuencia configurables, ideal para controlar servos. La función `ledcWrite()` se utiliza para enviar la señal PWM al pin correspondiente, y la función `map()` permite convertir un rango de grados a valores adecuados para la señal PWM.

---

## **Metodología**
Se conectó un servo a la ESP32 en una protoboard, asignando un pin para la señal PWM y conectando alimentación y tierra. Se programó en Arduino IDE el movimiento secuencial del servo a 0°, 90° y 180°, usando la función `map()` para convertir los grados en el ciclo de trabajo PWM correspondiente. Finalmente, se verificó físicamente el movimiento del servo y se ajustaron los retardos para observar cada posición de manera clara.

---

## **Materiales**
- 1 placa ESP32 (DOIT ESP32 DEVKIT V1)  
- 1 servomotor  
- Protoboard  
- Cables jumpers  
- Fuente de alimentación para el servo  
- Computadora con Arduino IDE  
- Cable USB para conexión y carga del código  

---

## **Procedimiento**

1. Se conectó la ESP32 a la protoboard.  
1. Se conectó el servo a la protoboard: el pin de señal al pin 12 de la ESP32, alimentación a 5V y tierra a GND.  
1. Se escribió el siguiente código en Arduino IDE:

    ```cpp
    #define pwm 12 // Pin de señal del servo
    
    int duty = 0;
    int grados = 0;
    
    void setup() {
      pinMode(pwm, OUTPUT);
      ledcAttachChannel(pwm, 50, 12, 0); // Configuración PWM
      Serial.begin(115200);
    }
    
    void loop() {
      grados = 0;
      duty = map(grados, 0, 180, 205, 410);
      Serial.print("Pos: ");
      Serial.println(duty);
      ledcWrite(pwm, duty);
      delay(1000);
    
       grados = 90;
      duty = map(grados, 0, 180, 205, 410);
      Serial.print("Pos: ");
      Serial.println(duty);
      ledcWrite(pwm, duty);
      delay(1000);
    
      grados = 180;
      duty = map(grados, 0, 180, 205, 410);
      Serial.print("Pos: ");
      Serial.println(duty);
      ledcWrite(pwm, duty);
      delay(1000);
    }
     ```

1. Se verificó y cargó el programa a la ESP32.
1. Finalmente, se observó el funcionamiento: el servo se movió a las posiciones de 0°, 90° y 180° de forma secuencial, repitiéndose continuamente.

---

## **Resultado**
El servo respondió correctamente a los comandos del microcontrolador, moviéndose a las posiciones predeterminadas según el código. Los valores de PWM calculados mediante map() permitieron controlar de manera precisa el ángulo del eje del servo. La información enviada por el monitor serial coincidió con las posiciones observadas físicamente.

---

## **Conclusión**
La práctica permitió comprender cómo controlar un servomotor mediante PWM utilizando la ESP32. Se logró que el servo se moviera a las posiciones de 0°, 90° y 180° de manera secuencial, demostrando la correcta programación y conexión del circuito. Además, se reforzó el entendimiento de la relación entre señal PWM, ciclo de trabajo y movimiento angular en servomotores, así como la importancia de monitorear la señal y el hardware para un control preciso.
