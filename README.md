<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>App Contable Empresarial</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
        }
        
        header {
            background-color: #4CAF50;
            color: white;
            padding: 15px;
            text-align: center;
            font-size: 24px;
        }
        
        section {
            padding: 15px;
            margin: 10px;
            background-color: white;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        
        h2 {
            font-size: 20px;
            color: #4CAF50;
        }
        
        input,
        select,
        textarea {
            width: 100%;
            padding: 8px;
            margin: 5px 0;
            box-sizing: border-box;
        }
        
        .container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
    </style>
</head>

<body>

    <header>
        Sistema de Administración y Contabilidad Empresarial
    </header>

    <div class="container">

        <!-- Datos Generales -->
        <section>
            <h2>Datos Generales de la Empresa</h2>
            <input type="text" placeholder="Nombre de la empresa">
            <input type="text" placeholder="RFC">
            <select>
            <option>Selecciona Régimen Fiscal</option>
            <option>Persona Física con Actividad Empresarial</option>
            <option>RIF</option>
            <option>Persona Moral</option>
        </select>
            <input type="text" placeholder="Dirección Fiscal">
            <input type="text" placeholder="Teléfono">
            <input type="email" placeholder="Correo Electrónico">
            <input type="text" placeholder="Giro o Actividad">
            <input type="date" placeholder="Fecha de Constitución o Inicio">
        </section>

        <!-- Documentos Legales -->
        <section>
            <h2>Documentos Legales y Fiscales</h2>
            <textarea placeholder="Descripción de documentos legales recibidos (Acta Constitutiva, Opinión de Cumplimiento, etc.)"></textarea>
            <input type="file" multiple>
        </section>

        <!-- Cuentas Bancarias -->
        <section>
            <h2>Cuentas Bancarias</h2>
            <textarea placeholder="Lista de cuentas bancarias, CLABE, bancos, tipo de cuenta"></textarea>
            <input type="file" placeholder="Subir estados de cuenta">
        </section>

        <!-- Facturación e Ingresos -->
        <section>
            <h2>Facturación e Ingresos</h2>
            <textarea placeholder="Lista de clientes y facturas emitidas"></textarea>
            <input type="file" placeholder="Subir XML y PDF de facturas">
        </section>

        <!-- Gastos y Egresos -->
        <section>
            <h2>Gastos y Egresos</h2>
            <textarea placeholder="Lista de proveedores, facturas recibidas y pagos"></textarea>
            <input type="file" placeholder="Subir comprobantes de pago">
        </section>

        <!-- Impuestos y Obligaciones -->
        <section>
            <h2>Impuestos y Obligaciones Fiscales</h2>
            <textarea placeholder="Historial de declaraciones presentadas y pagos de impuestos"></textarea>
        </section>

        <!-- Inventario -->
        <section>
            <h2>Inventario</h2>
            <textarea placeholder="Registro de inventarios y movimientos"></textarea>
            <input type="file" placeholder="Subir inventario actual">
        </section>

        <!-- Activos Fijos -->
        <section>
            <h2>Activos Fijos</h2>
            <textarea placeholder="Registro de activos fijos y depreciaciones"></textarea>
            <input type="file" placeholder="Subir comprobantes de compra">
        </section>

        <!-- Nómina y Personal -->
        <section>
            <h2>Nómina y Personal</h2>
            <textarea placeholder="Relación de empleados, contratos y pagos"></textarea>
            <input type="file" placeholder="Subir recibos de nómina">
        </section>

        <!-- Financiamientos y Deudas -->
        <section>
            <h2>Financiamientos y Deudas</h2>
            <textarea placeholder="Créditos vigentes, pagos y condiciones"></textarea>
        </section>

        <!-- Presupuestos y Proyecciones -->
        <section>
            <h2>Presupuestos y Proyecciones</h2>
            <textarea placeholder="Presupuesto anual y flujo de efectivo proyectado"></textarea>
        </section>

        <!-- Reportes -->
        <section>
            <h2>Reportes Financieros</h2>
            <button onclick="alert('Generar Reporte Financiero')">Generar Reporte</button>
        </section>

    </div>

</body>

</html>
