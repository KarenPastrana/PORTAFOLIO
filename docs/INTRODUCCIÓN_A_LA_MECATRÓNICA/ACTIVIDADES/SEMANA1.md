# Actividad 1

### Objetivos
- Usar el circuito integrado 555 en modo astable para prender y apagar un LED en el rango de 1 a 5 segundos.  
- Calcular el valor de las resistencias y capacitor para cumplir con el rango de tiempo.  
- Armar el circuito para verificar que el LED parpadee en el rango esperado.  

### **Marco Teórico**
El temporizador 555 es un circuito integrado destacado por su versatilidad y facilidad de uso, el cual puede configurarse en tres modos: monoestable, biestable y astable.  

En el modo astable, el temporizador 555 funciona como un oscilador, es decir, un generador de señales electrónicas periódicas. En este caso produce una onda cuadrada, donde la salida alterna entre el nivel alto (VCC) y el nivel bajo (0 V). La frecuencia de oscilación depende de dos resistencias externas (R1 y R2) y un capacitor (C1).  

Las fórmulas que definen el tiempo en nivel alto (TH), el tiempo en nivel bajo (TL) y la frecuencia (F) son:  

<p>
 <img src="../../assets/imgs/S1_Formulas.jpg" alt="Formulas" width="320";">
</p>

[Calculadora 555 Timer](https://www.digikey.com.mx/es/resources/conversion-calculators/conversion-calculator-555-timer?srsltid=AfmBOopbM2F4kBKWD8n8-fVGb5gEoQxKXo3YCXbVUPw4arBwxIQpEXOX)

De esta manera, es posible determinar los valores adecuados de resistencias y capacitor para obtener el tiempo de encendido y apagado deseado del LED.

### **Metodología**
Se realizó el cálculo de los valores de R<sub>1</sub>, R<sub>2</sub> y C<sub>1</sub> utilizando las fórmulas del modo astable para que el LED parpadeara en un rango de 1 a 5 segundos. Posteriormente, se armó el circuito en la protoboard y se realizaron las conexiones a la fuente de alimentación. Finalmente, se verificó el funcionamiento y comportamiento mediante un osciloscopio, observando la forma de onda cuadrada generada y comparándola con los tiempos calculados.  

### Materiales
- 1 protoboard  
- 1 multímetro  
- Puntas para fuente  
- Jumpers hembra-hembra  
- 1 LED  
- 1 resistencia de 6.8 KΩ  
- 2 resistencias de 20 KΩ  
- 1 capacitor de 220 µF  
- 1 circuito integrado 555  

### Procedimiento
1. Calcular los valores de resistencias y capacitor para lograr un periodo de encendido y apagado del LED en un rango de 1-5 segundos.  
<p>
 <img src="../../assets/imgs/S1_Calculos.jpg" alt="Calculos" width="320";">
</p>
2. Armar el circuito 555 en el protoboard y alimentarlo conectando sus pines a Vcc y GND.  
3. Conectar las resistencias y el capacitor de acuerdo a la configuración astable del 555.  
4. Añadir el LED con su resistencia.  
5. Alimentar el circuito con la fuente alimentadora.  
6. Verificar el parpadeo del LED, medir la señal en el osciloscopio y observar la forma de onda.
<p>
 <img src="../../assets/imgs/S1_Final.jpg" alt="Final" width="320";"> 
</p>

### Resultados
- El LED parpadeó dentro del rango esperado: 5 segundos en nivel alto y 3 segundos en nivel bajo.  
- En el osciloscopio se pudo observar ondas cuadradas con el periodo calculado.  
- La frecuencia obtenida fue cercana a 0.133 Hz.
<p>
<video width="320" controls>
  <source src="../../assets/imgs/S1_VideoMovVoltaje.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>

<video width="320" controls>
  <source src="../../assets/imgs/S1_VideoFinal.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>
 </p>

### Conclusiones
De acuerdo con los resultados obtenidos, se concreta que el circuito integrado 555 en modo astable permite generar señales de onda cuadrada que pueden usarse para controlar dispositivos como LEDs.  

Además, gracias a las fórmulas, los cálculos de resistencias y capacitor concuerdan con los tiempos de parpadeos observados, tanto en el protoboard como en el osciloscopio, lo que permitió comprobar de manera visual y cuantitativa el funcionamiento correcto del temporizador.  

### Bibliografía
- https://www.allaboutcircuits.com/tools/555-timer-astable-circuit/  
- https://resources.altium.com/es/p/everything-you-need-know-about-oscillators  
- https://latexeditor.lagrida.com/  
