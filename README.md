<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>SEMBRANDO VIDA - Planes de Trabajo</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://cdn.sheetjs.com/xlsx-latest/package/dist/xlsx.full.min.js"></script>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: url('Shade-Tree.jpg') no-repeat center center fixed;
      background-size: cover;
      padding: 40px;
      color: #333;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    h1 {
      color: white;
      text-align: center;
      font-size: 2.2em;
      background-color: #2e7d32;
      padding: 15px 25px;
      border-radius: 12px;
      box-shadow: 0 0 8px rgba(0,0,0,0.5);
      max-width: 800px;
      width: 100%;
      margin-bottom: 25px;
    }

    .container {
      background: rgba(255, 255, 255, 0.97);
      padding: 25px;
      border-radius: 10px;
      max-width: 800px;
      width: 100%;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
      margin-bottom: 25px;
    }

    form {
      display: flex;
      flex-direction: column;
    }

    label {
      font-weight: bold;
      margin-top: 12px;
    }

    input, select, textarea {
      padding: 10px;
      margin-top: 5px;
      border-radius: 6px;
      border: 1px solid #ccc;
      font-size: 1em;
      width: 100%;
      box-sizing: border-box;
    }

    button {
      background-color: #388e3c;
      color: white;
      padding: 12px;
      border: none;
      border-radius: 6px;
      margin-top: 20px;
      cursor: pointer;
      font-size: 1em;
      transition: background-color 0.3s ease;
      width: 100%;
    }

    button:hover {
      background-color: #2e7d32;
    }

    .btn-export {
      font-weight: bold;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
      overflow-x: auto;
      display: block;
    }

    th, td {
      border: 1px solid #ccc;
      padding: 10px;
      text-align: left;
      background: white;
      min-width: 100px;
    }

    th {
      background-color: #a5d6a7;
      font-weight: bold;
    }

    .btn-delete, .btn-edit {
      padding: 6px 10px;
      border-radius: 4px;
      color: white;
      border: none;
      cursor: pointer;
      margin-right: 5px;
      font-size: 0.9em;
    }

    .btn-delete {
      background-color: #d32f2f;
    }

    .btn-delete:hover {
      background-color: #b71c1c;
    }

    .btn-edit {
      background-color: #1976d2;
    }

    .btn-edit:hover {
      background-color: #1565c0;
    }

    /* 🔹 Adaptación para celulares y tablets */
    @media (max-width: 768px) {
      body {
        padding: 15px;
      }

      h1 {
        font-size: 1.5em;
        padding: 10px;
      }

      .container {
        padding: 15px;
      }

      label {
        font-size: 0.95em;
      }

      input, select, textarea, button {
        font-size: 0.95em;
      }

      th, td {
        font-size: 0.85em;
        padding: 8px;
      }

      /* Permitir scroll horizontal si la tabla es muy grande */
      table {
        display: block;
        overflow-x: auto;
        white-space: nowrap;
      }

      .btn-delete, .btn-edit {
        font-size: 0.8em;
        padding: 5px 8px;
      }
    }

    @media (max-width: 480px) {
      h1 {
        font-size: 1.3em;
      }

      button {
        font-size: 0.9em;
      }
    }
  </style>
