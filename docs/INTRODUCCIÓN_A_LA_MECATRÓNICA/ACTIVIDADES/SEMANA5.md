# Actividad 5: Control de velocidad y dirección de un motor

## **Objetivo**
Implementar el control de un motor de corriente directa (DC) utilizando un puente H y la placa ESP32, para regular su velocidad mediante modulación por ancho de pulso (PWM) y su dirección de giro.

---

## **Marco teórico**
Un **motor DC** convierte energía eléctrica en energía mecánica rotacional. El **puente H** permite controlar la dirección del giro invirtiendo la polaridad aplicada al motor, mientras que la **modulación por ancho de pulso (PWM)** se utiliza para ajustar su velocidad variando el ciclo de trabajo de la señal enviada al motor.

La **ESP32** dispone de canales de PWM integrados, que pueden configurarse mediante las funciones `ledcAttachChannel()` y `ledcWrite()`.  
Estas permiten generar señales con frecuencias y resoluciones específicas para controlar dispositivos como motores o servos.

---

## **Metodología**
La práctica consistió en conectar un motor DC a la ESP32 mediante un puente H, controlando su velocidad con señales PWM. Se utilizó un ciclo de incremento y decremento del valor PWM para observar los cambios en la velocidad de rotación del motor.

---

## **Materiales**
- 1 placa ESP32 (DOIT ESP32 DEVKIT V1)
- Protoboard  
- Puente H (L298N) 
- 1 Motor DC con caja reductora 
- Fuente de alimentación externa  
- Cables para fuente  
- Cables jumpers
- Computadora con Arduino IDE
- Cable USB para conexión y carga del código

---

## **Procedimiento**
1. Se colocó la ESP32 en la protoboard.  
1. Se conectó el puente H de modo que los pines de control `in1` y `in2` se conectaran a los pines 32 y 33 de la ESP32.  
1. Se conectaron los terminales del motor a las salidas del puente H.  
1. Se conectó la fuente de alimentación al puente H y al motor, verificando la polaridad.  
1. Se escribió y cargó el siguiente código en Arduino IDE:

    ```cpp
    #define in1 32 
    #define in2 33
    int var = 20;

    void setup() {
      pinMode(in1, OUTPUT);
      pinMode(in2, OUTPUT);
      ledcAttachChannel(25, 1000, 8, 0);
    }

    void loop() {
      for(int i = 1; i <= 255; i++){
        ledcWrite(25, i);
        digitalWrite(in1, 1);
        digitalWrite(in2, 0);
        delay(10);
      }
      for(int i = 255; i >= 1; i--){
        ledcWrite(25, i);
        digitalWrite(in1, 1);
        digitalWrite(in2, 0);
        delay(10);
      }
    }
    ```

1. Se verificó y cargó el programa a la ESP32.  
1. Se observó el comportamiento del motor mientras su velocidad aumentaba y disminuía progresivamente.  

---

## **Resultado**
El motor giró correctamente en una dirección, incrementando y reduciendo su velocidad de manera gradual según los valores PWM enviados desde la ESP32. Se confirmó el control efectivo de velocidad mediante el ciclo de modulación programado.

<img src="../../assets/imgs/MotorComp.jpg" alt="MotorComp" width="450">

<video width="500" controls>
  <source src="../../assets/Videos/Motor.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>



---

## **Conclusión**
La práctica permitió comprender el uso del **PWM en la ESP32** para controlar la velocidad de un motor DC, así como la función del **puente H** en la inversión de polaridad. Se logró un control preciso del giro del motor, demostrando la utilidad de la programación con PWM para el manejo de actuadores eléctricos en proyectos de robótica y automatización.

