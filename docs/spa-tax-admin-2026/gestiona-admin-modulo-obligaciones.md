# Gestión Admin — módulo de obligaciones tributarias y administrativas

## Objetivo
Controlar obligaciones, vencimientos, evidencias y costos de JoinHook SpA. La app no presenta declaraciones ni sustituye al SII o al contador.

## Modelo de datos propuesto

### ObligationTemplate
- id, nombre, autoridad, categoría, periodicidad
- regla de aplicabilidad y fuente oficial
- responsable, criticidad, instrucciones
- fecha de última revisión y estado activa/inactiva con motivo

### ObligationOccurrence
- id, template_id, período/año tributario
- vencimiento configurable y fecha/fuente de verificación
- estado: pendiente, recopilando antecedentes, preparado, en revisión, presentado, pagado, rectificado, no aplica, vencido
- motivo “no aplica”, responsable y bitácora
- importe declarado, pagado y saldo, si corresponde
- fechas de presentación/pago, folio/acuse y evidencia privada

### Expense / RecurringCost
- proveedor, categoría, concepto, fecha, monto, moneda
- frecuencia, próxima fecha, contrato, documento tributario
- costo único/recurrente, medio de pago y comprobante
- IVA del documento, sin calcularlo por suposición

### FilingEvidence
- tipo de documento, período, fecha, versión, ubicación privada
- quién subió y cuándo; permisos de acceso

### CashPlan
- constitución/trámites por cotizar
- oficina virtual: $60.000/año estimados por fundador; equivalente presupuestario $5.000/mes, no pago mensual confirmado
- contabilidad, patente, banco, certificados, herramientas y gastos
- ingresos proyectados separados de ingresos reales
- alertas si caja prevista no cubre próximos compromisos

## Reglas de negocio
1. “Presentado” exige fecha y folio/acuse o evidencia.
2. “Pagado” exige fecha y comprobante.
3. “No aplica” exige motivo, revisor y fecha de próxima revisión.
4. Vencimientos configurables; no codificar un día fijo para todos los casos.
5. Distinguir “sin ventas”, “sin movimiento”, “sin obligación aplicable” y “pendiente de confirmar”.
6. Preparado no equivale a enviado; enviado no equivale a pagado.
7. Rectificaciones conservan historial y evidencia anterior.
8. Registrar quién/cuándo/qué cambió.
9. Exportación CSV/XLSX/JSON para respaldo y revisión contable.
10. Panel: próximos 30 días, vencidas, pendientes de revisión, presentadas sin pago, pagadas y costos recurrentes.
11. No prometer alertas automáticas hasta implementarlas y probarlas.
12. Guardar fuente oficial y fecha de verificación de cada regla.

## Seguridad
- Nunca guardar Clave Tributaria, contraseñas bancarias, certificados privados ni llaves.
- No enviar F29 ni pagos automáticamente en la primera versión.
- Acceso por rol y mínimo privilegio; documentos privados, no repositorio público.
- Validar entradas y archivos, registrar eventos y probar restauración de backups.
- Si usa base de datos, revisar RLS/permisos, autenticación, sesiones, secretos, rate limiting y controles DevSecOps antes de datos reales.

## Fases
- Fase 1: diseño del catálogo, calendario manual, estados y presupuesto.
- Fase 2: CRUD, filtros, exportación y adjuntos privados.
- Fase 3: pruebas con datos ficticios (sin ventas, gastos recurrentes, F29 presentado/pendiente/rectificado/no aplica).
- Fase 4: uso con documentos reales tras controles de acceso y backup.
- Fase 5: automatizaciones opcionales solo tras revisar permisos y fiabilidad.

## Criterios de aceptación
- Crear obligación recurrente y generar períodos.
- Diferenciar preparación, presentación y pago.
- Exigir evidencia para presentado/pagado.
- Auditar “no aplica”.
- Exportar sin secretos.
- Mostrar costos únicos y recurrentes en flujo de caja.
- Evitar fechas tributarias inventadas.
- Tener procedimiento de backup/restore y control de acceso.
