<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ContaMex - Plataforma Integral para Contadores</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css">
    <style>
         :root {
            --primary-color: #2c3e50;
            --secondary-color: #3498db;
            --success-color: #27ae60;
            --danger-color: #e74c3c;
        }
        
        body {
            background: #f4f6f9;
        }
        
        .sidebar {
            background: var(--primary-color);
            color: #fff;
            min-height: 100vh;
            padding: 20px;
        }
        
        .sidebar a {
            color: #fff;
            text-decoration: none;
            padding: 10px 15px;
            display: block;
            margin-bottom: 5px;
        }
        
        .sidebar a:hover {
            background: #34495e;
        }
        
        .content {
            padding: 20px;
        }
        
        .section-header {
            margin-top: 30px;
            margin-bottom: 15px;
            border-bottom: 2px solid #ccc;
            padding-bottom: 5px;
        }
        
        .card {
            margin-bottom: 20px;
        }
    </style>
</head>

<body>
    <div class="container-fluid">
        <div class="row">
            <!-- Sidebar de Navegación -->
            <nav class="col-md-3 sidebar">
                <h3>ContaMex</h3>
                <ul class="nav flex-column">
                    <li><a href="#dashboard"><i class="fas fa-tachometer-alt me-2"></i>Dashboard</a></li>
                    <li><a href="#datos-fiscales"><i class="fas fa-id-card me-2"></i>Datos Fiscales</a></li>
                    <li><a href="#clientes-proveedores"><i class="fas fa-address-book me-2"></i>Clientes/Proveedores</a></li>
                    <li><a href="#datos-bancarios"><i class="fas fa-university me-2"></i>Datos Bancarios</a></li>
                    <li><a href="#facturacion-cfdi"><i class="fas fa-file-invoice me-2"></i>Facturación CFDI</a></li>
                    <li><a href="#calculos-fiscales"><i class="fas fa-calculator me-2"></i>Cálculos Fiscales</a></li>
                    <li><a href="#nomina"><i class="fas fa-users me-2"></i>Nómina</a></li>
                    <li><a href="#estados-financieros"><i class="fas fa-chart-line me-2"></i>Estados Financieros</a></li>
                    <li><a href="#declaraciones-fiscales"><i class="fas fa-file-alt me-2"></i>Declaraciones Fiscales</a></li>
                    <li><a href="#depreciacion"><i class="fas fa-tools me-2"></i>Depreciación/Amortización</a></li>
                    <li><a href="#cuentas-por-cobrar"><i class="fas fa-hand-holding-usd me-2"></i>Cuentas por Cobrar/Pagar</a></li>
                    <li><a href="#coeficiente-utilidad"><i class="fas fa-percentage me-2"></i>Coeficiente de Utilidad</a></li>
                    <li><a href="#control-documental"><i class="fas fa-folder-open me-2"></i>Control Documental</a></li>
                    <li><a href="#kpi"><i class="fas fa-bullseye me-2"></i>KPI</a></li>
                    <li><a href="#alertas"><i class="fas fa-exclamation-triangle me-2"></i>Alertas Inteligentes</a></li>
                    <li><a href="#control-interno"><i class="fas fa-shield-alt me-2"></i>Control Interno</a></li>
                    <li><a href="#isr-dividendos"><i class="fas fa-coins me-2"></i>ISR Dividendos</a></li>
                    <li><a href="#reportes-especificos"><i class="fas fa-chart-pie me-2"></i>Reportes Específicos</a></li>
                    <li><a href="#multas-recargos"><i class="fas fa-exclamation-circle me-2"></i>Multas y Recargos</a></li>
                    <li><a href="#utilidad-fiscal"><i class="fas fa-money-bill-wave me-2"></i>Utilidad Fiscal Neta</a></li>
                    <li><a href="#automatizacion-conciliacion"><i class="fas fa-sync-alt me-2"></i>Automatización/Conciliación</a></li>
                </ul>
            </nav>

            <!-- Contenido Principal -->
            <div class="col-md-9 content">
                <h1 class="mb-4">Plataforma Integral para Contadores - ContaMex</h1>

                <!-- Dashboard -->
                <section id="dashboard">
                    <h2 class="section-header">Dashboard</h2>
                    <div class="row">
                        <div class="col-md-3">
                            <div class="card">
                                <div class="card-body">
                                    <h5 class="card-title">Ventas Mensuales</h5>
                                    <p class="h3">$152,450 MXN</p>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="card">
                                <div class="card-body">
                                    <h5 class="card-title">Facturas Pendientes</h5>
                                    <p class="h3">15</p>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="card">
                                <div class="card-body">
                                    <h5 class="card-title">Gastos del Mes</h5>
                                    <p class="h3">$89,300 MXN</p>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-3">
                            <div class="card">
                                <div class="card-body">
                                    <h5 class="card-title">Próximo Vencimiento</h5>
                                    <p class="h3">15 días</p>
                                    <small>Declaración IVA</small>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-md-6">
                            <div class="card mb-3">
                                <div class="card-body">
                                    <h5 class="card-title">Flujo de Efectivo</h5>
                                    <canvas id="chartFlujoEfectivo"></canvas>
                                </div>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <div class="card mb-3">
                                <div class="card-body">
                                    <h5 class="card-title">Distribución de Gastos</h5>
                                    <canvas id="chartGastos"></canvas>
                                </div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- 1. Datos Fiscales y de Identificación -->
                <section id="datos-fiscales">
                    <h2 class="section-header">Datos Fiscales y de Identificación</h2>
                    <form>
                        <div class="mb-3">
                            <label for="rfcEmpresa" class="form-label">RFC</label>
                            <input type="text" class="form-control" id="rfcEmpresa" placeholder="Ej. ABC123456789">
                        </div>
                        <div class="mb-3">
                            <label for="curpEmpresa" class="form-label">CURP (si aplica)</label>
                            <input type="text" class="form-control" id="curpEmpresa" placeholder="Ej. ABCD123456HDFLLL09">
                        </div>
                        <div class="mb-3">
                            <label for="nombreEmpresa" class="form-label">Nombre o Razón Social</label>
                            <input type="text" class="form-control" id="nombreEmpresa" placeholder="Nombre de la empresa">
                        </div>
                        <div class="mb-3">
                            <label for="regimenFiscal" class="form-label">Régimen Fiscal</label>
                            <select class="form-select" id="regimenFiscal">
                <option value="RESICO">RESICO</option>
                <option value="RIF">RIF</option>
                <option value="General">General de Ley</option>
              </select>
                        </div>
                        <div class="mb-3">
                            <label for="direccionFiscal" class="form-label">Dirección Fiscal</label>
                            <textarea class="form-control" id="direccionFiscal" rows="2"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="csd" class="form-label">Certificado de Sello Digital (CSD)</label>
                            <input type="file" class="form-control" id="csd">
                        </div>
                        <div class="mb-3">
                            <label for="obligacionesSAT" class="form-label">Obligaciones Fiscales SAT</label>
                            <textarea class="form-control" id="obligacionesSAT" rows="2" placeholder="Lista de obligaciones fiscales..."></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Datos Fiscales</button>
                    </form>
                </section>

                <!-- 2. Datos de Clientes y Proveedores -->
                <section id="clientes-proveedores">
                    <h2 class="section-header">Datos de Clientes y Proveedores</h2>
                    <form>
                        <div class="mb-3">
                            <label for="rfcCliente" class="form-label">RFC</label>
                            <input type="text" class="form-control" id="rfcCliente" placeholder="Ej. XYZ987654321">
                        </div>
                        <div class="mb-3">
                            <label for="razonSocial" class="form-label">Razón Social</label>
                            <input type="text" class="form-control" id="razonSocial" placeholder="Nombre o Razón Social">
                        </div>
                        <div class="mb-3">
                            <label for="tipoRelacion" class="form-label">Tipo de Relación</label>
                            <select class="form-select" id="tipoRelacion">
                <option value="cliente">Cliente</option>
                <option value="proveedor">Proveedor</option>
              </select>
                        </div>
                        <div class="mb-3">
                            <label for="direccionContacto" class="form-label">Dirección</label>
                            <input type="text" class="form-control" id="direccionContacto" placeholder="Dirección">
                        </div>
                        <div class="mb-3">
                            <label for="contactoNombre" class="form-label">Nombre de Contacto</label>
                            <input type="text" class="form-control" id="contactoNombre" placeholder="Nombre">
                        </div>
                        <div class="mb-3">
                            <label for="contactoCorreo" class="form-label">Correo Electrónico</label>
                            <input type="email" class="form-control" id="contactoCorreo" placeholder="ejemplo@dominio.com">
                        </div>
                        <div class="mb-3">
                            <label for="contactoTelefono" class="form-label">Teléfono</label>
                            <input type="tel" class="form-control" id="contactoTelefono" placeholder="Número de teléfono">
                        </div>
                        <div class="mb-3">
                            <label for="historialPagos" class="form-label">Historial de Pagos y Facturación</label>
                            <textarea class="form-control" id="historialPagos" rows="3" placeholder="Detalles del historial..."></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Cliente/Proveedor</button>
                    </form>
                </section>

                <!-- 3. Datos Bancarios -->
                <section id="datos-bancarios">
                    <h2 class="section-header">Datos Bancarios</h2>
                    <form>
                        <div class="mb-3">
                            <label for="numeroCuenta" class="form-label">Número de Cuenta</label>
                            <input type="text" class="form-control" id="numeroCuenta" placeholder="Número de cuenta">
                        </div>
                        <div class="mb-3">
                            <label for="banco" class="form-label">Banco</label>
                            <input type="text" class="form-control" id="banco" placeholder="Nombre del banco">
                        </div>
                        <div class="mb-3">
                            <label for="clabe" class="form-label">CLABE</label>
                            <input type="text" class="form-control" id="clabe" placeholder="CLABE interbancaria">
                        </div>
                        <div class="mb-3">
                            <label for="saldoInicial" class="form-label">Saldo Inicial</label>
                            <input type="number" class="form-control" id="saldoInicial" placeholder="Saldo inicial">
                        </div>
                        <div class="mb-3">
                            <label for="saldoFinal" class="form-label">Saldo Final</label>
                            <input type="number" class="form-control" id="saldoFinal" placeholder="Saldo final">
                        </div>
                        <div class="mb-3">
                            <label for="movimientosBancarios" class="form-label">Movimientos Bancarios</label>
                            <textarea class="form-control" id="movimientosBancarios" rows="3" placeholder="Detalles de movimientos bancarios..."></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="conciliacionBancaria" class="form-label">Conciliación Bancaria</label>
                            <textarea class="form-control" id="conciliacionBancaria" rows="2" placeholder="Detalles de conciliación bancaria..."></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Datos Bancarios</button>
                    </form>
                </section>

                <!-- 4. Facturación CFDI 4.0 -->
                <section id="facturacion-cfdi">
                    <h2 class="section-header">Facturación CFDI 4.0</h2>
                    <form>
                        <div class="mb-3">
                            <label for="serieFolio" class="form-label">Serie y Folio</label>
                            <input type="text" class="form-control" id="serieFolio" placeholder="Ej. A-001">
                        </div>
                        <div class="mb-3">
                            <label for="uuid" class="form-label">UUID (Folio Fiscal)</label>
                            <input type="text" class="form-control" id="uuid" placeholder="UUID">
                        </div>
                        <div class="mb-3">
                            <label for="fechaEmision" class="form-label">Fecha de Emisión</label>
                            <input type="date" class="form-control" id="fechaEmision">
                        </div>
                        <div class="mb-3">
                            <label for="fechaPago" class="form-label">Fecha de Pago (PPD)</label>
                            <input type="date" class="form-control" id="fechaPago">
                        </div>
                        <div class="mb-3">
                            <label for="tipoComprobante" class="form-label">Tipo de Comprobante</label>
                            <select class="form-select" id="tipoComprobante">
                <option value="I">Ingreso</option>
                <option value="E">Egreso</option>
                <option value="P">Pago</option>
              </select>
                        </div>
                        <div class="mb-3">
                            <label for="usoCfdi" class="form-label">Uso del CFDI</label>
                            <input type="text" class="form-control" id="usoCfdi" placeholder="Ej. G01">
                        </div>
                        <div class="mb-3">
                            <label for="metodoPago" class="form-label">Método de Pago</label>
                            <select class="form-select" id="metodoPago">
                <option value="PUE">PUE</option>
                <option value="PPD">PPD</option>
              </select>
                        </div>
                        <div class="mb-3">
                            <label for="formaPago" class="form-label">Forma de Pago</label>
                            <input type="text" class="form-control" id="formaPago" placeholder="Ej. Transferencia">
                        </div>
                        <div class="mb-3">
                            <label for="subtotal" class="form-label">Subtotal</label>
                            <input type="number" class="form-control" id="subtotal" placeholder="Subtotal">
                        </div>
                        <div class="mb-3">
                            <label for="descuento" class="form-label">Descuento</label>
                            <input type="number" class="form-control" id="descuento" placeholder="Descuento">
                        </div>
                        <div class="mb-3">
                            <label for="iva" class="form-label">IVA</label>
                            <input type="number" class="form-control" id="iva" placeholder="IVA">
                        </div>
                        <div class="mb-3">
                            <label for="retenciones" class="form-label">Retenciones (ISR, IVA)</label>
                            <input type="number" class="form-control" id="retenciones" placeholder="Retenciones">
                        </div>
                        <div class="mb-3">
                            <label for="totalCfdi" class="form-label">Total</label>
                            <input type="number" class="form-control" id="totalCfdi" placeholder="Total">
                        </div>
                        <div class="mb-3">
                            <label for="moneda" class="form-label">Moneda</label>
                            <input type="text" class="form-control" id="moneda" placeholder="Ej. MXN">
                        </div>
                        <div class="mb-3">
                            <label for="tipoCambio" class="form-label">Tipo de Cambio</label>
                            <input type="number" step="0.01" class="form-control" id="tipoCambio" placeholder="Tipo de cambio">
                        </div>
                        <div class="mb-3">
                            <label for="metodoCancelacion" class="form-label">Método de Cancelación</label>
                            <input type="text" class="form-control" id="metodoCancelacion" placeholder="Método de cancelación">
                        </div>
                        <div class="mb-3">
                            <label for="relacionCfdi" class="form-label">Relación con otros CFDI</label>
                            <input type="text" class="form-control" id="relacionCfdi" placeholder="Notas de crédito, devoluciones, etc.">
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Facturación CFDI</button>
                    </form>
                </section>

                <!-- 5. Cálculos Fiscales -->
                <section id="calculos-fiscales">
                    <h2 class="section-header">Cálculos Fiscales</h2>
                    <!-- ISR -->
                    <div class="mb-4">
                        <h5>ISR Mensual (Personas Morales y Físicas)</h5>
                        <ul>
                            <li>Ingresos acumulables</li>
                            <li>Deducciones autorizadas</li>
                            <li>Base gravable</li>
                            <li>ISR causado</li>
                            <li>Pagos provisionales realizados</li>
                            <li>ISR a pagar</li>
                        </ul>
                        <form>
                            <div class="mb-3">
                                <label for="ingresos" class="form-label">Ingresos Acumulables</label>
                                <input type="number" class="form-control" id="ingresos" placeholder="Ingresos">
                            </div>
                            <div class="mb-3">
                                <label for="deducciones" class="form-label">Deducciones Autorizadas</label>
                                <input type="number" class="form-control" id="deducciones" placeholder="Deducciones">
                            </div>
                            <div class="mb-3">
                                <label for="pagosProvisionales" class="form-label">Pagos Provisionales</label>
                                <input type="number" class="form-control" id="pagosProvisionales" placeholder="Pagos provisionales">
                            </div>
                            <button type="button" class="btn btn-secondary" onclick="calcularISR()">Calcular ISR</button>
                            <p id="resultadoISR" class="mt-2"></p>
                        </form>
                    </div>
                    <!-- IVA -->
                    <div class="mb-4">
                        <h5>IVA</h5>
                        <ul>
                            <li>IVA trasladado (ventas)</li>
                            <li>IVA acreditable (gastos)</li>
                            <li>IVA a pagar o a favor</li>
                        </ul>
                        <form>
                            <div class="mb-3">
                                <label for="ivaTrasladado" class="form-label">IVA Trasladado</label>
                                <input type="number" class="form-control" id="ivaTrasladado" placeholder="IVA trasladado">
                            </div>
                            <div class="mb-3">
                                <label for="ivaAcreditable" class="form-label">IVA Acreditable</label>
                                <input type="number" class="form-control" id="ivaAcreditable" placeholder="IVA acreditable">
                            </div>
                            <button type="button" class="btn btn-secondary" onclick="calcularIVA()">Calcular IVA</button>
                            <p id="resultadoIVA" class="mt-2"></p>
                        </form>
                    </div>
                    <!-- Retenciones -->
                    <div class="mb-4">
                        <h5>Retenciones</h5>
                        <ul>
                            <li>ISR retenido por honorarios o arrendamiento</li>
                            <li>IVA retenido (3era parte)</li>
                        </ul>
                        <form>
                            <div class="mb-3">
                                <label for="isrRetenido" class="form-label">ISR Retenido</label>
                                <input type="number" class="form-control" id="isrRetenido" placeholder="ISR retenido">
                            </div>
                            <div class="mb-3">
                                <label for="ivaRetenido" class="form-label">IVA Retenido</label>
                                <input type="number" class="form-control" id="ivaRetenido" placeholder="IVA retenido">
                            </div>
                            <button type="button" class="btn btn-secondary" onclick="calcularRetenciones()">Calcular Retenciones</button>
                            <p id="resultadoRetenciones" class="mt-2"></p>
                        </form>
                    </div>
                    <!-- DIOT y PTU (simplificados) -->
                    <div class="mb-4">
                        <h5>Cálculo DIOT</h5>
                        <ul>
                            <li>Proveedores nacionales y extranjeros</li>
                            <li>Proveedores con retención</li>
                            <li>Pagos en efectivo y en cheque</li>
                            <li>IVA pagado y retenido</li>
                        </ul>
                        <button class="btn btn-secondary">Generar DIOT</button>
                    </div>
                    <div class="mb-4">
                        <h5>PTU (Participación de los Trabajadores en las Utilidades)</h5>
                        <ul>
                            <li>Utilidad fiscal</li>
                            <li>Base para PTU</li>
                            <li>Reparto entre empleados</li>
                        </ul>
                        <button class="btn btn-secondary">Calcular PTU</button>
                    </div>
                </section>

                <!-- 6. Cálculos Laborales (Nómina) -->
                <section id="nomina">
                    <h2 class="section-header">Cálculos Laborales (Nómina)</h2>
                    <form>
                        <div class="mb-3">
                            <label for="salarioBase" class="form-label">Salario Base</label>
                            <input type="number" class="form-control" id="salarioBase" placeholder="Salario base">
                        </div>
                        <div class="mb-3">
                            <label for="sdi" class="form-label">Salario Diario Integrado (SDI)</label>
                            <input type="number" class="form-control" id="sdi" placeholder="SDI">
                        </div>
                        <div class="mb-3">
                            <label for="percepciones" class="form-label">Percepciones Ordinarias/Extraordinarias</label>
                            <textarea class="form-control" id="percepciones" rows="2"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="deduccionesNomina" class="form-label">Deducciones (ISR, IMSS, etc.)</label>
                            <textarea class="form-control" id="deduccionesNomina" rows="2"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="cuotasPatronales" class="form-label">Cuotas Patronales</label>
                            <input type="number" class="form-control" id="cuotasPatronales" placeholder="Cuotas patronales">
                        </div>
                        <div class="mb-3">
                            <label for="subsidioEmpleo" class="form-label">Subsidio al Empleo</label>
                            <input type="number" class="form-control" id="subsidioEmpleo" placeholder="Subsidio al empleo">
                        </div>
                        <div class="mb-3">
                            <label for="aguinaldo" class="form-label">Aguinaldo</label>
                            <input type="number" class="form-control" id="aguinaldo" placeholder="Aguinaldo">
                        </div>
                        <div class="mb-3">
                            <label for="primaVacacional" class="form-label">Prima Vacacional</label>
                            <input type="number" class="form-control" id="primaVacacional" placeholder="Prima vacacional">
                        </div>
                        <div class="mb-3">
                            <label for="finiquito" class="form-label">Finiquito / Liquidación</label>
                            <input type="number" class="form-control" id="finiquito" placeholder="Finiquito">
                        </div>
                        <div class="mb-3">
                            <label for="reporteSUA" class="form-label">Reporte SUA (IMSS)</label>
                            <input type="file" class="form-control" id="reporteSUA">
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Datos de Nómina</button>
                    </form>
                </section>

                <!-- 7. Estados Financieros -->
                <section id="estados-financieros">
                    <h2 class="section-header">Estados Financieros</h2>
                    <h5>Balance General</h5>
                    <form>
                        <div class="mb-3">
                            <label for="activos" class="form-label">Activos (Circulantes, Fijos, Intangibles)</label>
                            <textarea class="form-control" id="activos" rows="2" placeholder="Detalles de activos"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="pasivos" class="form-label">Pasivos (Corto y Largo Plazo)</label>
                            <textarea class="form-control" id="pasivos" rows="2" placeholder="Detalles de pasivos"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="capitalContable" class="form-label">Capital Contable</label>
                            <input type="number" class="form-control" id="capitalContable" placeholder="Capital contable">
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Balance General</button>
                    </form>
                    <h5 class="mt-4">Estado de Resultados</h5>
                    <form>
                        <div class="mb-3">
                            <label for="ingresosResultados" class="form-label">Ingresos</label>
                            <input type="number" class="form-control" id="ingresosResultados" placeholder="Ingresos">
                        </div>
                        <div class="mb-3">
                            <label for="costoVentas" class="form-label">Costo de Ventas</label>
                            <input type="number" class="form-control" id="costoVentas" placeholder="Costo de ventas">
                        </div>
                        <div class="mb-3">
                            <label for="gastosOperacion" class="form-label">Gastos de Operación</label>
                            <textarea class="form-control" id="gastosOperacion" rows="2" placeholder="Gastos de operación"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="utilidadAntesImpuestos" class="form-label">Utilidad o Pérdida antes de Impuestos</label>
                            <input type="number" class="form-control" id="utilidadAntesImpuestos" placeholder="Utilidad o pérdida">
                        </div>
                        <div class="mb-3">
                            <label for="impuestosPagar" class="form-label">Impuestos a Pagar</label>
                            <input type="number" class="form-control" id="impuestosPagar" placeholder="Impuestos a pagar">
                        </div>
                        <div class="mb-3">
                            <label for="utilidadNeta" class="form-label">Utilidad o Pérdida Neta</label>
                            <input type="number" class="form-control" id="utilidadNeta" placeholder="Utilidad o pérdida neta">
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Estado de Resultados</button>
                    </form>
                    <h5 class="mt-4">Flujo de Efectivo</h5>
                    <form>
                        <div class="mb-3">
                            <label for="operaciones" class="form-label">Operaciones</label>
                            <textarea class="form-control" id="operaciones" rows="2" placeholder="Flujo de efectivo de operaciones"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="inversiones" class="form-label">Inversiones</label>
                            <textarea class="form-control" id="inversiones" rows="2" placeholder="Flujo de efectivo de inversiones"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="financiamiento" class="form-label">Financiamiento</label>
                            <textarea class="form-control" id="financiamiento" rows="2" placeholder="Flujo de efectivo de financiamiento"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Flujo de Efectivo</button>
                    </form>
                </section>

                <!-- 8. Declaraciones Fiscales -->
                <section id="declaraciones-fiscales">
                    <h2 class="section-header">Declaraciones Fiscales</h2>
                    <ul>
                        <li>Declaración Mensual (ISR, IVA)</li>
                        <li>Declaración Anual</li>
                        <li>Declaración de Pagos Definitivos (Retenciones)</li>
                        <li>Declaración Informativa de Operaciones con Terceros (DIOT)</li>
                        <li>Declaración de Operaciones Relevantes (RMF 2.8.1.16)</li>
                    </ul>
                    <button class="btn btn-secondary">Generar Declaraciones Fiscales</button>
                </section>

                <!-- 9. Depreciación y Amortización -->
                <section id="depreciacion">
                    <h2 class="section-header">Depreciación y Amortización</h2>
                    <form>
                        <div class="mb-3">
                            <label for="activoFijo" class="form-label">Activo Fijo</label>
                            <input type="text" class="form-control" id="activoFijo" placeholder="Nombre del activo">
                        </div>
                        <div class="mb-3">
                            <label for="costoActivo" class="form-label">Costo del Activo</label>
                            <input type="number" class="form-control" id="costoActivo" placeholder="Costo">
                        </div>
                        <div class="mb-3">
                            <label for="vidaUtil" class="form-label">Vida Útil (en años)</label>
                            <input type="number" class="form-control" id="vidaUtil" placeholder="Vida útil">
                        </div>
                        <div class="mb-3">
                            <label for="valorResidual" class="form-label">Valor Residual</label>
                            <input type="number" class="form-control" id="valorResidual" placeholder="Valor residual">
                        </div>
                        <button type="submit" class="btn btn-primary">Calcular Depreciación</button>
                    </form>
                </section>

                <!-- 10. Control de Cuentas por Cobrar y Pagar -->
                <section id="cuentas-por-cobrar">
                    <h2 class="section-header">Control de Cuentas por Cobrar y Pagar</h2>
                    <form>
                        <div class="mb-3">
                            <label for="clientesSaldos" class="form-label">Clientes y Saldos</label>
                            <textarea class="form-control" id="clientesSaldos" rows="2" placeholder="Listado de clientes y sus saldos"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="antiguedadSaldos" class="form-label">Antigüedad de Saldos</label>
                            <textarea class="form-control" id="antiguedadSaldos" rows="2" placeholder="Detalles de antigüedad"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="proveedoresAdeudos" class="form-label">Proveedores y Adeudos</label>
                            <textarea class="form-control" id="proveedoresAdeudos" rows="2" placeholder="Listado de proveedores y adeudos"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="pagosProgramados" class="form-label">Pagos Programados</label>
                            <textarea class="form-control" id="pagosProgramados" rows="2" placeholder="Detalles de pagos programados"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Control de Cuentas</button>
                    </form>
                </section>

                <!-- 11. Cálculo de Coeficiente de Utilidad -->
                <section id="coeficiente-utilidad">
                    <h2 class="section-header">Cálculo de Coeficiente de Utilidad</h2>
                    <form>
                        <div class="mb-3">
                            <label for="ingresosNominales" class="form-label">Ingresos Nominales</label>
                            <input type="number" class="form-control" id="ingresosNominales" placeholder="Ingresos">
                        </div>
                        <div class="mb-3">
                            <label for="deduccionesAutorizadas" class="form-label">Deducciones Autorizadas</label>
                            <input type="number" class="form-control" id="deduccionesAutorizadas" placeholder="Deducciones">
                        </div>
                        <div class="mb-3">
                            <label for="baseGravable" class="form-label">Base Gravable</label>
                            <input type="number" class="form-control" id="baseGravable" placeholder="Base gravable">
                        </div>
                        <button type="submit" class="btn btn-primary">Calcular Coeficiente de Utilidad</button>
                    </form>
                </section>

                <!-- 12. Control Documental -->
                <section id="control-documental">
                    <h2 class="section-header">Control Documental</h2>
                    <form>
                        <div class="mb-3">
                            <label for="xmlCfdi" class="form-label">Almacenamiento de XML de CFDI</label>
                            <input type="file" class="form-control" id="xmlCfdi" accept=".xml">
                        </div>
                        <div class="mb-3">
                            <label for="pdfDocumentos" class="form-label">Almacenamiento de PDF (facturas, contratos, etc.)</label>
                            <input type="file" class="form-control" id="pdfDocumentos" accept=".pdf">
                        </div>
                        <div class="mb-3">
                            <label for="expedientesDigitales" class="form-label">Expedientes Digitales (Cliente/Proveedor)</label>
                            <textarea class="form-control" id="expedientesDigitales" rows="2" placeholder="Detalles de expedientes"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Control Documental</button>
                    </form>
                </section>

                <!-- 13. Indicadores Clave (KPI) -->
                <section id="kpi">
                    <h2 class="section-header">Indicadores Clave (KPI) para Contadores</h2>
                    <form>
                        <div class="mb-3">
                            <label for="margenUtilidad" class="form-label">Margen de Utilidad (%)</label>
                            <input type="number" class="form-control" id="margenUtilidad" placeholder="Margen de utilidad">
                        </div>
                        <div class="mb-3">
                            <label for="rotacionCuentas" class="form-label">Índice de Rotación de Cuentas por Cobrar</label>
                            <input type="number" class="form-control" id="rotacionCuentas" placeholder="Índice de rotación">
                        </div>
                        <div class="mb-3">
                            <label for="diasCuentasPagar" class="form-label">Días de Cuentas por Pagar</label>
                            <input type="number" class="form-control" id="diasCuentasPagar" placeholder="Días">
                        </div>
                        <div class="mb-3">
                            <label for="razonLiquidez" class="form-label">Razón de Liquidez</label>
                            <input type="number" class="form-control" id="razonLiquidez" placeholder="Razón de liquidez">
                        </div>
                        <div class="mb-3">
                            <label for="puntoEquilibrio" class="form-label">Punto de Equilibrio</label>
                            <input type="number" class="form-control" id="puntoEquilibrio" placeholder="Punto de equilibrio">
                        </div>
                        <div class="mb-3">
                            <label for="ebitda" class="form-label">EBITDA</label>
                            <input type="number" class="form-control" id="ebitda" placeholder="EBITDA">
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar KPIs</button>
                    </form>
                </section>

                <!-- 14. Alertas Inteligentes -->
                <section id="alertas">
                    <h2 class="section-header">Alertas Inteligentes</h2>
                    <ul>
                        <li>Vencimiento de obligaciones fiscales</li>
                        <li>Pagos próximos a vencer</li>
                        <li>Diferencias en conciliaciones</li>
                        <li>Discrepancias fiscales</li>
                        <li>Clientes con adeudos prolongados</li>
                        <li>Proveedores con incumplimiento de entrega</li>
                    </ul>
                    <button class="btn btn-warning">Generar Alertas</button>
                </section>

                <!-- 15. Información de Control Interno -->
                <section id="control-interno">
                    <h2 class="section-header">Información de Control Interno</h2>
                    <form>
                        <div class="mb-3">
                            <label for="politicasGastos" class="form-label">Políticas de Gastos</label>
                            <textarea class="form-control" id="politicasGastos" rows="2" placeholder="Detalle las políticas de gastos"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="presupuestos" class="form-label">Presupuestos por Área</label>
                            <textarea class="form-control" id="presupuestos" rows="2" placeholder="Detalle los presupuestos"></textarea>
                        </div>
                        <div class="mb-3">
                            <label for="flujosAutorizacion" class="form-label">Flujos de Autorización</label>
                            <textarea class="form-control" id="flujosAutorizacion" rows="2" placeholder="Detalle los flujos de autorización"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Guardar Control Interno</button>
                    </form>
                </section>

                <!-- 16. Cálculo de ISR por Dividendos -->
                <section id="isr-dividendos">
                    <h2 class="section-header">Cálculo de ISR por Dividendos</h2>
                    <form>
                        <div class="mb-3">
                            <label for="utilidadFiscalNeta" class="form-label">Utilidad Fiscal Neta</label>
                            <input type="number" class="form-control" id="utilidadFiscalNeta" placeholder="Utilidad fiscal neta">
                        </div>
                        <div class="mb-3">
                            <label for="cufin" class="form-label">CUFIN</label>
                            <input type="number" class="form-control" id="cufin" placeholder="CUFIN">
                        </div>
                        <button type="submit" class="btn btn-primary">Calcular ISR Dividendos</button>
                    </form>
                </section>

                <!-- 17. Reportes Específicos -->
                <section id="reportes-especificos">
                    <h2 class="section-header">Reportes Específicos</h2>
                    <ul>
                        <li>Reporte de ingresos por cliente</li>
                        <li>Reporte de gastos por categoría</li>
                        <li>Reporte de impuestos pagados</li>
                        <li>Reporte de nómina y seguridad social</li>
                        <li>Reporte de flujo de efectivo proyectado</li>
                    </ul>
                    <button class="btn btn-secondary">Generar Reportes</button>
                </section>

                <!-- 18. Cálculo de Multas y Recargos -->
                <section id="multas-recargos">
                    <h2 class="section-header">Cálculo de Multas y Recargos</h2>
                    <form>
                        <div class="mb-3">
                            <label for="multas" class="form-label">Multas por Declaraciones Fuera de Plazo</label>
                            <input type="number" class="form-control" id="multas" placeholder="Multas">
                        </div>
                        <div class="mb-3">
                            <label for="recargos" class="form-label">Recargos por Pagos Extemporáneos</label>
                            <input type="number" class="form-control" id="recargos" placeholder="Recargos">
                        </div>
                        <div class="mb-3">
                            <label for="actualizacionInflacion" class="form-label">Actualización por Inflación (INPC)</label>
                            <input type="number" class="form-control" id="actualizacionInflacion" placeholder="Actualización por inflación">
                        </div>
                        <button type="submit" class="btn btn-primary">Calcular Multas y Recargos</button>
                    </form>
                </section>

                <!-- 19. Cálculo de Utilidad Fiscal Neta -->
                <section id="utilidad-fiscal">
                    <h2 class="section-header">Cálculo de Utilidad Fiscal Neta</h2>
                    <form>
                        <div class="mb-3">
                            <label for="utilidadAntesImpuestosUF" class="form-label">Utilidad Antes de Impuestos</label>
                            <input type="number" class="form-control" id="utilidadAntesImpuestosUF" placeholder="Utilidad antes de impuestos">
                        </div>
                        <div class="mb-3">
                            <label for="ajustesFiscales" class="form-label">Ajustes Fiscales (No deducibles, etc.)</label>
                            <textarea class="form-control" id="ajustesFiscales" rows="2" placeholder="Detalle de ajustes fiscales"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Calcular Utilidad Fiscal Neta</button>
                    </form>
                </section>

                <!-- 20. Automatización y Conciliación -->
                <section id="automatizacion-conciliacion">
                    <h2 class="section-header">Automatización y Conciliación</h2>
                    <ul>
                        <li>Importación masiva de CFDI (XML)</li>
                        <li>Descarga automática desde SAT</li>
                        <li>Conciliación bancaria automática</li>
                        <li>Validación de XML (estructura y requisitos fiscales)</li>
                    </ul>
                    <button class="btn btn-secondary">Iniciar Automatización</button>
                </section>

            </div>
            <!-- Fin Contenido Principal -->
        </div>
    </div>

    <!-- Scripts -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Cálculo ISR (ejemplo simplificado)
        function calcularISR() {
            const ingresos = parseFloat(document.getElementById('ingresos').value) || 0;
            const deducciones = parseFloat(document.getElementById('deducciones').value) || 0;
            const pagosProvisionales = parseFloat(document.getElementById('pagosProvisionales').value) || 0;
            const baseGravable = ingresos - deducciones;
            const isrCausado = baseGravable * 0.30; // Supuesto 30%
            const isrAPagar = isrCausado - pagosProvisionales;
            document.getElementById('resultadoISR').textContent = 'ISR a Pagar: $' + isrAPagar.toFixed(2);
        }

        // Cálculo IVA (ejemplo simplificado)
        function calcularIVA() {
            const ivaTrasladado = parseFloat(document.getElementById('ivaTrasladado').value) || 0;
            const ivaAcreditable = parseFloat(document.getElementById('ivaAcreditable').value) || 0;
            const ivaAPagar = ivaTrasladado - ivaAcreditable;
            document.getElementById('resultadoIVA').textContent = 'IVA a Pagar: $' + ivaAPagar.toFixed(2);
        }

        // Cálculo de Retenciones (ejemplo simplificado)
        function calcularRetenciones() {
            const isrRetenido = parseFloat(document.getElementById('isrRetenido').value) || 0;
            const ivaRetenido = parseFloat(document.getElementById('ivaRetenido').value) || 0;
            const totalRetenciones = isrRetenido + ivaRetenido;
            document.getElementById('resultadoRetenciones').textContent = 'Total Retenciones: $' + totalRetenciones.toFixed(2);
        }

        // Gráfica de Flujo de Efectivo
        const ctxFlujo = document.getElementById('chartFlujoEfectivo').getContext('2d');
        const chartFlujoEfectivo = new Chart(ctxFlujo, {
            type: 'line',
            data: {
                labels: ['Enero', 'Febrero', 'Marzo', 'Abril', 'Mayo'],
                datasets: [{
                    label: 'Flujo de Efectivo',
                    data: [120000, 130000, 125000, 135000, 140000],
                    borderColor: 'rgba(46, 204, 113, 1)',
                    fill: false
                }]
            }
        });

        // Gráfica de Distribución de Gastos
        const ctxGastos = document.getElementById('chartGastos').getContext('2d');
        const chartGastos = new Chart(ctxGastos, {
            type: 'pie',
            data: {
                labels: ['Gastos Operativos', 'Impuestos', 'Salarios', 'Otros'],
                datasets: [{
                    data: [40000, 25000, 30000, 15000],
                    backgroundColor: ['#3498db', '#e74c3c', '#f39c12', '#95a5a6']
                }]
            }
        });
    </script>
</body>

</html>
