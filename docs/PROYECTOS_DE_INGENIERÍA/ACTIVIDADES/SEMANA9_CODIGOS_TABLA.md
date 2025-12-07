# SEMANA 9 CÓDIGOS DE TABLA PARA CORTAR EN ROUTER

## Karen

Para la elaboración de los códigos para cortar una tabla con mi apellido primero hice el diseño en SolidWorks.

<div align="center">
  <img src="../../assets/imgs/Tabla_Karen.jpg" alt="Karen" width="500">
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/Tabla_Karen.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-SLDPRT-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar SLDPRT">
  </a>
</div>

Posteriormente, utilicé el software VCarve. El proceso inició con la importación de mi diseño y la definición del tamaño del material, configurando las dimensiones de la madera.

<div align="center">
  <img src="../../assets/imgs/Tabla_Karen.jpeg" alt="Karen" width="500">
</div>

Después seleccioné las herramientas adecuadas según el tipo de operación. Para las áreas donde necesitaba cajear o retirar material, así como para los cortes exteriores e interiores, utilicé una punta plana. En cambio, para grabar las letras de mi apellido, utilicé una punta en V.

En VCarve configuré la profundidad de corte, la velocidad de avance y el tipo de estrategia (pocketing para el cajeado y profile para los cortes).

Finalmente, generé los códigos G-code correspondientes a cada herramienta, asegurándome de guardar las trayectorias por separado para evitar errores al momento del corte. Con esto obtuve los archivos listos para la CNC.



<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/Tabla_Karen.DXF" download>
    <img src="https://img.shields.io/badge/Descargar-DXF-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar DXF">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/Tabla_Karen.crv" download>
    <img src="https://img.shields.io/badge/Descargar-crv-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar crv">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/Plano_Tabla_Karen.nc" download>
    <img src="https://img.shields.io/badge/Descargar-nc-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar Plano.nc">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/CorteV_Tabla_Karen.nc" download>
    <img src="https://img.shields.io/badge/Descargar-nc-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar CorteV.nc">
  </a>
</div>

---

## Tabla Samantha
Para fabricar la tabla de madera, primero definí las dimensiones finales y la frase que iba a llevar grabada. Con esos elementos listos, preparé el diseño digital en software CAD, asegurando que todas las medidas estuvieran correctamente establecidas para que el mecanizado fuera preciso.

<div align="center">
  <img src="../../assets/imgs/TABLASOLIDSSAM.png" alt="Sam" width="500">
</div>

### 1. Preparación del diseño en VCarve
Una vez terminado el diseño, lo importé a VCarve, donde configuré cada operación de maquinado según el tipo de acabado que necesitaba en la madera. Dentro del software definí:

- **Trayectoria de grabado (V-Carve / Engraving):** Para la frase 
- **Operación de cajeado (Pocket Toolpath):** Para remover material de zonas interiores y generar cavidades limpias y uniformes.
- **Corte perimetral (Profile Cut):** Para desprender la pieza principal de la tabla y cortar el circulo superior.

<div align="center">
  <img src="../../assets/imgs/CAJEADO.png" alt="Sam" width="500">
</div>
  
Con todas las trayectorias configuradas, generé los códigos G correspondientes. 

### 2. Preparación en el Router CNC
Transferí el archivo al router CNC y fijé la tabla de madera a la cama de la máquina para evitar desplazamientos. Luego coloqué la herramienta requerida en el spindle y realicé el cero en los ejes X, Y y Z para que la máquina tomara la referencia correcta.

### 3. Ejecución del corte

Con todo configurado, inicié el programa. El router CNC ejecutó de forma automática las distintas fases:
1. Grabado de la frase, siguiendo las líneas del diseño.
2. Cajeado de las áreas internas, removiendo material de forma controlada.
3. Corte del hueco central y corte final del contorno para desprender la pieza.

Al finalizar, retiré la tabla y realicé un lijado ligero para eliminar rebabas y dejar la superficie lista para acabado.

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/Tabla.SLDPRT" download>
    <img src="https://img.shields.io/badge/Descargar-stl-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar CorteV.nc">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/TablaMadera.DXF" download>
    <img src="https://img.shields.io/badge/Descargar-dxf-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar CorteV.nc">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/Tablamadera.crv" download>
    <img src="https://img.shields.io/badge/Descargar-crv-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar CorteV.nc">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/TablaSam.txt" download>
    <img src="https://img.shields.io/badge/Descargar-txt-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar CorteV.nc">
  </a>
</div>

<!-- Botón de descarga -->
<div align="center">
  <a href="../../assets/archivos/TablaSamV.txt" download>
    <img src="https://img.shields.io/badge/Descargar-txt-red?style=for-the-badge&logo=adobeacrobatreader" alt="Descargar CorteV.nc">
  </a>
</div>