</head>
<body>

  <h1>SEMBRANDO VIDA – PLANES DE TRABAJO</h1>

  <div class="container">
    <form id="planForm">
      <label>Municipio:</label>
      <input type="text" id="municipio" required>

      <label>Facilitador:</label>
      <input type="text" id="facilitador" required>

      <label>Comunidad:</label>
      <select id="comunidad" required>
        <option value="">Selecciona una comunidad</option>
        <option value="JUAN ALDAMA">JUAN ALDAMA</option>
        <option value="VICENTE GUERRERO">VICENTE GUERRERO</option>
        <option value="OFICINA">OFICINA</option>
        <option value="ALLENDE 1RA">ALLENDE 1RA</option>
        <option value="ALLENDE 2DA">ALLENDE 2DA</option>
      </select>

      <label>Técnico:</label>
      <select id="tecnico" required>
        <option value="">Selecciona un técnico</option>
        <option value="DAVID ARMANDO LANDERO DÍAZ">DAVID ARMANDO LANDERO DÍAZ</option>
        <option value="YANET JIMÉNEZ HERNÁNDEZ">YANET JIMÉNEZ HERNÁNDEZ</option>
        <option value="JORGE ABRAHAM VELAZQUEZ VICENTE">JORGE ABRAHAM VELAZQUEZ VICENTE</option>
        <option value="JESSICA DEL CARMEN">JESSICA DEL CARMEN</option>
        <option value="JORGE ROMERO">JORGE ROMERO</option>
        <option value="SELENA">SELENA</option>
      </select>

      <label>Fecha:</label>
      <input type="date" id="fecha" required>

      <label>Hora:</label>
      <input type="time" id="hora" required>

      <label>Descripción:</label>
      <textarea id="descripcion" rows="3" required></textarea>

      <button type="submit" id="btnAgregar">Agregar Plan</button>
    </form>
  </div>

  <div class="container">
    <table id="tablaPlanes">
      <thead>
        <tr>
          <th>Municipio</th>
          <th>Facilitador</th>
          <th>Comunidad</th>
          <th>Técnico</th>
          <th>Fecha</th>
          <th>Hora</th>
          <th>Descripción</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>

  <div class="container">
    <button class="btn-export" onclick="exportarExcel()">Exportar a Excel</button>
  </div>

  <script>
    const form = document.getElementById('planForm');
    const tabla = document.querySelector('#tablaPlanes tbody');
    const btnAgregar = document.getElementById('btnAgregar');
    const planes = [];
    let editIndex = -1;

    form.addEventListener('submit', function(e) {
      e.preventDefault();

      const plan = {
        Municipio: document.getElementById('municipio').value,
        Facilitador: document.getElementById('facilitador').value,
        Comunidad: document.getElementById('comunidad').value,
        Técnico: document.getElementById('tecnico').value,
        Fecha: document.getElementById('fecha').value,
        Hora: document.getElementById('hora').value,
        Descripción: document.getElementById('descripcion').value
      };

      if (editIndex === -1) {
        planes.push(plan);
        agregarFilaTabla(plan);
      } else {
        planes[editIndex] = plan;
        actualizarTabla();
        btnAgregar.textContent = "Agregar Plan";
        editIndex = -1;
      }

      form.reset();
    });

    function agregarFilaTabla(plan) {
      const fila = tabla.insertRow();

      fila.insertCell().textContent = plan.Municipio;
      fila.insertCell().textContent = plan.Facilitador;
      fila.insertCell().textContent = plan.Comunidad;
      fila.insertCell().textContent = plan.Técnico;
      fila.insertCell().textContent = plan.Fecha;
      fila.insertCell().textContent = plan.Hora;
      fila.insertCell().textContent = plan.Descripción;

      const celdaAcciones = fila.insertCell();

      const btnEditar = document.createElement('button');
      btnEditar.textContent = 'Editar';
      btnEditar.className = 'btn-edit';
      btnEditar.onclick = function () {
        editIndex = fila.rowIndex - 1;
        const plan = planes[editIndex];
        document.getElementById('municipio').value = plan.Municipio;
        document.getElementById('facilitador').value = plan.Facilitador;
        document.getElementById('comunidad').value = plan.Comunidad;
        document.getElementById('tecnico').value = plan.Técnico;
        document.getElementById('fecha').value = plan.Fecha;
        document.getElementById('hora').value = plan.Hora;
        document.getElementById('descripcion').value = plan.Descripción;
        btnAgregar.textContent = "Actualizar Plan";
        window.scrollTo({ top: 0, behavior: 'smooth' });
      };

      const btnEliminar = document.createElement('button');
      btnEliminar.textContent = 'Eliminar';
      btnEliminar.className = 'btn-delete';
      btnEliminar.onclick = function () {
        const index = fila.rowIndex - 1;
        planes.splice(index, 1);
        actualizarTabla();
      };

      celdaAcciones.appendChild(btnEditar);
      celdaAcciones.appendChild(btnEliminar);
    }

    function actualizarTabla() {
      tabla.innerHTML = "";
      planes.forEach(plan => agregarFilaTabla(plan));
    }

    function exportarExcel() {
      if (planes.length === 0) {
        alert("No hay planes para exportar.");
        return;
      }
      const wb = XLSX.utils.book_new();
      const ws = XLSX.utils.json_to_sheet(planes, {
        header: ["Municipio", "Facilitador", "Comunidad", "Técnico", "Fecha", "Hora", "Descripción"],
        origin: "A2" // ✅ Empieza en la fila 2
      });
      XLSX.utils.book_append_sheet(wb, ws, "Planes");
      XLSX.writeFile(wb, "planes_de_trabajo.xlsx");
    }
  </script>

</body>
</html>
