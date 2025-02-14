<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestión Contable Avanzada</title>
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
    </style>
</head>

<body>

    <div class="container">
        <h1>Gestión Contable Avanzada</h1>
        <div class="upload-section" onclick="document.getElementById('fileInput').click()">
            <p>📂 Arrastra o selecciona un archivo Excel</p>
            <input type="file" id="fileInput" style="display: none;" accept=".xlsx,.xls">
        </div>
        <div id="statusMessage"></div>
        <div class="results" id="resultsContainer"></div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script>
        document.getElementById('fileInput').addEventListener('change', async(event) => {
            const file = event.target.files[0];
            if (!file) return;

            document.getElementById('statusMessage').innerHTML = `<div class="status processing">Procesando archivo ${file.name}...</div>`;

            try {
                const reader = new FileReader();
                reader.onload = async(e) => {
                    const data = new Uint8Array(e.target.result);
                    const workbook = XLSX.read(data, {
                        type: 'array'
                    });
                    const sheet = workbook.Sheets[workbook.SheetNames[0]];
                    const jsonData = XLSX.utils.sheet_to_json(sheet);

                    const results = procesarDatos(jsonData);
                    mostrarResultados(results);
                };
                reader.readAsArrayBuffer(file);
            } catch (error) {
                document.getElementById('statusMessage').innerHTML = `<div class="status error">Error al procesar el archivo: ${error.message}</div>`;
            }
        });

        // Función para calcular el ISR según las tasas progresivas en México
        function calcularISR(ingresos) {
            let isr = 0;
            if (ingresos <= 416220.00) {
                isr = ingresos * 0.15;
            } else if (ingresos <= 624329.00) {
                isr = 62432.00 + (ingresos - 416220.00) * 0.20;
            } else if (ingresos <= 867123.00) {
                isr = 104864.00 + (ingresos - 624329.00) * 0.25;
            } else {
                isr = 162864.00 + (ingresos - 867123.00) * 0.30;
            }
            return isr;
        }

        function procesarDatos(datos) {
            let empresa = {};
            let cuentasBancarias = [];
            let facturas = [];
            let impuestos = {
                totalFacturado: 0,
                iva: 0,
                isr: 0
            };
            let nomina = {
                salarios: [],
                totalSalarios: 0,
                retenciones: 0,
                provisiones: 0
            };
            let activosFijos = [];
            let cuentasPorPagar = [];
            let cuentasPorCobrar = [];
            let depreciacion = {
                anual: 0,
                acumulada: 0
            };

            datos.forEach(row => {
                // Datos de la empresa
                if (row.EMPRESA) empresa = {
                    nombre: row.EMPRESA,
                    rfc: row.RFC,
                    direccion: row.DIRECCION
                };

                // Cuentas bancarias
                if (row.BANCO && row.SALDO) {
                    cuentasBancarias.push({
                        banco: row.BANCO,
                        saldo: parseFloat(row.SALDO)
                    });
                }

                // Facturas
                if (row.FACTURA && row.MONTO) {
                    facturas.push({
                        numero: row.FACTURA,
                        monto: parseFloat(row.MONTO),
                        fecha: row.FECHA
                    });
                    impuestos.totalFacturado += parseFloat(row.MONTO);
                }

                // Nómina
                if (row.EMPLEADO && row.SALARIO) {
                    const salario = parseFloat(row.SALARIO);
                    nomina.salarios.push({
                        empleado: row.EMPLEADO,
                        salario
                    });
                    nomina.totalSalarios += salario;
                    nomina.retenciones += salario * 0.10; // Retención del 10%
                    nomina.provisiones += salario * 0.05; // Provisión del 5%
                }

                // Activos fijos
                if (row.ACTIVO && row.VALOR) {
                    const valor = parseFloat(row.VALOR);
                    activosFijos.push({
                        activo: row.ACTIVO,
                        valor
                    });
                    depreciacion.anual += valor * 0.10; // Depreciación anual del 10%
                    depreciacion.acumulada += valor * 0.10; // Depreciación acumulada
                }

                // Cuentas por cobrar
                if (row.CLIENTE && row.COBRAR) {
                    cuentasPorCobrar.push({
                        cliente: row.CLIENTE,
                        monto: parseFloat(row.COBRAR)
                    });
                }

                // Cuentas por pagar
                if (row.PROVEEDOR && row.PAGAR) {
                    cuentasPorPagar.push({
                        proveedor: row.PROVEEDOR,
                        monto: parseFloat(row.PAGAR)
                    });
                }
            });

            // Cálculos de impuestos
            impuestos.iva = impuestos.totalFacturado * 0.16; // IVA del 16%
            impuestos.isr = calcularISR(impuestos.totalFacturado); // ISR según tasas progresivas

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

        function mostrarResultados(results) {
            document.getElementById('statusMessage').innerHTML = `<div class="status success">Archivo procesado correctamente.</div>`;

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
                <p>IVA: $${results.impuestos.iva.toFixed(2)}</p>
                <p>ISR: $${results.impuestos.isr.toFixed(2)}</p>
            </div>
            <div class="card">
                <h3>💼 Nómina</h3>
                ${results.nomina.salarios.map(n => `<p>${n.empleado}: $${n.salario.toFixed(2)}</p>`).join('')}
                <p class="total">Total Salarios: $${results.nomina.totalSalarios.toFixed(2)}</p>
                <p>Retenciones: $${results.nomina.retenciones.toFixed(2)}</p>
                <p>Provisiones: $${results.nomina.provisiones.toFixed(2)}</p>
            </div>
            <div class="card">
                <h3>🏢 Activos Fijos</h3>
                ${results.activosFijos.map(a => `<p>${a.activo}: $${a.valor.toFixed(2)}</p>`).join('')}
                <p class="total">Depreciación Anual: $${results.depreciacion.anual.toFixed(2)}</p>
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
    </script>

</body>

</html>
