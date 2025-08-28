<style>
/* Reset */
body {
  margin: 0;
  font-family: Arial, sans-serif;
}

/* Sidebar */
.sidebar {
  width: 250px;
  background-color: #f5f5f5;
  border-right: 1px solid #ccc;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  padding: 20px;
  box-sizing: border-box;
  overflow-y: auto;
  transform: translateX(-100%);
  transition: transform 0.3s ease;
  z-index: 999;
}

.sidebar h2 {
  color: #4b0082;
  text-align: center;
  margin-bottom: 20px;
}

.sidebar a {
  display: block;
  padding: 12px 15px;
  margin: 8px 0;
  background: #e0e0e0;
  color: #000;
  text-decoration: none;
  border-radius: 8px;
  font-weight: bold;
  transition: 0.3s;
}

.sidebar a:hover {
  background: #4b0082;
  color: white;
}

/* Botón toggle (visible siempre) */
.toggle-btn {
  position: fixed;
  top: 15px;
  left: 15px;
  background: #4b0082;
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 20px;
  z-index: 1000;
}

/* Sidebar activo */
.sidebar.active {
  transform: translateX(0);
}

/* Contenido */
.content {
  margin-left: 0;
  padding: 30px;
}

.section {
  background-color: #f0f0f0;
  padding: 30px;
  border-radius: 15px;
  margin-bottom: 40px;
  text-align: center;
}

.section img {
  border-radius: 15px;
  margin: 20px 0;
}
</style>

<!-- Botón toggle -->
<button class="toggle-btn" onclick="toggleSidebar()">☰</button>

<!-- Sidebar -->
<div class="sidebar" id="sidebar">
  <h2>📂 Índice</h2>
  <a href="#portada" onclick="toggleSidebar()">Portafolio</a>
  <a href="#mecatronica" onclick="toggleSidebar()">Introducción a la Mecatrónica</a>
  <a href="#proyectos" onclick="toggleSidebar()">Proyectos de Ingeniería</a>
</div>

<!-- Contenido -->
<div class="content">

  <!-- Portada -->
  <div id="portada" class="section">
    <h1 style="color:#4b0082;">Portafolio</h1>
    <p style="font-size:18px;">Bienvenido(a).</p>
    <img src="assets/imgs/ibero.jpeg" alt="Portada" width="300">
  </div>

  <!-- Introducción a la Mecatrónica -->
  <div id="mecatronica" class="section">
    <h2 style="color:#228b22;">Introducción a la Mecatrónica</h2>
    <a href="docs/Introducción a la Mecatrónica/Introducción a la Mecatrónica.md"
       style="display:inline-block; background-image:url('assets/imgs/F1.jpg'); background-size:cover; color:white; text-decoration:none; padding:25px 40px; border-radius:15px; font-weight:bold; margin:10px;">
       Introducción a la Mecatrónica
    </a>
  </div>

  <!-- Proyectos de Ingeniería -->
  <div id="proyectos" class="section">
    <h2 style="color:#1e90ff;">Proyectos de Ingeniería</h2>
    <a href="Proyectos de Ingeniería"
       style="display:inline-block; background-image:url('assets/imgs/F2.jpg'); background-size:cover; color:white; text-decoration:none; padding:25px 40px; border-radius:15px; font-weight:bold; margin:10px;">
       Proyectos de Ingeniería
    </a>
  </div>

</div>

<script>
function toggleSidebar() {
  document.getElementById("sidebar").classList.toggle("active");
}
</script>
