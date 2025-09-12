# Actividad 1

## Objetivos
<p>
-Usar el circuito integrado 555 en modo astable para prender y apagar un LED en el rango de 1 a 5 segundos.
<p>
  
-Calcular el valor de las resistencias y capacitor para cumplir con el rango de tiempo.
<p>
-Armar el circuito para verificar que el LED parpadee en el rango esperado.
  <p>
    
<div>

## Introducción (Marco Teórico (teoría y metodología))
<p>
  
### **Marco Teórico**
<p>
El temporizador 555 es un circuito integrado destacado por su versatilidad y facilidad de uso, el cual puede configurarse en tres modos: monoestable, biestable y astable. 
<p>
En el modo astable, el temporizador 555 funciona como un oscilador, es decir, un generador de señales electrónicas periódicas. En este caso produce una onda cuadrada, donde la salida alterna entre el nivel alto (VCC) y el nivel bajo (0 V). La frecuencia de oscilación depende de dos resistencias externas (R1 y R2) y un capacitor (C1). 
<p>
Las fórmulas que definen el tiempo en nivel alto (TH), el tiempo en nivel bajo (TL) y la frecuencia (F) son: 
<p><p>
$$T_H=0.693⋅(R_1+R_2)⋅C_1$$
</p><p>
$$T_L=0.693⋅R_2⋅C_1$$
</p><p>
$$f=\frac{​1.44​}{(R_1+2R_2)⋅C_1}$$
  
<p>
De esta manera, es posible determinar los valores adecuados de resistencias y capacitor para obtener el tiempo de encendido y apagado deseado del LED.
<p>

### **Metodología**  
<p>
Se realizó el cálculo de los valores de R<sub>1</sub>, R<sub>2</sub> y C<sub>1</sub> utilizando las fórmulas del modo astable para que el LED parpadeara en un rango de 1 a 5 segundos. Posteriormente, se armó el circuito en la protoboard y se realizaron las conexiones a la fuente de alimentación. Finalmente, se verificó el funcionamiento y comportamiento mediante un osciloscopio, observando la forma de onda cuadrada generada y comparándola con los tiempos calculados. 
<p>

</div>

## Materiales
<p>
- 1 protoboard
</p>
- 1 multímetro
<p>
- Puntas para fuente
<p>
- Jumpers hembra-hembra
<p>
- 1 LED
<p>
- 1 resistencia de 6.8 KΩ
<p>
- 2 resistencias de 20 KΩ
<p>
- 1 capacitor de 220 µF
<p>
- 1 circuito integrado 555
<p>
  
</div>

## Procedimiento
</div>
<p>
1. Calcular los valores ded resittencias y capacitor para lograr un periodo de encendido y apagado del LED en un rango de 1-5 segundos. <p>
2. Armar el circuito 555 en el protoboard y alimentarlo conectando sus pines a Vcc y GND. <p>
3. Conectar las resistencias y el cpacitor de acuerdo a la configuración astable del 555. <p>
4. Añadir el LED con su resistencia.<p>
5. Alimentar el circuito con la fuente alimentadora. <p>
6. Verificar el parpadeo del LED, medir la señal en el osciloscopio y observar la forma de onda.
<p>
</div>
  
## Resultados
</div>
<p>
- El LED parpadeó dentro del rango esperado: 5 segundos en nivel alto y 3 segundos en nivel bajo.
<p>
- En el osciloscopio se pudo observar ondas cuadradas con el periodo calculado.
<p>
- La freuencia obtenida fue cercana a 0.133 Hz.
<p>
</div>
  
## Conclusiones
</div>
<p>
De acuerdo con los resultados obtenidos, se concreta que el circuito integrado 555 en nmodo astable permite generar señales de onda cuadrada que pueden usarse para controlar dispositivos como LEDs. 
<p>
Además, gracias a las fórmulas, los cálculos de resistencias y cpacitor concuerdan con los tiempos de parpadeos observados, tanto en el protoboard como en el osciloscopio, lo que permitió comprobar de manera visual y cuantitativa el funcionamiento correcto del temporizador.
<p>
</div>
  
## Bibliografía
</div>
