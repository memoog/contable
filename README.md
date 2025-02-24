<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema Contable Profesional</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f8f9fa;
            color: #2c3e50;
        }
        
        .header {
            background: #2c3e50;
            color: white;
            padding: 1.5rem;
            text-align: center;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .container {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 1rem;
        }
        
        .section {
            background: white;
            border-radius: 10px;
            box-shadow: 0 2px 15px rgba(0, 0, 0, 0.05);
            margin-bottom: 2rem;
            overflow: hidden;
        }
        
        .section-header {
            background: #3498db;
            color: white;
            padding: 1rem;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: background 0.3s;
        }
        
        .section-header:hover {
            background: #2980b9;
        }
        
        .section-content {
            padding: 1.5rem;
            border-top: 2px solid #f1f3f5;
            display: block;
        }
        
        .grid-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 1.5rem;
        }
        
        .clasificacion-item {
            padding: 1rem;
            border: 1px solid #dee2e6;
            border-radius: 8px;
            background: #fff;
        }
        
        .calculadora-container {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1rem;
        }
        
        .input-group {
            margin-bottom: 1rem;
        }
        
        input,
        select {
            width: 100%;
            padding: 0.8rem;
            border: 1px solid #dee2e6;
            border-radius: 4px;
            margin-top: 0.5rem;
        }
        
        button {
            background: #27ae60;
            color: white;
            border: none;
            padding: 0.8rem 1.5rem;
            border-radius: 4px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        button:hover {
            background: #219a52;
        }
        
        .accion-calculo {
            background: #e3f2fd;
            padding: 0.5rem;
            margin: 0.3rem 0;
            border-radius: 4px;
            cursor: pointer;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 1rem 0;
        }
        
        th,
        td {
            padding: 12px;
            border: 1px solid #dee2e6;
            text-align: left;
        }
        
        th {
            background: #2c3e50;
            color: white;
        }
        /* Estilos para pestañas en Datos Extraídos */
        
        .tab-buttons {
            display: flex;
            border-bottom: 2px solid #dee2e6;
            margin-bottom: 1rem;
        }
        
        .tab-button {
            padding: 0.5rem 1rem;
            cursor: pointer;
            background: #f1f3f5;
            border: 1px solid #dee2e6;
            border-bottom: none;
            margin-right: 0.5rem;
            border-radius: 4px 4px 0 0;
        }
        
        .tab-button.active {
            background: white;
            font-weight: bold;
            border-bottom: 2px solid white;
        }
    </style>
</head>

<body>
    <div class="header">
        <h1>Sistema Contable Profesional</h1>
        <input type="file" id="excelInput" accept=".xlsx, .xls" />
    </div>

    <div class="container">
        <!-- Clasificación Automática de Datos -->
        <div class="section">
            <div class="section-header" onclick="toggleSection(this)">
                <span>📂 Clasificación Automática de Datos</span>
                <span>▼</span>
            </div>
            <div class="section-content" id="clasificacionContable">
                <div class="grid-container" id="clasificacionGrid"></div>
            </div>
        </div>

        <!-- Resumen de Clasificación -->
        <div class="section">
            <div class="section-header" onclick="toggleSection(this)">
                <span>📊 Resumen de Clasificación de Datos</span>
                <span>▼</span>
            </div>
            <div class="section-content" id="resumenClasificacion">
                <!-- Se mostrará un resumen con el número de registros por categoría -->
            </div>
        </div>

        <!-- Herramientas de Cálculo Contable -->
        <div class="section">
            <div class="section-header" onclick="toggleSection(this)">
                <span>🧮 Herramientas de Cálculo Contable</span>
                <span>▼</span>
            </div>
            <div class="section-content">
                <div class="calculadora-container">
                    <div class="input-group">
                        <label>Seleccione cálculo:</label>
                        <select id="tipoCalculo" onchange="cambiarCalculadora()">
              <option value="isr">ISR sobre Utilidad</option>
              <option value="iva">Cálculo de IVA</option>
              <option value="cu">Coeficiente de Utilidad</option>
              <option value="pp">Pago Provisional</option>
              <option value="flujo">Flujo de Efectivo</option>
              <option value="depreciacion">Depreciación</option>
              <option value="razon">Razones Financieras</option>
              <option value="margen">Margen de Utilidad</option>
            </select>
                    </div>

                    <div id="calculadoraDinamica"></div>
                    <button onclick="realizarCalculo()">Calcular</button>
                    <div class="resultado" id="resultadoCalculo"></div>
                </div>
            </div>
        </div>

        <!-- Datos Extraídos Completos (Vista en Pestañas) -->
        <div class="section">
            <div class="section-header" onclick="toggleSection(this)">
                <span>📄 Datos Extraídos Completos</span>
                <span>▼</span>
            </div>
            <div class="section-content" id="datosExtraidos">
                <div class="tab-buttons" id="datosExtraidosTabs"></div>
                <div id="datosExtraidosContent"></div>
            </div>
        </div>
    </div>

    <script>
        // Tasas y variables globales
        const TASAS = {
            IVA: 0.16,
            ISR: 0.30,
            PPA: 0.0125
        };

        let datosGlobales = {
            clasificacion: {},
            extraidos: {}
        };

        // Palabras clave para la clasificación según la información necesaria para un contador profesional
        const categoriasKeywords = {
            documentos: [
                /acta constitutiva/i,
                /rfc/i,
                /e\.firma/i,
                /registro ante el sat/i,
                /cfdi/i,
                /balance general/i,
                /estado de resultados/i,
                /registro de ingresos/i,
                /egresos/i,
                /cuentas bancarias/i
            ],
            facturacion: [
                /factura/i,
                /emisión de factura/i,
                /recepción de factura/i,
                /cuentas por cobrar/i,
                /registro de ventas/i,
                /nota de crédito/i,
                /devolución/i
            ],
            gastos: [
                /gasto/i,
                /retiro/i,
                /pago/i,
                /proveedor/i,
                /nómina/i,
                /renta/i,
                /luz/i,
                /internet/i,
                /servicio/i,
                /viático/i,
                /compra de insumos/i
            ],
            impuestos: [
                /isr/i,
                /iva/i,
                /imss/i,
                /diot/i,
                /retención/i,
                /declaración/i,
                /sat/i,
                /impuesto/i,
                /pago provisional/i
            ],
            financiero: [
                /flujo/i,
                /efectivo/i,
                /conciliación/i,
                /presupuesto/i,
                /proyección/i,
                /razón financiera/i,
                /balance/i,
                /estado financiero/i,
                /análisis/i
            ],
            cumplimiento: [
                /nómina/i,
                /seguridad social/i,
                /infonavit/i,
                /sar/i,
                /contrato/i,
                /aguinaldo/i,
                /stps/i,
                /obligaciones laborales/i,
                /prestaciones/i
            ],
            auditoria: [
                /auditoría/i,
                /control interno/i,
                /inventario/i,
                /revisión de documentación/i,
                /procedimiento interno/i,
                /política/i
            ]
        };

        // Manejo del archivo Excel
        document.getElementById('excelInput').addEventListener('change', function(e) {
            const file = e.target.files[0];
            const reader = new FileReader();

            reader.onload = function(e) {
                procesarExcel(e.target.result);
            };

            reader.readAsArrayBuffer(file);
        });

        // Procesa el archivo y actualiza las secciones de la interfaz
        function procesarExcel(data) {
            try {
                const workbook = XLSX.read(new Uint8Array(data), {
                    type: 'array'
                });
                clasificarDatosContables(workbook);
                actualizarInterfazClasificacion();
                actualizarResumenClasificacion();
                crearTabsExtraidos();
            } catch (error) {
                alert('Error al procesar el archivo: ' + error.message);
            }
        }

        // Clasifica cada fila de cada hoja en las categorías definidas
        function clasificarDatosContables(workbook) {
            // Inicializar las categorías según lo requerido
            datosGlobales.clasificacion = {
                documentos: {
                    datos: [],
                    calculos: []
                },
                facturacion: {
                    datos: [],
                    calculos: []
                },
                gastos: {
                    datos: [],
                    calculos: []
                },
                impuestos: {
                    datos: [],
                    calculos: ['isr', 'iva', 'pp']
                },
                financiero: {
                    datos: [],
                    calculos: ['flujo', 'razon']
                },
                cumplimiento: {
                    datos: [],
                    calculos: []
                },
                auditoria: {
                    datos: [],
                    calculos: ['depreciacion']
                }
            };

            datosGlobales.extraidos = {};

            workbook.SheetNames.forEach(sheetName => {
                const sheet = workbook.Sheets[sheetName];
                const data = XLSX.utils.sheet_to_json(sheet, {
                    header: 1
                });
                datosGlobales.extraidos[sheetName] = data;

                data.forEach(row => {
                    const texto = (row[0] || '').toString().toLowerCase();
                    for (const categoria in categoriasKeywords) {
                        categoriasKeywords[categoria].forEach(regex => {
                            if (regex.test(texto)) {
                                datosGlobales.clasificacion[categoria].datos.push(row);
                            }
                        });
                    }
                });
            });
        }

        // Actualiza la interfaz de clasificación (muestra algunas filas de ejemplo y herramientas disponibles)
        function actualizarInterfazClasificacion() {
            const container = document.getElementById('clasificacionGrid');
            container.innerHTML = Object.entries(datosGlobales.clasificacion).map(([categoria, info]) => `
        <div class="clasificacion-item">
          <h3>📦 ${categoria.toUpperCase()}</h3>
          <div class="datos-muestra">
            ${info.datos.slice(0, 3).map(row => `
              <div>${row[0]}: ${formatoMoneda(row[1])}</div>
            `).join('')}
          </div>
          ${info.calculos.length > 0 ? `
          <div class="acciones-calculo">
            <h4>Cálculos disponibles:</h4>
            ${info.calculos.map(calc => `
              <div class="accion-calculo" onclick="seleccionarCalculo('${calc}')">
                ${obtenerNombreCalculo(calc)}
              </div>
            `).join('')}
          </div>
          ` : ''}
        </div>
      `).join('');
    }

    // Actualiza el resumen de clasificación mostrando el número de registros por categoría
    function actualizarResumenClasificacion() {
      const container = document.getElementById('resumenClasificacion');
      let html = '<table><tr><th>Categoría</th><th>Número de Registros</th></tr>';
      for (const [categoria, info] of Object.entries(datosGlobales.clasificacion)) {
        html += `<tr><td>${categoria.toUpperCase()}</td><td>${info.datos.length}</td></tr>`;
      }
      html += '</table>';
      container.innerHTML = html;
    }

    // Crea la interfaz en pestañas para mostrar los datos extraídos de cada hoja
    function crearTabsExtraidos() {
      const tabsContainer = document.getElementById('datosExtraidosTabs');
      const contentContainer = document.getElementById('datosExtraidosContent');
      tabsContainer.innerHTML = '';
      
      // Crear botón para cada hoja
      const sheetNames = Object.keys(datosGlobales.extraidos);
      sheetNames.forEach((sheetName, index) => {
        const btn = document.createElement('div');
        btn.className = 'tab-button' + (index === 0 ? ' active' : '');
        btn.innerText = sheetName;
        btn.onclick = function() {
          document.querySelectorAll('.tab-button').forEach(b => b.classList.remove('active'));
          this.classList.add('active');
          mostrarHoja(sheetName);
        };
        tabsContainer.appendChild(btn);
      });
      
      // Mostrar la primera hoja por defecto
      if (sheetNames.length > 0) {
        mostrarHoja(sheetNames[0]);
      }
    }

    // Muestra el contenido de una hoja en forma de tabla
    function mostrarHoja(sheetName) {
      const data = datosGlobales.extraidos[sheetName];
      let html = '<table>';
      data.forEach(row => {
        html += '<tr>';
        row.forEach(cell => {
          html += `<td>${cell}</td>`;
        });
        html += '</tr>';
      });
      html += '</table>';
      document.getElementById('datosExtraidosContent').innerHTML = html;
    }

    // Mapea las claves de los cálculos a nombres descriptivos
    function obtenerNombreCalculo(key) {
      const nombres = {
        isr: '📝 Calcular ISR',
        iva: '🧾 Calcular IVA',
        cu: '📊 Coeficiente Utilidad',
        pp: '📅 Pago Provisional',
        flujo: '💸 Flujo de Efectivo',
        depreciacion: '📉 Depreciación',
        razon: '📈 Razones Financieras',
        margen: '📊 Margen de Utilidad'
      };
      return nombres[key] || key;
    }

    // Permite seleccionar un cálculo desde la sección de clasificación
    function seleccionarCalculo(calculo) {
      document.getElementById('tipoCalculo').value = calculo;
      cambiarCalculadora();
    }

    // Muestra el formulario dinámico según el cálculo seleccionado
    function cambiarCalculadora() {
      const tipo = document.getElementById('tipoCalculo').value;
      let html = '';
      
      switch(tipo) {
        case 'isr':
          html = `
            <div class="input-group">
              <label>Utilidad Fiscal:</label>
              <input type="number" id="utilidad" placeholder="Ingrese la utilidad fiscal">
            </div>
          `;
          break;
        case 'iva':
          html = `
            <div class="input-group">
              <label>Base Gravable:</label>
              <input type="number" id="baseIVA" placeholder="Ingrese la base gravable">
            </div>
          `;
          break;
        case 'cu':
          html = `
            <div class="input-group">
              <label>Utilidad Fiscal:</label>
              <input type="number" id="utilidadCU" placeholder="Ingrese la utilidad fiscal">
            </div>
            <div class="input-group">
              <label>Ingresos Totales:</label>
              <input type="number" id="ingresosCU" placeholder="Ingrese los ingresos totales">
            </div>
          `;
          break;
        case 'pp':
          html = `
            <div class="input-group">
              <label>Ingresos del Mes:</label>
              <input type="number" id="ingresosPP" placeholder="Ingrese los ingresos del mes">
            </div>
          `;
          break;
        case 'flujo':
          html = `
            <div class="input-group">
              <label>Entradas de Efectivo:</label>
              <input type="number" id="entradas" placeholder="Ingrese las entradas de efectivo">
            </div>
            <div class="input-group">
              <label>Salidas de Efectivo:</label>
              <input type="number" id="salidas" placeholder="Ingrese las salidas de efectivo">
            </div>
          `;
          break;
        case 'depreciacion':
          html = `
            <div class="input-group">
              <label>Valor Inicial:</label>
              <input type="number" id="valorInicial" placeholder="Ingrese el valor inicial">
            </div>
            <div class="input-group">
              <label>Valor Residual:</label>
              <input type="number" id="valorResidual" placeholder="Ingrese el valor residual">
            </div>
            <div class="input-group">
              <label>Vida Útil (años):</label>
              <input type="number" id="vidaUtil" placeholder="Ingrese la vida útil en años">
            </div>
          `;
          break;
        case 'razon':
          html = `
            <div class="input-group">
              <label>Activos Corrientes:</label>
              <input type="number" id="activosCorrientes" placeholder="Ingrese el total de activos corrientes">
            </div>
            <div class="input-group">
              <label>Pasivos Corrientes:</label>
              <input type="number" id="pasivosCorrientes" placeholder="Ingrese el total de pasivos corrientes">
            </div>
          `;
          break;
        case 'margen':
          html = `
            <div class="input-group">
              <label>Utilidad Neta:</label>
              <input type="number" id="utilidadNeta" placeholder="Ingrese la utilidad neta">
            </div>
            <div class="input-group">
              <label>Ingresos Totales:</label>
              <input type="number" id="ingresosMargen" placeholder="Ingrese los ingresos totales">
            </div>
          `;
          break;
      }
      
      document.getElementById('calculadoraDinamica').innerHTML = html;
    }

    // Realiza el cálculo según el tipo seleccionado y muestra el resultado
    function realizarCalculo() {
      const tipo = document.getElementById('tipoCalculo').value;
      let resultado = '';
      
      switch(tipo) {
        case 'isr':
          const utilidad = parseFloat(document.getElementById('utilidad').value) || 0;
          resultado = `ISR a Pagar: ${formatoMoneda(utilidad * TASAS.ISR)}`;
          break;
        case 'iva':
          const baseIVA = parseFloat(document.getElementById('baseIVA').value) || 0;
          resultado = `IVA Trasladado: ${formatoMoneda(baseIVA * TASAS.IVA)}`;
          break;
        case 'cu':
          const utilidadCU = parseFloat(document.getElementById('utilidadCU').value) || 0;
          const ingresosCU = parseFloat(document.getElementById('ingresosCU').value) || 1;
          resultado = `Coeficiente de Utilidad: ${(utilidadCU/ingresosCU).toFixed(2)}`;
          break;
        case 'pp':
          const ingresosPP = parseFloat(document.getElementById('ingresosPP').value) || 0;
          resultado = `Pago Provisional: ${formatoMoneda(ingresosPP * TASAS.PPA)}`;
          break;
        case 'flujo':
          const entradas = parseFloat(document.getElementById('entradas').value) || 0;
          const salidas = parseFloat(document.getElementById('salidas').value) || 0;
          resultado = `Flujo Neto: ${formatoMoneda(entradas - salidas)}`;
          break;
        case 'depreciacion':
          const valorInicial = parseFloat(document.getElementById('valorInicial').value) || 0;
          const valorResidual = parseFloat(document.getElementById('valorResidual').value) || 0;
          const vidaUtil = parseFloat(document.getElementById('vidaUtil').value) || 1;
          const depreciacion = (valorInicial - valorResidual) / vidaUtil;
          resultado = `Depreciación Anual: ${formatoMoneda(depreciacion)}`;
          break;
        case 'razon':
          const activosCorrientes = parseFloat(document.getElementById('activosCorrientes').value) || 0;
          const pasivosCorrientes = parseFloat(document.getElementById('pasivosCorrientes').value) || 1;
          const razonLiquidez = activosCorrientes / pasivosCorrientes;
          resultado = `Razón de Liquidez: ${razonLiquidez.toFixed(2)}`;
          break;
        case 'margen':
          const utilidadNeta = parseFloat(document.getElementById('utilidadNeta').value) || 0;
          const ingresosMargen = parseFloat(document.getElementById('ingresosMargen').value) || 1;
          const margenUtilidad = (utilidadNeta / ingresosMargen) * 100;
          resultado = `Margen de Utilidad: ${margenUtilidad.toFixed(2)} %`;
          break;
      }
      
      document.getElementById('resultadoCalculo').innerHTML = resultado;
    }

    // Formatea números en formato moneda MXN
    function formatoMoneda(valor) {
      return new Intl.NumberFormat('es-MX', {
        style: 'currency',
        currency: 'MXN',
        minimumFractionDigits: 2
      }).format(valor);
    }

    // Alterna la visibilidad de cada sección
    function toggleSection(header) {
      const content = header.nextElementSibling;
      content.style.display = content.style.display === 'none' ? 'block' : 'none';
    }

    // Inicializa el formulario de cálculos
    cambiarCalculadora();
    </script>
</body>

</html>
