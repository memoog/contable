<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ContaMex - Análisis y Extracción de Excel</title>
  
  <!-- Librerías para procesar Excel y generar PDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  
  <style>
    /* Estilos generales */
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f9;
      margin: 0;
      padding: 0;
      color: #333;
    }
    .header {
      background: #2c3e50;
      color: #fff;
      padding: 20px;
      text-align: center;
    }
    .header input[type="file"] {
      margin-top: 10px;
      padding: 8px;
      border-radius: 5px;
      border: none;
      cursor: pointer;
    }
    .nav-tabs {
      display: flex;
      justify-content: center;
      background: #3498db;
    }
    .nav-tabs button {
      background: none;
      border: none;
      color: #fff;
      padding: 15px 30px;
      cursor: pointer;
      font-size: 16px;
    }
    .nav-tabs button.active {
      background: #2980b9;
    }
    .container {
      max-width: 1000px;
      margin: 20px auto;
      padding: 20px;
      background: #fff;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    /* Acordeón para datos extraídos */
    .sheet-section {
      margin-bottom: 15px;
      border: 1px solid #ddd;
      border-radius: 5px;
      overflow: hidden;
    }
    .sheet-header {
      background: #2980b9;
      color: #fff;
      padding: 10px 15px;
      cursor: pointer;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .sheet-content {
      display: none;
      padding: 10px 15px;
      border-top: 1px solid #ddd;
    }
    .scroll-container {
      overflow-x: auto;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-bottom: 10px;
    }
    th, td {
      padding: 8px;
      border: 1px solid #ccc;
      text-align: left;
    }
    th {
      background: #3498db;
      color: #fff;
    }
    .export-buttons {
      text-align: center;
      margin-top: 20px;
    }
    .export-btn {
      background: #27ae60;
      color: #fff;
      border: none;
      padding: 10px 20px;
      margin: 0 10px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
  </style>
</head>
<body>

  <!-- Encabezado con única entrada para subir archivo -->
  <div class="header">
    <h1>ContaMex - Análisis y Extracción de Excel</h1>
    <p>Sube tu archivo Excel para extraer toda la información y realizar el análisis contable.</p>
    <input type="file" id="excelInput" accept=".xlsx, .xls" />
  </div>

  <!-- Pestañas de navegación -->
  <div class="nav-tabs">
    <button id="tabExtracted" class="active" onclick="showTab('extracted')">Datos Extraídos</button>
    <button id="tabAnalysis" onclick="showTab('analysis')">Análisis Contable</button>
  </div>

  <!-- Contenedor para Datos Extraídos -->
  <div id="extractedContainer" class="container">
    <h2>Datos Extraídos</h2>
    <div id="dataExtracted"></div>
  </div>

  <!-- Contenedor para Análisis Contable -->
  <div id="analysisContainer" class="container" style="display:none;">
    <h2>Análisis Contable</h2>
    <div id="analysisContent"></div>
    <div class="export-buttons">
      <button class="export-btn" onclick="exportarExcel()">Exportar Excel</button>
      <button class="export-btn" onclick="generarPDF()">Generar PDF</button>
    </div>
  </div>
  
  <script>
    let globalWorkbook = null;
    /* Función para cambiar de pestaña */
    function showTab(tab) {
      if(tab === 'extracted'){
        document.getElementById("extractedContainer").style.display = "block";
        document.getElementById("analysisContainer").style.display = "none";
        document.getElementById("tabExtracted").classList.add("active");
        document.getElementById("tabAnalysis").classList.remove("active");
      } else {
        document.getElementById("extractedContainer").style.display = "none";
        document.getElementById("analysisContainer").style.display = "block";
        document.getElementById("tabExtracted").classList.remove("active");
        document.getElementById("tabAnalysis").classList.add("active");
      }
    }
    
    /* Event listener para subir el archivo */
    document.getElementById("excelInput").addEventListener("change", function(e) {
      const file = e.target.files[0];
      if(!file) return;
      const reader = new FileReader();
      reader.onload = function(e) {
        const data = new Uint8Array(e.target.result);
        try {
          globalWorkbook = XLSX.read(data, { type: "array" });
        } catch (error) {
          alert("Error al leer el archivo Excel.");
          return;
        }
        // Procesa y muestra ambos módulos
        mostrarDatosExtraidos(globalWorkbook);
        procesarAnalisis(globalWorkbook);
      };
      reader.readAsArrayBuffer(file);
    });
    
    /* Función para generar tabla HTML a partir de datos */
    function generarTablaHTML(dataArray) {
      let html = '<div class="scroll-container"><table>';
      dataArray.forEach((row, rowIndex) => {
        html += "<tr>";
        row.forEach(cell => {
          if(rowIndex === 0) {
            html += `<th>${cell !== undefined ? cell : ""}</th>`;
          } else {
            html += `<td>${cell !== undefined ? cell : ""}</td>`;
          }
        });
        html += "</tr>";
      });
      html += "</table></div>";
      return html;
    }
    
    /* Función para mostrar toda la información extraída (por hoja) en formato acordeón */
    function mostrarDatosExtraidos(workbook) {
      const container = document.getElementById("dataExtracted");
      container.innerHTML = "";
      workbook.SheetNames.forEach(sheetName => {
        const sheet = workbook.Sheets[sheetName];
        const dataArray = XLSX.utils.sheet_to_json(sheet, { header: 1 });
        let sectionHTML = `<div class="sheet-section">
                             <div class="sheet-header" onclick="toggleSheetContent(this)">
                               <span>${sheetName}</span>
                               <span>▼</span>
                             </div>
                             <div class="sheet-content">
                               ${dataArray && dataArray.length > 0 ? generarTablaHTML(dataArray) : "<p>No hay datos en esta hoja.</p>"}
                             </div>
                           </div>`;
        container.innerHTML += sectionHTML;
      });
    }
    
    /* Función para el acordeón */
    function toggleSheetContent(header) {
      const content = header.nextElementSibling;
      content.style.display = (content.style.display === "block") ? "none" : "block";
    }
    
    /**************************
     * MÓDULO DE ANÁLISIS CONTABLE
     **************************/
    // Definiciones para identificar hojas mensuales y extraer datos
    const mesesOrdenados = ["ENERO", "FEBRERO", "MARZO", "ABRIL", "MAYO", "JUNIO", "JULIO", "AGOSTO", "SEPTIEMBRE", "OCTUBRE", "NOVIEMBRE", "DICIEMBRE"];
    const mappingMeses = {
      "ENERO": ["enero", "ene"],
      "FEBRERO": ["febrero", "feb"],
      "MARZO": ["marzo", "mar"],
      "ABRIL": ["abril", "abr", "brio"],
      "MAYO": ["mayo", "may"],
      "JUNIO": ["junio", "jun"],
      "JULIO": ["julio", "jul"],
      "AGOSTO": ["agosto", "ago"],
      "SEPTIEMBRE": ["septiembre", "sep"],
      "OCTUBRE": ["octubre", "oct"],
      "NOVIEMBRE": ["noviembre", "nov"],
      "DICIEMBRE": ["diciembre", "dic", "hoja11"]
    };
    
    // Palabras clave para extraer información de cada hoja mensual
    const camposExtraer = {
      depositosFacturados: { label: "Depósitos Nominales Facturados", search: ["depositos nominales facturados", "ingresos nominales facturados"] },
      depositosNoFacturados: { label: "Depósitos No Facturados", search: ["depositos no facturados", "depositos sin facturar"] },
      retirosFacturados: { label: "Retiros Facturados", search: ["retiros facturados"] },
      retirosNoFacturados: { label: "Retiros No Facturados", search: ["retiros no facturados"] }
    };
    
    // Parámetros para los cálculos de ISR
    const margenUtilidad = 0.10; // 10%
    const tasaImpuesto = 0.30;   // 30%
    
    // Objeto para almacenar los datos de análisis por mes
    let datosMes = {};
    
    // Función para normalizar texto
    function normalizarTexto(texto) {
      if(typeof texto !== "string") return "";
      return texto.normalize("NFD").replace(/[\u0300-\u036f]/g, "").toLowerCase().trim();
    }
    
    // Extraer suma de valores de una fila si la primera celda contiene alguna palabra clave
    function extraerValorDeFila(fila, busquedas) {
      const texto = normalizarTexto(fila[0] || "");
      let coincide = busquedas.some(term => texto.includes(normalizarTexto(term)));
      if(!coincide) return 0;
      let suma = 0;
      for(let i = 1; i < fila.length; i++) {
        let valor = parseFloat(fila[i]);
        if(!isNaN(valor)) {
          suma += valor;
        }
      }
      return suma;
    }
    
    // Extraer los valores relevantes de una hoja mensual
    function extraerValoresDeHoja(datosHoja) {
      let valores = {
        depositosFacturados: 0,
        depositosNoFacturados: 0,
        retirosFacturados: 0,
        retirosNoFacturados: 0
      };
      datosHoja.forEach(fila => {
        for(let key in camposExtraer) {
          const campo = camposExtraer[key];
          valores[key] += extraerValorDeFila(fila, campo.search);
        }
      });
      return valores;
    }
    
    // Procesar los datos de análisis de los 12 meses
    function procesarAnalisis(workbook) {
      datosMes = {}; // Reiniciar datos
      mesesOrdenados.forEach(mes => {
        // Buscar la hoja que corresponda al mes (usando mapping)
        let sheetName = workbook.SheetNames.find(nombre => {
          return mappingMeses[mes].some(patron => normalizarTexto(nombre).includes(normalizarTexto(patron)));
        });
        if(sheetName) {
          const sheet = workbook.Sheets[sheetName];
          const datosHoja = XLSX.utils.sheet_to_json(sheet, { header: 1 });
          const extraidos = extraerValoresDeHoja(datosHoja);
          extraidos.totalDepositos = extraidos.depositosFacturados + extraidos.depositosNoFacturados;
          extraidos.totalRetiros = extraidos.retirosFacturados + extraidos.retirosNoFacturados;
          extraidos.ingresosNominalesMes = extraidos.totalDepositos - extraidos.totalRetiros;
          datosMes[mes] = extraidos;
        } else {
          datosMes[mes] = {
            depositosFacturados: 0,
            depositosNoFacturados: 0,
            retirosFacturados: 0,
            retirosNoFacturados: 0,
            totalDepositos: 0,
            totalRetiros: 0,
            ingresosNominalesMes: 0
          };
        }
      });
      calcularCamposAdicionales();
      mostrarAnalisis();
    }
    
    // Calcular campos acumulados y derivados
    function calcularCamposAdicionales() {
      let acumulado = 0;
      mesesOrdenados.forEach(mes => {
        let d = datosMes[mes];
        d.ingresosNominalesAnteriores = acumulado;
        d.ingresosNominalesTotales = acumulado + d.ingresosNominalesMes;
        acumulado = d.ingresosNominalesTotales;
        d.utilidad = d.ingresosNominalesTotales * margenUtilidad;
        d.cu = (d.ingresosNominalesTotales !== 0) ? (d.utilidad / d.ingresosNominalesTotales) * 100 : 0;
        d.impuestoDeterminado = d.utilidad * tasaImpuesto;
        d.base = d.utilidad;
        d.tasa = tasaImpuesto * 100;
        d.ppa = d.impuestoDeterminado;
        d.isrMensual = d.impuestoDeterminado;
        // Para C.U. (Utilidad Fiscal)
        d.ingresosAcumulables = d.totalDepositos;
        d.deduccionesAutorizadas = d.totalRetiros;
        d.perdidas = (d.ingresosNominalesMes < 0) ? Math.abs(d.ingresosNominalesMes) : 0;
        d.utilidadFiscal = d.ingresosAcumulables - d.deduccionesAutorizadas - d.perdidas;
        d.resultadoFiscal = d.utilidadFiscal;
      });
    }
    
    // Mostrar el análisis contable en la pestaña "Análisis Contable"
    function mostrarAnalisis() {
      const container = document.getElementById("analysisContent");
      let html = `<h3>Reporte ISR</h3>`;
      html += `<div class="scroll-container"><table>
                <tr>
                  <th>Mes</th>
                  <th>Ingresos Nominales del Mes</th>
                  <th>Ingresos Nominales Anteriores</th>
                  <th>Ingresos Nominales Totales</th>
                  <th>Utilidad</th>
                  <th>C.U. (%)</th>
                  <th>Impuesto Determinado</th>
                  <th>Base</th>
                  <th>Tasa (%)</th>
                  <th>PPA</th>
                  <th>ISR Mensual</th>
                </tr>`;
      mesesOrdenados.forEach(mes => {
        const d = datosMes[mes];
        html += `<tr>
                   <td>${mes}</td>
                   <td>$${d.ingresosNominalesMes.toLocaleString()}</td>
                   <td>$${d.ingresosNominalesAnteriores.toLocaleString()}</td>
                   <td>$${d.ingresosNominalesTotales.toLocaleString()}</td>
                   <td>$${d.utilidad.toLocaleString()}</td>
                   <td>${d.cu.toFixed(2)}%</td>
                   <td>$${d.impuestoDeterminado.toLocaleString()}</td>
                   <td>$${d.base.toLocaleString()}</td>
                   <td>${d.tasa}%</td>
                   <td>$${d.ppa.toLocaleString()}</td>
                   <td>$${d.isrMensual.toLocaleString()}</td>
                 </tr>`;
      });
      html += `</table></div>`;
      
      html += `<h3>Reporte C.U. (Utilidad Fiscal)</h3>`;
      html += `<div class="scroll-container"><table>
                <tr>
                  <th>Mes</th>
                  <th>Ingresos Acumulables</th>
                  <th>Deducciones Autorizadas</th>
                  <th>Pérdidas</th>
                  <th>Utilidad Fiscal</th>
                  <th>Resultado Fiscal</th>
                  <th>Ingresos Nominales</th>
                </tr>`;
      mesesOrdenados.forEach(mes => {
        const d = datosMes[mes];
        html += `<tr>
                   <td>${mes}</td>
                   <td>$${d.ingresosAcumulables.toLocaleString()}</td>
                   <td>$${d.deduccionesAutorizadas.toLocaleString()}</td>
                   <td>$${d.perdidas.toLocaleString()}</td>
                   <td>$${d.utilidadFiscal.toLocaleString()}</td>
                   <td>$${d.resultadoFiscal.toLocaleString()}</td>
                   <td>$${d.ingresosNominalesMes.toLocaleString()}</td>
                 </tr>`;
      });
      html += `</table></div>`;
      container.innerHTML = html;
    }
    
    /**************************
     * Funciones de Exportación
     **************************/
    function exportarExcel() {
      const wb = XLSX.utils.book_new();
      // Hoja: Reporte ISR
      let wsDataISR = [];
      wsDataISR.push(["Mes", "Ingresos Nominales del Mes", "Ingresos Nominales Anteriores", "Ingresos Nominales Totales", "Utilidad", "C.U. (%)", "Impuesto Determinado", "Base", "Tasa (%)", "PPA", "ISR Mensual"]);
      mesesOrdenados.forEach(mes => {
        const d = datosMes[mes];
        wsDataISR.push([mes, d.ingresosNominalesMes, d.ingresosNominalesAnteriores, d.ingresosNominalesTotales, d.utilidad, d.cu, d.impuestoDeterminado, d.base, d.tasa, d.ppa, d.isrMensual]);
      });
      const wsISR = XLSX.utils.aoa_to_sheet(wsDataISR);
      XLSX.utils.book_append_sheet(wb, wsISR, "Reporte ISR");
      
      // Hoja: Reporte C.U.
      let wsDataCU = [];
      wsDataCU.push(["Mes", "Ingresos Acumulables", "Deducciones Autorizadas", "Pérdidas", "Utilidad Fiscal", "Resultado Fiscal", "Ingresos Nominales"]);
      mesesOrdenados.forEach(mes => {
        const d = datosMes[mes];
        wsDataCU.push([mes, d.ingresosAcumulables, d.deduccionesAutorizadas, d.perdidas, d.utilidadFiscal, d.resultadoFiscal, d.ingresosNominalesMes]);
      });
      const wsCU = XLSX.utils.aoa_to_sheet(wsDataCU);
      XLSX.utils.book_append_sheet(wb, wsCU, "Reporte CU");
      
      XLSX.writeFile(wb, "Reporte_Gestion_Contable.xlsx");
    }
    
    function generarPDF() {
      const { jsPDF } = jspdf;
      const doc = new jsPDF();
      let y = 15;
      doc.setFontSize(16);
      doc.text("Reporte ISR", 15, y);
      y += 10;
      mesesOrdenados.forEach(mes => {
        const d = datosMes[mes];
        doc.setFontSize(12);
        doc.text(`${mes}: Ingresos del Mes $${d.ingresosNominalesMes.toLocaleString()} | Acumulados $${d.ingresosNominalesAnteriores.toLocaleString()} | Totales $${d.ingresosNominalesTotales.toLocaleString()}`, 15, y);
        y += 7;
        doc.text(`Utilidad $${d.utilidad.toLocaleString()} | C.U. ${d.cu.toFixed(2)}%`, 15, y);
        y += 7;
        doc.text(`Impuesto $${d.impuestoDeterminado.toLocaleString()} | Base $${d.base.toLocaleString()} | Tasa ${d.tasa}% | PPA $${d.ppa.toLocaleString()} | ISR Mensual $${d.isrMensual.toLocaleString()}`, 15, y);
        y += 10;
        if(y > 250) { doc.addPage(); y = 15; }
      });
      doc.addPage();
      y = 15;
      doc.setFontSize(16);
      doc.text("Reporte C.U. (Utilidad Fiscal)", 15, y);
      y += 10;
      mesesOrdenados.forEach(mes => {
        const d = datosMes[mes];
        doc.setFontSize(12);
        doc.text(`${mes}: Ingresos Acumulables $${d.ingresosAcumulables.toLocaleString()} | Deducciones $${d.deduccionesAutorizadas.toLocaleString()} | Pérdidas $${d.perdidas.toLocaleString()}`, 15, y);
        y += 7;
        doc.text(`Utilidad Fiscal $${d.utilidadFiscal.toLocaleString()} | Resultado Fiscal $${d.resultadoFiscal.toLocaleString()} | Ingresos Nominales $${d.ingresosNominalesMes.toLocaleString()}`, 15, y);
        y += 10;
        if(y > 250) { doc.addPage(); y = 15; }
      });
      doc.save("Reporte_Gestion_Contable.pdf");
    }
  </script>
</body>
</html>
