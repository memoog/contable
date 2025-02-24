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
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .container {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 1rem;
        }
        .section {
            background: white;
            border-radius: 10px;
            box-shadow: 0 2px 15px rgba(0,0,0,0.05);
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
        input, select {
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
        th, td {
            padding: 12px;
            border: 1px solid #dee2e6;
            text-align: left;
        }
        th {
            background: #2c3e50;
            color: white;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>Sistema Contable Profesional</h1>
        <input type="file" id="excelInput" accept=".xlsx, .xls" />
    </div>

    <div class="container">
        <!-- Clasificación Contable Mejorada -->
        <div class="section">
            <div class="section-header" onclick="toggleSection(this)">
                <span>📂 Clasificación Automática de Datos</span>
                <span>▼</span>
            </div>
            <div class="section-content" id="clasificacionContable">
                <div class="grid-container" id="clasificacionGrid"></div>
            </div>
        </div>

        <!-- Herramientas de Cálculo Integradas -->
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
                        </select>
                    </div>
                    
                    <div id="calculadoraDinamica"></div>
                    <button onclick="realizarCalculo()">Calcular</button>
                    <div class="resultado" id="resultadoCalculo"></div>
                </div>
            </div>
        </div>
    </div>

<script>
const TASAS = {
    IVA: 0.16,
    ISR: 0.30,
    PPA: 0.0125
};

let datosGlobales = {
    clasificacion: {},
    calculos: {}
};

document.getElementById('excelInput').addEventListener('change', function(e) {
    const file = e.target.files[0];
    const reader = new FileReader();
    
    reader.onload = function(e) {
        procesarExcel(e.target.result);
    };
    
    reader.readAsArrayBuffer(file);
});

function procesarExcel(data) {
    try {
        const workbook = XLSX.read(new Uint8Array(data), { type: 'array' });
        clasificarDatosContables(workbook);
        actualizarInterfazClasificacion();
    } catch(error) {
        alert('Error al procesar el archivo: ' + error.message);
    }
}

function clasificarDatosContables(workbook) {
    datosGlobales.clasificacion = {
        ingresos: { datos: [], calculos: ['isr', 'cu', 'flujo'] },
        gastos: { datos: [], calculos: ['isr', 'iva'] },
        impuestos: { datos: [], calculos: ['iva', 'pp'] },
        activos: { datos: [], calculos: ['depreciacion'] },
        transacciones: { datos: [], calculos: ['flujo'] }
    };

    workbook.SheetNames.forEach(sheetName => {
        const sheet = workbook.Sheets[sheetName];
        const data = XLSX.utils.sheet_to_json(sheet, { header: 1 });
        
        data.forEach(row => {
            const concepto = (row[0] || '').toString().toLowerCase();
            const valor = row[1] || '';
            
            // Clasificación mejorada basada en estructura del archivo
            if (/(ingreso|venta|depósito)/i.test(concepto)) {
                datosGlobales.clasificacion.ingresos.datos.push(row);
            }
            if (/(gasto|retiro|pago)/i.test(concepto)) {
                datosGlobales.clasificacion.gastos.datos.push(row);
            }
            if (/(isr|iva|impuesto)/i.test(concepto)) {
                datosGlobales.clasificacion.impuestos.datos.push(row);
            }
            if (/(activo|inventario)/i.test(concepto)) {
                datosGlobales.clasificacion.activos.datos.push(row);
            }
            if (/(transacción|movimiento)/i.test(concepto)) {
                datosGlobales.clasificacion.transacciones.datos.push(row);
            }
        });
    });
}

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
            <div class="acciones-calculo">
                <h4>Cálculos disponibles:</h4>
                ${info.calculos.map(calc => `
                    <div class="accion-calculo" onclick="seleccionarCalculo('${calc}')">
                        ${obtenerNombreCalculo(calc)}
                    </div>
                `).join('')}
            </div>
        </div>
    `).join('');
}

function obtenerNombreCalculo(key) {
    const nombres = {
        isr: '📝 Calcular ISR',
        iva: '🧾 Calcular IVA',
        cu: '📊 Coeficiente Utilidad',
        pp: '📅 Pago Provisional',
        flujo: '💸 Flujo de Efectivo',
        depreciacion: '📉 Depreciación'
    };
    return nombres[key] || key;
}

function seleccionarCalculo(calculo) {
    document.getElementById('tipoCalculo').value = calculo;
    cambiarCalculadora();
}

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
                    <input type="number" id="utilidadCU">
                </div>
                <div class="input-group">
                    <label>Ingresos Totales:</label>
                    <input type="number" id="ingresosCU">
                </div>
            `;
            break;
            
        case 'pp':
            html = `
                <div class="input-group">
                    <label>Ingresos del Mes:</label>
                    <input type="number" id="ingresosPP">
                </div>
            `;
            break;
            
        case 'flujo':
            html = `
                <div class="input-group">
                    <label>Entradas de Efectivo:</label>
                    <input type="number" id="entradas">
                </div>
                <div class="input-group">
                    <label>Salidas de Efectivo:</label>
                    <input type="number" id="salidas">
                </div>
            `;
            break;
    }
    
    document.getElementById('calculadoraDinamica').innerHTML = html;
}

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
    }
    
    document.getElementById('resultadoCalculo').innerHTML = resultado;
}

function formatoMoneda(valor) {
    return new Intl.NumberFormat('es-MX', {
        style: 'currency',
        currency: 'MXN',
        minimumFractionDigits: 2
    }).format(valor);
}

function toggleSection(header) {
    const content = header.nextElementSibling;
    content.style.display = content.style.display === 'none' ? 'block' : 'none';
}

// Inicialización
cambiarCalculadora();
</script>
</body>
</html>
