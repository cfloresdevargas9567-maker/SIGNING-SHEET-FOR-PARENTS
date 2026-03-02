<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Signing Sheet</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
  body {
    font-family: Arial, sans-serif;
    background: #f5f5f5;
    padding: 20px;
  }
  h1 {
    text-align: center;
  }
  .box {
    background: white;
    padding: 15px;
    border-radius: 10px;
    max-width: 500px;
    margin: auto;
  }
  label {
    font-weight: bold;
  }
  canvas {
    border: 1px solid #000;
    width: 100%;
    height: 150px;
  }
  button {
    margin-top: 10px;
    padding: 10px;
    width: 100%;
    font-size: 16px;
  }
</style>
</head>

<body>
<div class="box">
  <h1>Signing Sheet</h1>

  <p><strong>Familia:</strong> <span id="familia">General</span></p>

  <label>Nombre del niño:</label><br>
  <input type="text" style="width:100%;"><br><br>

  <label>Firma:</label>
  <canvas id="firma"></canvas>

  <button onclick="guardar()">Guardar Firma</button>
  <button onclick="limpiar()">Limpiar Firma</button>
</div>

<script>
  // Leer apellido desde el link
  const params = new URLSearchParams(window.location.search);
  const familia = params.get("familia") || "General";
  document.getElementById("familia").textContent = familia;

  // Firma
  const canvas = document.getElementById("firma");
  const ctx = canvas.getContext("2d");
  let dibujando = false;

  function resizeCanvas() {
    canvas.width = canvas.offsetWidth;
    canvas.height = canvas.offsetHeight;
  }
  resizeCanvas();

  canvas.addEventListener("mousedown", e => {
    dibujando = true;
    ctx.beginPath();
    ctx.moveTo(e.offsetX, e.offsetY);
  });

  canvas.addEventListener("mousemove", e => {
    if (!dibujando) return;
    ctx.lineTo(e.offsetX, e.offsetY);
    ctx.stroke();
  });

  canvas.addEventListener("mouseup", () => dibujando = false);
  canvas.addEventListener("touchstart", e => {
    dibujando = true;
    const t = e.touches[0];
    ctx.beginPath();
    ctx.moveTo(t.clientX - canvas.offsetLeft, t.clientY - canvas.offsetTop);
  });

  canvas.addEventListener("touchmove", e => {
    if (!dibujando) return;
    const t = e.touches[0];
    ctx.lineTo(t.clientX - canvas.offsetLeft, t.clientY - canvas.offsetTop);
    ctx.stroke();
  });

  canvas.addEventListener("touchend", () => dibujando = false);

  function limpiar() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  }

  function guardar() {
    const data = canvas.toDataURL();
    const a = document.createElement("a");
    a.href = data;
    a.download = `firma_${familia}_${Date.now()}.png`;
    a.click();
  }
</script>
</body>
</html>

https://docs.google.com/forms/d/e/1FAIpQLScqCQK4eiuLrH8c2j5LE8UIsQO6hRbclBQwVO_8OS2nwCFRDQ/viewform?usp=share_link&ouid=102264524862103777919
