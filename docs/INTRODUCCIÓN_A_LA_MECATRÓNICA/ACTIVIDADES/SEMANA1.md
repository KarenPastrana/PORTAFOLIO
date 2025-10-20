# Actividad 1: Creación del Repositorio del Portafolio en GitHub 

## Descripción General
Este repositorio es una bitácora digital creada en GitHub Pages utilizando MkDocs con el tema Material Design, lo que permite presentar la información de manera ordenada, visual y fácil de navegar. Su propósito es ofrecer un sitio atractivo, funcional y bien estructurado para documentar todas las actividades y proyectos del curso de **Introducción a la Mecatrónica** y **Proyectos de Ingeniería**.

---

## 1. Creación del Repositorio
1. Se ingresó a la cuenta de GitHub y se creó un nuevo repositorio llamado **“Portafolio”**.  
2. Dentro del repositorio, se presionó la tecla **`.` (punto)** para abrir el **editor en línea tipo Visual Studio Code**, lo que permitió editar archivos directamente desde el navegador.  
3. Desde este entorno se crearon nuevas carpetas y archivos del proyecto usando las opciones:
    - **Nuevo archivo:** 
    - **Nueva carpeta:**
4. Los archivos principales se guardaron con formato **Markdown (.md)**, ya que MkDocs utiliza este tipo de archivos para generar las páginas del sitio.
 

---

## 2. Estructura de Carpetas
La organización del proyecto se realizó dentro de una carpeta principal llamada `docs/`, que contiene todo el contenido visible del sitio.  

La estructura quedó de la siguiente forma:

```
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
```

Cada carpeta tiene un propósito específico:

- `assets/` → almacena imágenes, videos y archivos usados en las páginas.  
- `Introducción a la Mecatrónica/` → contiene las actividades de esa materia.  
- `Proyectos de Ingeniería/` → contiene las actividades de esa materia.  
- `index.md` → es la página principal del portafolio.
 

---

## 3. Archivo Principal `mkdocs.yml`
El archivo `mkdocs.yml` define la configuración general del sitio web. Aquí se configuró el **nombre del sitio**, los **colores**, las **extensiones de Markdown** y el **tema visual**.

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

El color del **banner (encabezado)** se cambió modificando el valor de `primary`. 
Se puede reemplazar por cualquier color como `blue`, `red`, `green`, o incluso un código hexadecimal (por ejemplo `#4b0082`).

---

##  4. Edición del Contenido en Markdown
Cada página del sitio se escribió en formato **Markdown (.md)**, lo cual permite usar títulos, subtítulos, listas, texto en negrita o cursiva, imágenes y videos.

###  1. Títulos y Subtítulos
Para definir la jerarquía de los títulos:

```markdown
# Título principal
## Subtítulo
### Sub-subtítulo
```

### 2. Estilos de Fuente
Cómo resaltar el texto:

```
**Negrita**
***Negrita y cursiva***
_Cursiva_
~~Tachado~~
```

###  3. Listas y Numeraciones
Listas numeradas:

```
1. Primer paso
2. Segundo paso
3. Tercer paso
```

Listas con viñetas:
```
- Elemento de lista
* Otro elemento
+ Otro elemento
```


###  4. Imágenes y Videos (HTML)

Para insertar una imagen:
```
<img src="ruta/de/tu/imagen.jpg" alt="Descripción" width="320">
```

 **Notas:**

- `src`: reemplaza `"ruta/de/tu/imagen.jpg"` por la ubicación real de tu archivo.  
- `alt`: texto que describe la imagen, útil para accesibilidad.  
- `width`: ajusta el ancho de la imagen según necesites.




Para insertar un video:
```
<video width="400" controls>
  <source src="ruta/de/tu/video.mp4" type="video/mp4">
</video>
```

 **Notas:**

- `src`: reemplaza `"ruta/de/tu/video.mp4"` por la ubicación real de tu video.  
- `width`: ajusta el ancho del video según tu diseño.  
- `controls`: agrega los controles de reproducción (play, pausa, volumen).  


Puedes agregar atributos para modificar el comportamiento del video:

```
<video width="500" controls autoplay loop muted>
  <source src="ruta/de/tu/video.mp4" type="video/mp4">
  Tu navegador no soporta video.
</video>
```

**Significado de los atributos:**

- `autoplay` → el video se reproduce automáticamente al cargar la página.  
- `loop` → el video se repite en bucle una vez que termina.  
- `muted` → inicia el video en silencio (recomendado si usas autoplay, ya que algunos navegadores no reproducen videos con sonido automáticamente).  
- Puedes combinarlos según necesites: `<video width="500" controls autoplay loop>`.  


---

##  5. Diseño y Personalización
En los archivos `.md`, se mezcló **Markdown con HTML** para lograr un diseño más visual.
Por ejemplo, en el archivo `index.md` se agregó un contenedor centrado con títulos coloridos y botones con imágenes de fondo:

```html
<div align="center" style="background-color:#f0f0f0; padding: 30px; border-radius: 15px;">
  <h1 style="color:#4b0082;">PORTAFOLIO</h1>
  <p style="font-size:18px;">Bienvenido(a).</p>
  <img src="assets/imgs/ibero.jpeg" width="300" style="border-radius:15px;">
</div>
```

 **Notas:**

- `align="center"` → centra el contenido.
- `background-color` → define el color de fondo del contenedor.
- `padding` → separa el contenido del borde.
- `border-radius` → redondea las esquinas del contenedor o imagen.
- `width` → ajusta el tamaño de la imagen según tu diseño.
- Cambia `src` a la ruta de tu imagen real.

También se añadieron botones con enlaces para dirigir a las diferentes secciones:

```html 
<a href="https://usuario.github.io/PORTAFOLIO/INTRODUCCIÓN_A_LA_MECATRÓNICA/"
   style="display:inline-block; background-color:#4b0082; color:white; padding:15px 25px; border-radius:10px;">
   Ir a Introducción a la Mecatrónica
</a>
```

 **Notas:**

- `href` → reemplaza con la URL real de tu sección.
- `display:inline-block` → permite que el botón tenga tamaño definido y se alinee correctamente.
- `background-color y color` → definen colores del botón y texto.
- `padding` → ajusta el espacio interno del botón.
- `border-radius` → redondea las esquinas del botón.

---

##  6. Publicación del Sitio en GitHub Pages

1. Se guardaron todos los archivos y se realizó un **commit** con los cambios.
2. En el repositorio de GitHub, se fue a Settings → Pages.
3. En Source, se seleccionó la rama `main` y la carpeta `/docs`.
4. GitHub generó automáticamente la página en línea con la URL:
`https://usuario.github.io/Portafolio/`

Una vez publicado, cada modificación subida al repositorio se actualiza automáticamente en el sitio.

---

##  7. Resultado Final
El resultado fue un portafolio web completo y personalizable, con una estructura clara y visualmente atractiva. El sitio permite acceder fácilmente a las actividades semanales y proyectos, incluyendo imágenes, videos, botones y estilos personalizados.

Gracias a MkDocs y GitHub Pages, el proceso de publicación es sencillo, y la apariencia del sitio puede modificarse libremente desde el archivo `mkdocs.yml` sin necesidad de programación avanzada.

---

##  Conclusión General

El desarrollo del repositorio permitió comprender el funcionamiento de GitHub Pages y MkDocs, la estructura de carpetas y archivos, así como la personalización de temas, colores y estilos mediante el uso de Markdown y HTML. Este portafolio se convierte así en una herramienta práctica y visual para documentar el avance dentro del curso.




---



