<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Gestión Contable Avanzada - Normativas Mexicanas</title>
  <style>
    /* Estilos Modernos */
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background-color: #f4f7f6;
      margin: 0;
      padding: 20px;
      color: #333;
    }
    .container {
      max-width: 1000px;
      margin: 0 auto;
      background: white;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
    }
    h1 {
      text-align: center;
      color: #2c3e50;
      margin-bottom: 20px;
      font-size: 2.5em;
    }
    .upload-section {
      text-align: center;
      padding: 30px;
      border: 2px dashed #3498db;
      border-radius: 12px;
      background-color: #ecf5ff;
      cursor: pointer;
      transition: background-color 0.3s ease;
      margin-bottom: 20px;
    }
    .upload-section:hover {
      background-color: #d6e9ff;
    }
    .upload-section p {
      margin: 0;
      font-size: 1.2em;
      color: #3498db;
    }
    .status {
      margin-top: 20px;
      padding: 15px;
      border-radius: 8px;
      font-weight: bold;
      text-align: center;
      font-size: 1.1em;
    }
    .status.processing {
      background: #f1c40f;
      color: #fff;
    }
    .status.success {
      background: #2ecc71;
      color: #fff;
    }
    .status.error {
      background: #e74c3c;
      color: #fff;
    }
    .results {
      margin-top: 30px;
    }
    .card {
      background: #fff;
      padding: 20px;
      margin-bottom: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    .card:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    }
    .card h3 {
      margin-top: 0;
      color: #3498db;
      font-size: 1.5em;
    }
    .card p {
      margin: 10px 0;
      font-size: 1.1em;
    }
    .card .total {
      font-weight: bold;
      color: #27ae60;
    }
    .btn-pdf {
      display: inline-block;
      padding: 10px 20px;
      background-color: #3498db;
      color: #fff;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.3s ease;
      font-size: 1em;
      text-decoration: none;
      margin-top: 20px;
    }
    .btn-pdf:hover {
      background-color: #2980b9;
    }
  </style>
</head>
<body>

<div class="container">
  <h1>Gestión Contable Avanzada</h1>
  <div class="upload-section" onclick="document.getElementById('fileInput').click()">
    <p>📂 Arrastra o selecciona un archivo Excel o PDF</p>
    <input type="file" id="fileInput" style="display: none;" accept=".xlsx,.xls,.pdf">
  </div>
  <div id="statusMessage"></div>
  <div class="results" id="resultsContainer"></div>
  <!-- Botón para descargar informe PDF (se mostrará al procesar datos) -->
  <div id="pdfButtonContainer" style="text-align: center; display: none;">
    <button class="btn-pdf" onclick="generarInformePDF()">Descargar Informe PDF</button>
  </div>
</div>

<!-- Librerías -->
<!-- XLSX para Excel -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<!-- pdf.js para leer PDFs -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.14.305/pdf.min.js"></script>
<!-- jsPDF para generar PDFs -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

