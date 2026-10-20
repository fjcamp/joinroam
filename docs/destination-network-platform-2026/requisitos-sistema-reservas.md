# Requisitos del sistema de reservas

## MVP recomendado
Comenzar con un catálogo central y páginas de destino, cada una con su propio calendario y gestión. Antes de automatizar pagos, validar el proceso de operación con unos pocos destinos piloto y medir consultas, conversiones, cancelaciones y carga de soporte.

## Funciones del viajero
- Buscar por zona, fechas, tamaño/tipo de vehículo, servicios, capacidad y actividades cercanas.
- Ver fotos autorizadas, descripción, servicios confirmados, reglas, horarios, accesos, precios, disponibilidad y políticas.
- Enviar consulta o solicitud de reserva.
- Recibir confirmación, instrucciones de llegada, comprobante y canal de ayuda.
- Revisar historial, solicitar cambios/cancelación conforme a la política y compartir reseña moderada.

## Funciones por destino/anfitrión
- Perfil independiente y documentación de titularidad/autorización de operación según corresponda.
- Calendario, temporadas, precios, cupos y cierres por mantenimiento.
- Definición de capacidad máxima, tipos de vehículos admitidos, horarios, servicios y reglas.
- Aceptación/rechazo de solicitudes o confirmación automática según configuración.
- Registro de reservas, comunicaciones, incidencias, cancelaciones y devoluciones.
- Reportes de ocupación y ventas.
- Gestión de usuarios por rol sin acceso a datos privados de otros anfitriones.

## Funciones de administración JoinHook
- Alta, revisión y publicación/suspensión de destinos.
- Lista de requisitos con evidencia, fecha, responsable y fecha de próxima revisión.
- Estados de revisión y plan de corrección.
- Revisión de quejas, incidentes, reservas y cambios relevantes.
- Auditoría de acciones administrativas.
- Panel de red con métricas agregadas y exportación controlada.

## Modelo de datos mínimo
- Host / anfitrión
- Destination / destino
- DestinationService / servicio disponible y su evidencia
- DestinationReview / evaluación, criterio, resultado, revisor, fecha, evidencia, caducidad
- Availability / disponibilidad independiente por destino
- RatePlan / tarifa y temporada
- Booking / reserva con destino, fechas, viajero, vehículo, estado, precio, moneda, condiciones aceptadas
- Payment / pago y devolución, solo cuando se habilite un proveedor de pagos
- Message / comunicación
- PolicyVersion / política aceptada por versión
- Incident / incidencia y resolución
- AuditLog / eventos relevantes

## Estados sugeridos de reserva
Borrador → Solicitud recibida → Pendiente de anfitrión (si aplica) → Confirmada → En estancia → Completada. Estados alternativos: Rechazada, Cancelada, No presentada, En disputa, Reembolsada parcialmente/totalmente. Definir transiciones válidas y responsables.

## Requisitos de calidad y seguridad
- Autenticación robusta, roles y permisos mínimos; aislamiento por anfitrión/destino.
- Protección de datos personales y minimización de campos.
- Registro de cambios, consentimiento y versiones de términos/políticas.
- Validación de entradas, protección contra inyección/XSS/CSRF según arquitectura.
- Rate limiting, sesiones seguras, HTTPS, copias de seguridad y plan de recuperación.
- Gestión segura de secretos; nunca almacenar contraseñas bancarias ni claves del SII.
- Pagos con proveedor especializado; evitar almacenar datos de tarjetas.
- Monitorización, logs sin datos sensibles y procedimiento de respuesta a incidentes.
- Accesibilidad y diseño adaptable a móvil, tablet y escritorio.
- Disponibilidad consistente: evitar doble reserva mediante bloqueo/operación atómica de inventario.

## Fuera de alcance hasta validación
- Pagos en línea, comisiones automáticas, facturación automatizada y conectores externos.
- Confirmación instantánea global, sincronización con plataformas de terceros o aplicación móvil nativa.
- Afirmar que el sistema existe o está probado. Todo debe pasar por implementación, pruebas y verificación antes de declararse disponible.
