# Actividad 1: 🧭 Creación del Repositorio del Portafolio en GitHub 

## 📌 Descripción General
Este repositorio corresponde a una bitácora digital creada en GitHub Pages utilizando MkDocs con el tema Material Design que permite presentar la información de forma ordenada, visual y fácil de navegar. El propósito fue desarrollar un sitio organizado, atractivo y funcional para documentar todas las actividades y proyectos del curso de **Introducción a la Mecatrónica** y **Proyectos de Ingeniería**.

---

## ⚙️ 1. Creación del Repositorio
1. Se ingresó a la cuenta de GitHub y se creó un nuevo repositorio llamado **“Portafolio”**.  
2. Dentro del repositorio, se presionó la tecla **`.` (punto)** para abrir el **editor en línea tipo Visual Studio Code**, lo que permitió editar archivos directamente desde el navegador.  
3. Desde este entorno se crearon nuevas carpetas y archivos del proyecto usando las opciones:
   - **Nuevo archivo:** `Archivo > Nuevo archivo` o clic derecho en el panel lateral.  
   - **Nueva carpeta:** clic derecho > “New Folder”.  
4. Los archivos principales se guardaron con formato **Markdown (.md)**, ya que MkDocs utiliza este tipo de archivos para generar las páginas del sitio.  

---

## 🗂️ 2. Estructura de Carpetas
La organización del proyecto se realizó dentro de una carpeta principal llamada `docs/`, que contiene todo el contenido visible del sitio.  
La estructura quedó de la siguiente forma:

📦 Portafolio/
┣ 📂 .github/
┣ 📂 docs/
┃ ┣ 📂 INTRODUCCIÓN_A_LA_MECATRÓNICA/
┃ ┃ ┣ 📂 ACTIVIDADES/
┃ ┃ ┣ 📂 assets/
┃ ┃ ┣ 📜 1_INTRO.md
┃ ┃ ┣ 📜 2_ABOUT.md
┃ ┃ ┗ 📜 PROYECTO.md
┃ ┣ 📂 PROYECTOS_DE_INGENIERÍA/
┃ ┃ ┣ 📂 ACTIVIDADES/
┃ ┃ ┣ 📂 assets/
┃ ┃ ┣ 📜 1_PROYECTOS.md
┃ ┃ ┣ 📜 2_ABOUT.md
┃ ┃ ┗ 📜 PROYECTO.md
┃ ┣ 📂 SOPORTE/
┃ ┗ 📂 assets/
┃ ┗ 📜 index.md
┣ 📜 .gitignore
┣ 📜 LICENSE
┣ 📜 README.md
┣ 📜 mkdocs.yml
┗ 📜 requirements.txt


Cada carpeta tiene un propósito específico:  
- **assets/** → almacena imágenes y videos usados en las páginas.  
- **Introducción a la Mecatrónica/** → contiene las actividades/proyecto.  
- **Proyectos de Ingeniería/** → contiene las actividades/proyecto.
- **index.md** → es la página principal del portafolio.  

---

## 🧱 3. Archivo Principal `mkdocs.yml`
El archivo `mkdocs.yml` define la configuración general del sitio web.  
Aquí se configuró el **nombre del sitio**, los **colores**, las **extensiones de Markdown** y el **tema visual**.

Ejemplo del código usado:
```yaml
site_name: Portafolio
markdown_extensions:
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.inlinehilite
  - pymdownx.superfences
  - admonition
  - attr_list
  - def_list
theme:
  name: material
  highlightjs: true 
  palette:
      - scheme: default
        primary: purple
```

💡 El color del banner (encabezado) se cambió modificando el valor de `primary`. 
Se puede reemplazar por cualquier color como `blue`, `teal`, `red`, `green`, o incluso un código hexadecimal (por ejemplo `#4b0082`).


## ✍️ 4. Edición del Contenido en Markdown
Cada página del sitio se escribió en formato **Markdown (.md)**, lo cual permite usar títulos, subtítulos, listas, texto en negrita o cursiva, imágenes y videos.
🔹 **Títulos y Subtítulos**
# Título principal
## Subtítulo
### Sub-subtítulo

🔹 **Estilos de Fuente**
**Negrita**
*Cursiva*
***Negrita y cursiva***

🔹 **Listas y Numeraciones**
1. Primer paso
2. Segundo paso
3. Tercer paso

- Elemento de lista
- Otro elemento

🔹 **Imágenes y Videos**
Para insertar una imagen:
![Descripción](assets/imgs/imagen.jpg)

Para insertar un video (enlace o local):
<video width="400" controls>
  <source src="../assets/videos/demostracion.mp4" type="video/mp4">
</video>

## 🖥️ 5. Diseño y Personalización
En los archivos `.md`, se mezcló **Markdown con HTML** para lograr un diseño más visual.
Por ejemplo, en el archivo `index.md` se agregó un contenedor centrado con títulos coloridos y botones con imágenes de fondo:

<div align="center" style="background-color:#f0f0f0; padding: 30px; border-radius: 15px;">
  <h1 style="color:#4b0082;">PORTAFOLIO</h1>
  <p style="font-size:18px;">Bienvenido(a).</p>
  <img src="assets/imgs/ibero.jpeg" width="300" style="border-radius:15px;">
</div>

También se añadieron botones con enlaces para dirigir a las diferentes secciones:

<a href="https://usuario.github.io/PORTAFOLIO/INTRODUCCIÓN_A_LA_MECATRÓNICA/"
   style="display:inline-block; background-color:#4b0082; color:white; padding:15px 25px; border-radius:10px;">
   Ir a Introducción a la Mecatrónica
</a>

## 🌐 6. Publicación del Sitio en GitHub Pages

1. Se guardaron todos los archivos y se realizó un commit con los cambios.
2. En el repositorio de GitHub, se fue a Settings → Pages.
3. En Source, se seleccionó la rama `main` y la carpeta `/docs`.
4. GitHub generó automáticamente la página en línea con la URL:
`https://usuario.github.io/Portafolio/`

Una vez publicado, cada modificación subida al repositorio se actualiza automáticamente en el sitio.

## 🧾 7. Resultado Final
El resultado fue un portafolio web completo y personalizable, con una estructura clara y visualmente atractiva. El sitio permite acceder fácilmente a las actividades semanales, reportes y proyectos, incluyendo imágenes, videos, botones y estilos personalizados.

Gracias a MkDocs y GitHub Pages, el proceso de publicación es sencillo, y la apariencia del sitio puede modificarse libremente desde el archivo `mkdocs.yml` sin necesidad de programación avanzada.

## 🪶 Conclusión General

El desarrollo del repositorio permitió comprender el funcionamiento de GitHub Pages y MkDocs, la estructura de carpetas y archivos, así como la personalización de temas, colores y estilos mediante el uso de Markdown y HTML. Este portafolio se convierte así en una herramienta práctica y visual para documentar el avance dentro del curso y los proyectos de ingeniería.




