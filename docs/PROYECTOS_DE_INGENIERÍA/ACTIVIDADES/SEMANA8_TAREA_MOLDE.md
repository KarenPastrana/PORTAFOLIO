# SEMANA 8 TAREA DISEÑO DE MOLDE

## Karen

Para esta tarea realicé un diseño para cortar en espuma rosa utilizando el router CNC. Como primer paso, seleccioné una imagen de inspiración, la cual me ayudó a definir la forma general y los elementos principales del modelo.

<div align="center">
<img src="../../assets/imgs/S8MOLDE_INSPIRACION.jpg" alt="Resultado" width="200">
</div>

A partir de esta referencia, enocntré una imagen con medidas y las aproximé a las que yo quería, asegurándome de considerar las dimensiones reales del material y las limitaciones de la máquina.

<div align="center">
<img src="../../assets/imgs/S8MOLDE_MEDIDAS.jpg" alt="Resultado" width="200">
</div>

Posteriormente diseñé la pieza en SolidWorks, ajustando cada detalle para que el corte fuera preciso.

<div align="center">
  <img src="../../assets/imgs/S8R1.jpg" alt="Karen" width="500">
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/DISEÑO_CNC_PARTE_1_KAREN.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-SLDPRT-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar SLDPRT">
  </a>
</div>

<div align="center">
  <img src="../../assets/imgs/S8R2.jpg" alt="Karen" width="500">
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/DISEÑO_CNC_PARTE_2_KAREN.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-SLDPRT-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar SLDPRT">
  </a>
</div>
---

## Molde Samantha
Para fabricar un molde de silicón del dinosaurio, primero definí qué objeto quería reproducir y, con esa idea clara, preparé un modelo 3D en Blender que sirviera como base para generar la cavidad del molde.

### 1. Importación y preparación del modelo 3D
Comencé importando la figura digital del dinosaurio en Blender. Una vez dentro del entorno, apliqué una inflación de aproximadamente 3 mm, con el fin de darle un margen de grosor adecuado al espacio interior del molde.

<div align="center">
  <img src="../../assets/imgs/1DinoInf.png" alt="Sam" width="500">
</div>

### 2. Diseño de la caja del molde
Después generé la caja principal, que es el volumen que envuelve al modelo y define el contorno exterior del molde. Ajusté sus dimensiones para que dejara el espacio requerido alrededor de la figura, evitando interferencias en la geometría interna.

<div align="center">
  <img src="../../assets/imgs/2DinBox.png" alt="Sam" width="500">
</div>

### 3. Creación de pestañas de ensamble
Con la caja definida, modelé las pestañas laterales, extruyéndolas hacia afuera. Estas pestañas son esenciales porque permiten unir las dos mitades del molde con tornillería y mantenerlas alineadas durante el vertido y el fraguado del silicón.

<div align="center">
  <img src="../../assets/imgs/4CorteBox.png" alt="Sam" width="230">
</div>

<div align="center">
  <img src="../../assets/imgs/5ExtBox.png" alt="Sam" width="230">
</div>


### 4. Booleano para generar la cavidad del dinosaurio
Tomando la caja ya con pestañas, apliqué una operación Boolean → Difference utilizando el modelo 3D del dinosaurio como objeto de sustracción. Esto generó la cavidad interna con la forma exacta del dinosaurio, que será luego rellenada con silicón.

<div align="center">
  <img src="../../assets/imgs/6Contra.png" alt="Sam" width="500">
</div>


### 5. Canal de vertido
Para crear el punto de entrada del silicón, añadí un cilindro y lo posicioné de manera estratégica sobre la caja. A este cilindro también se le aplicó una operación boolean para perforar un canal de vertido, por donde se inyectará el material en estado líquido.

<div align="center">
  <img src="../../assets/imgs/9Huecos.png" alt="Sam" width="500">
</div>


### 6. División del molde
Una vez listo el volumen completo, procedí a cortar el molde en dos mitades a lo largo de un plano central. Esto permite desmoldar la pieza final y facilita el ensamblaje y cierre durante el vertido.

<div align="center">
  <img src="../../assets/imgs/10MOLDESIN.png" alt="Sam" width="500">
</div>

### 7. Orificios para tornillos
Finalmente, perforé las pestañas mediante cilindros adicionales para generar los orificios de tornillo, necesarios para alinear y fijar ambas mitades durante el proceso de vaciado.

<div align="center">
  <img src="../../assets/imgs/11MOLDEDINO.png" alt="Sam" width="500">
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/ImageToStl.com_Dino.zip" download>
    <img src="https://img.shields.io/badge/Descargar-zip-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar usdz">
  </a>
</div>