<script>
  // Variable global para almacenar los resultados y usarlos al generar el informe PDF
  let globalResults = null;

  /**********************
   * Cálculos de Impuestos
   **********************/

  // Función para calcular el ISR utilizando una tabla de rangos (ejemplo referencial)
  function calcularISR(ingresos) {
    // Tabla de rangos (valores referenciales; revisar según normativas vigentes)
    const brackets = [
      { limite: 416220.00, tasa: 0.15, cuotaFija: 0 },
      { limite: 624329.00, tasa: 0.20, cuotaFija: 62432.00 },
      { limite: 867123.00, tasa: 0.25, cuotaFija: 104864.00 },
      { limite: Infinity, tasa: 0.30, cuotaFija: 162864.00 }
    ];

    for (let bracket of brackets) {
      if (ingresos <= bracket.limite) {
        // Se aplica la cuota fija y la tasa sobre el excedente del límite inferior (se asume límite inferior del bracket anterior)
        // Para simplificar, usamos el cálculo directo
        return bracket.cuotaFija + (ingresos - (bracket.limite - (bracket.limite * bracket.tasa))) * bracket.tasa;
      }
    }
    return 0;
  }

  // Función para calcular un impuesto sobre nómina (por ejemplo, 3% del salario; este valor varía según entidad)
  function calcularImpuestoNomina(salario) {
    return salario * 0.03;
  }

  /**********************
   * Procesamiento de Datos
   **********************/

  // Función para procesar datos de Excel
  function procesarDatosExcel(datos) {
    let empresa = {};
    let cuentasBancarias = [];
    let facturas = [];
    let impuestos = { totalFacturado: 0, iva: 0, isr: 0 };
    let nomina = { salarios: [], totalSalarios: 0, retenciones: 0, provisiones: 0, impuestoNomina: 0 };
    let activosFijos = [];
    let cuentasPorPagar = [];
    let cuentasPorCobrar = [];
    let depreciacion = { anual: 0, acumulada: 0 };

    datos.forEach(row => {
      // Datos de la empresa
      if (row.EMPRESA) empresa = { nombre: row.EMPRESA, rfc: row.RFC, direccion: row.DIRECCION };

      // Cuentas bancarias
      if (row.BANCO && row.SALDO) {
        cuentasBancarias.push({ banco: row.BANCO, saldo: parseFloat(row.SALDO) });
      }

      // Facturas
      if (row.FACTURA && row.MONTO) {
        const monto = parseFloat(row.MONTO);
        facturas.push({ numero: row.FACTURA, monto: monto, fecha: row.FECHA });
        impuestos.totalFacturado += monto;
      }

      // Nómina
      if (row.EMPLEADO && row.SALARIO) {
        const salario = parseFloat(row.SALARIO);
        nomina.salarios.push({ empleado: row.EMPLEADO, salario });
        nomina.totalSalarios += salario;
        nomina.retenciones += salario * 0.10; // Retención del 10%
        nomina.provisiones += salario * 0.05; // Provisión del 5%
        nomina.impuestoNomina += calcularImpuestoNomina(salario);
      }

      // Activos fijos
      if (row.ACTIVO && row.VALOR) {
        const valor = parseFloat(row.VALOR);
        activosFijos.push({ activo: row.ACTIVO, valor });
        // Se asume depreciación anual fija del 10%
        depreciacion.anual += valor * 0.10;
        depreciacion.acumulada += valor * 0.10;
      }

      // Cuentas por cobrar
      if (row.CLIENTE && row.COBRAR) {
        cuentasPorCobrar.push({ cliente: row.CLIENTE, monto: parseFloat(row.COBRAR) });
      }

      // Cuentas por pagar
      if (row.PROVEEDOR && row.PAGAR) {
        cuentasPorPagar.push({ proveedor: row.PROVEEDOR, monto: parseFloat(row.PAGAR) });
      }
    });

    // Cálculos de impuestos
    impuestos.iva = impuestos.totalFacturado * 0.16; // IVA al 16%
    impuestos.isr = calcularISR(impuestos.totalFacturado);

    return {
      empresa,
      cuentasBancarias,
      facturas,
      impuestos,
      nomina,
      activosFijos,
      cuentasPorCobrar,
      cuentasPorPagar,
      depreciacion
    };
  }

  // Función para procesar datos extraídos del PDF (se asume un formato de líneas "clave: valor")
  function procesarDatosPDF(text) {
    let lines = text.split('\n');
    let empresa = {};
    let cuentasBancarias = [];
    let facturas = [];
    let impuestos = { totalFacturado: 0, iva: 0, isr: 0 };
    let nomina = { salarios: [], totalSalarios: 0, retenciones: 0, provisiones: 0, impuestoNomina: 0 };
    let activosFijos = [];
    let cuentasPorPagar = [];
    let cuentasPorCobrar = [];
    let depreciacion = { anual: 0, acumulada: 0 };

    lines.forEach(line => {
      line = line.trim();
      if (!line) return;
      // Empresa
      if (line.startsWith("EMPRESA:")) {
        empresa.nombre = line.replace("EMPRESA:", "").trim();
      }
      if (line.startsWith("RFC:")) {
        empresa.rfc = line.replace("RFC:", "").trim();
      }
      if (line.startsWith("DIRECCION:")) {
        empresa.direccion = line.replace("DIRECCION:", "").trim();
      }
      // Cuentas bancarias (formato: BANCO: Nombre, SALDO: 1234.56)
      if (line.startsWith("BANCO:")) {
        let bancoMatch = line.match(/BANCO:\s*([^,]+),\s*SALDO:\s*([\d\.]+)/i);
        if (bancoMatch) {
          cuentasBancarias.push({ banco: bancoMatch[1].trim(), saldo: parseFloat(bancoMatch[2]) });
        }
      }
      // Facturas (formato: FACTURA: 001, MONTO: 1500.00, FECHA: 2025-01-15)
      if (line.startsWith("FACTURA:")) {
        let facturaMatch = line.match(/FACTURA:\s*([^,]+),\s*MONTO:\s*([\d\.]+),\s*FECHA:\s*(.+)/i);
        if (facturaMatch) {
          let monto = parseFloat(facturaMatch[2]);
          facturas.push({ numero: facturaMatch[1].trim(), monto: monto, fecha: facturaMatch[3].trim() });
          impuestos.totalFacturado += monto;
        }
      }
      // Nómina (formato: EMPLEADO: Nombre, SALARIO: 5000.00)
      if (line.startsWith("EMPLEADO:")) {
        let nominaMatch = line.match(/EMPLEADO:\s*([^,]+),\s*SALARIO:\s*([\d\.]+)/i);
        if (nominaMatch) {
          let salario = parseFloat(nominaMatch[2]);
          nomina.salarios.push({ empleado: nominaMatch[1].trim(), salario });
          nomina.totalSalarios += salario;
          nomina.retenciones += salario * 0.10;
          nomina.provisiones += salario * 0.05;
          nomina.impuestoNomina += calcularImpuestoNomina(salario);
        }
      }
      // Activos fijos (formato: ACTIVO: Nombre, VALOR: 8000.00)
      if (line.startsWith("ACTIVO:")) {
        let activoMatch = line.match(/ACTIVO:\s*([^,]+),\s*VALOR:\s*([\d\.]+)/i);
        if (activoMatch) {
          let valor = parseFloat(activoMatch[2]);
          activosFijos.push({ activo: activoMatch[1].trim(), valor });
          depreciacion.anual += valor * 0.10;
          depreciacion.acumulada += valor * 0.10;
        }
      }
      // Cuentas por cobrar (formato: CLIENTE: Nombre, COBRAR: 2500.00)
      if (line.startsWith("CLIENTE:")) {
        let clienteMatch = line.match(/CLIENTE:\s*([^,]+),\s*COBRAR:\s*([\d\.]+)/i);
        if (clienteMatch) {
          cuentasPorCobrar.push({ cliente: clienteMatch[1].trim(), monto: parseFloat(clienteMatch[2]) });
        }
      }
      // Cuentas por pagar (formato: PROVEEDOR: Nombre, PAGAR: 1200.00)
      if (line.startsWith("PROVEEDOR:")) {
        let proveedorMatch = line.match(/PROVEEDOR:\s*([^,]+),\s*PAGAR:\s*([\d\.]+)/i);
        if (proveedorMatch) {
          cuentasPorPagar.push({ proveedor: proveedorMatch[1].trim(), monto: parseFloat(proveedorMatch[2]) });
        }
      }
    });

    impuestos.iva = impuestos.totalFacturado * 0.16;
    impuestos.isr = calcularISR(impuestos.totalFacturado);

    return {
      empresa,
      cuentasBancarias,
      facturas,
      impuestos,
      nomina,
      activosFijos,
      cuentasPorCobrar,
      cuentasPorPagar,
      depreciacion
    };
  }

  // Función para mostrar los resultados en la página (para Excel y PDF)
  function mostrarResultados(results) {
    globalResults = results; // Se guarda para usar en la generación del PDF
    document.getElementById('statusMessage').innerHTML = `<div class="status success">Archivo procesado correctamente.</div>`;
    document.getElementById('pdfButtonContainer').style.display = 'block';
    
    const container = document.getElementById('resultsContainer');
    container.innerHTML = `
      <div class="card">
        <h3>📌 Empresa</h3>
        <p>Nombre: ${results.empresa.nombre || 'N/A'}</p>
        <p>RFC: ${results.empresa.rfc || 'N/A'}</p>
        <p>Dirección: ${results.empresa.direccion || 'N/A'}</p>
      </div>
      <div class="card">
        <h3>🏦 Cuentas Bancarias</h3>
        ${results.cuentasBancarias.map(c => `<p>${c.banco}: $${c.saldo.toFixed(2)}</p>`).join('')}
        <p class="total">Total: $${results.cuentasBancarias.reduce((acc, c) => acc + c.saldo, 0).toFixed(2)}</p>
      </div>
      <div class="card">
        <h3>📑 Facturas</h3>
        ${results.facturas.map(f => `<p>#${f.numero} - $${f.monto.toFixed(2)} (${f.fecha})</p>`).join('')}
        <p class="total">Total Facturado: $${results.impuestos.totalFacturado.toFixed(2)}</p>
      </div>
      <div class="card">
        <h3>🧾 Impuestos</h3>
        <p>IVA (16%): $${results.impuestos.iva.toFixed(2)}</p>
        <p>ISR: $${results.impuestos.isr.toFixed(2)}</p>
      </div>
      <div class="card">
        <h3>💼 Nómina</h3>
        ${results.nomina.salarios.map(n => `<p>${n.empleado}: $${n.salario.toFixed(2)}</p>`).join('')}
        <p class="total">Total Salarios: $${results.nomina.totalSalarios.toFixed(2)}</p>
        <p>Retenciones (10%): $${results.nomina.retenciones.toFixed(2)}</p>
        <p>Provisiones (5%): $${results.nomina.provisiones.toFixed(2)}</p>
        <p>Impuesto Nómina (3%): $${results.nomina.impuestoNomina.toFixed(2)}</p>
      </div>
      <div class="card">
        <h3>🏢 Activos Fijos</h3>
        ${results.activosFijos.map(a => `<p>${a.activo}: $${a.valor.toFixed(2)}</p>`).join('')}
        <p class="total">Depreciación Anual (10%): $${results.depreciacion.anual.toFixed(2)}</p>
        <p>Depreciación Acumulada: $${results.depreciacion.acumulada.toFixed(2)}</p>
      </div>
      <div class="card">
        <h3>📈 Cuentas por Cobrar</h3>
        ${results.cuentasPorCobrar.map(c => `<p>${c.cliente}: $${c.monto.toFixed(2)}</p>`).join('')}
        <p class="total">Total por Cobrar: $${results.cuentasPorCobrar.reduce((acc, c) => acc + c.monto, 0).toFixed(2)}</p>
      </div>
      <div class="card">
        <h3>📉 Cuentas por Pagar</h3>
        ${results.cuentasPorPagar.map(p => `<p>${p.proveedor}: $${p.monto.toFixed(2)}</p>`).join('')}
        <p class="total">Total por Pagar: $${results.cuentasPorPagar.reduce((acc, p) => acc + p.monto, 0).toFixed(2)}</p>
      </div>
    `;
  }

  /**********************
   * Manejo de Archivos (Excel / PDF)
   **********************/
  document.getElementById('fileInput').addEventListener('change', async (event) => {
    const file = event.target.files[0];
    if (!file) return;
    
    document.getElementById('statusMessage').innerHTML = `<div class="status processing">Procesando archivo ${file.name}...</div>`;
    
    const extension = file.name.split('.').pop().toLowerCase();
    
    // Procesar Excel
    if (extension === "xlsx" || extension === "xls") {
      try {
        const reader = new FileReader();
        reader.onload = (e) => {
          const data = new Uint8Array(e.target.result);
          const workbook = XLSX.read(data, { type: 'array' });
          const sheet = workbook.Sheets[workbook.SheetNames[0]];
          const jsonData = XLSX.utils.sheet_to_json(sheet);
          const results = procesarDatosExcel(jsonData);
          mostrarResultados(results);
        };
        reader.readAsArrayBuffer(file);
      } catch (error) {
        document.getElementById('statusMessage').innerHTML = `<div class="status error">Error al procesar el archivo: ${error.message}</div>`;
      }
    }
    // Procesar PDF
    else if (extension === "pdf") {
      try {
        const reader = new FileReader();
        reader.onload = async (e) => {
          const arrayBuffer = e.target.result;
          const pdf = await pdfjsLib.getDocument({ data: arrayBuffer }).promise;
          const maxPages = pdf.numPages;
          let fullText = '';
          for (let i = 1; i <= maxPages; i++) {
            const page = await pdf.getPage(i);
            const textContent = await page.getTextContent();
            const pageText = textContent.items.map(item => item.str).join('\n');
            fullText += pageText + '\n';
          }
          const results = procesarDatosPDF(fullText);
          mostrarResultados(results);
        };
        reader.readAsArrayBuffer(file);
      } catch (error) {
        document.getElementById('statusMessage').innerHTML = `<div class="status error">Error al procesar el PDF: ${error.message}</div>`;
      }
    } else {
      document.getElementById('statusMessage').innerHTML = `<div class="status error">Tipo de archivo no soportado.</div>`;
    }
  });

  /**********************
   * Generación de Informe PDF
   **********************/
  async function generarInformePDF() {
    if (!globalResults) return;
    
    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();
    let y = 10;
    doc.setFontSize(16);
    doc.text("Informe de Gestión Contable Avanzada", 10, y);
    y += 10;
    
    doc.setFontSize(14);
    doc.text("Empresa:", 10, y);
    y += 7;
    doc.setFontSize(12);
    doc.text(`Nombre: ${globalResults.empresa.nombre || 'N/A'}`, 10, y);
    y += 7;
    doc.text(`RFC: ${globalResults.empresa.rfc || 'N/A'}`, 10, y);
    y += 7;
    doc.text(`Dirección: ${globalResults.empresa.direccion || 'N/A'}`, 10, y);
    y += 10;
    
    function agregarSeccion(titulo, contenido) {
      doc.setFontSize(14);
      if(y > 260) { doc.addPage(); y = 10; }
      doc.text(titulo, 10, y);
      y += 7;
      doc.setFontSize(12);
      contenido.forEach(linea => {
        if(y > 260) { doc.addPage(); y = 10; }
        doc.text(linea, 10, y);
        y += 7;
      });
      y += 5;
    }
    
    // Cuentas Bancarias
    const bancos = globalResults.cuentasBancarias.map(c => `${c.banco}: $${c.saldo.toFixed(2)}`);
    bancos.push(`Total: $${globalResults.cuentasBancarias.reduce((acc, c) => acc + c.saldo, 0).toFixed(2)}`);
    agregarSeccion("Cuentas Bancarias:", bancos);
    
    // Facturas
    const facturas = globalResults.facturas.map(f => `#${f.numero} - $${f.monto.toFixed(2)} (${f.fecha})`);
    facturas.push(`Total Facturado: $${globalResults.impuestos.totalFacturado.toFixed(2)}`);
    agregarSeccion("Facturas:", facturas);
    
    // Impuestos
    const impuestos = [
      `IVA (16%): $${globalResults.impuestos.iva.toFixed(2)}`,
      `ISR: $${globalResults.impuestos.isr.toFixed(2)}`
    ];
    agregarSeccion("Impuestos:", impuestos);
    
    // Nómina
    const nomina = globalResults.nomina.salarios.map(n => `${n.empleado}: $${n.salario.toFixed(2)}`);
    nomina.push(`Total Salarios: $${globalResults.nomina.totalSalarios.toFixed(2)}`);
    nomina.push(`Retenciones (10%): $${globalResults.nomina.retenciones.toFixed(2)}`);
    nomina.push(`Provisiones (5%): $${globalResults.nomina.provisiones.toFixed(2)}`);
    nomina.push(`Impuesto Nómina (3%): $${globalResults.nomina.impuestoNomina.toFixed(2)}`);
    agregarSeccion("Nómina:", nomina);
    
    // Activos Fijos
    const activos = globalResults.activosFijos.map(a => `${a.activo}: $${a.valor.toFixed(2)}`);
    activos.push(`Depreciación Anual (10%): $${globalResults.depreciacion.anual.toFixed(2)}`);
    activos.push(`Depreciación Acumulada: $${globalResults.depreciacion.acumulada.toFixed(2)}`);
    agregarSeccion("Activos Fijos:", activos);
    
    // Cuentas por Cobrar
    const cobrar = globalResults.cuentasPorCobrar.map(c => `${c.cliente}: $${c.monto.toFixed(2)}`);
    cobrar.push(`Total por Cobrar: $${globalResults.cuentasPorCobrar.reduce((acc, c) => acc + c.monto, 0).toFixed(2)}`);
    agregarSeccion("Cuentas por Cobrar:", cobrar);
    
    // Cuentas por Pagar
    const pagar = globalResults.cuentasPorPagar.map(p => `${p.proveedor}: $${p.monto.toFixed(2)}`);
    pagar.push(`Total por Pagar: $${globalResults.cuentasPorPagar.reduce((acc, p) => acc + p.monto, 0).toFixed(2)}`);
    agregarSeccion("Cuentas por Pagar:", pagar);
    
    doc.save('informe_contable.pdf');
  }
</script>

</body>
</html>
