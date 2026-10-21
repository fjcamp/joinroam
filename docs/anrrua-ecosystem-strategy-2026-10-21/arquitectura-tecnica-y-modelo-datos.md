# Ánrrua — arquitectura técnica y modelo de datos
**Corte:** 21-10-2026. **Estado:** DISEÑADO; no implementado ni desplegado.

## 1. Principio
Separar captación inicial de producto transaccional. No construir un marketplace completo antes de validar oferta, demanda, operación, privacidad y economía unitaria.

- Validación: formularios públicos reales y consentidos, CRM interno, fichas privadas y procesos manuales.
- Piloto: catálogo pequeño autorizado; consultas y comprobaciones manuales de disponibilidad; sin afirmar reserva automática.
- MVP: web adaptable, catálogo, filtros, perfil anfitrión, solicitudes, calendario independiente y consola de administración.
- Posterior: pagos, comisiones, reembolsos, reviews e integraciones después de validar responsabilidades y costos.

## 2. Stack candidato
- **Datos:** PostgreSQL; Supabase candidato para acelerar Auth, Database y Storage, sujeto a revisar precios vigentes, límites, backups, accesos y necesidades reales.
- **Frontend:** tecnología web adaptable que encaje con el stack e infraestructura que JoinHook ya opera; evitar duplicar infraestructura antes de revisar el código disponible.
- **Backend:** reglas privilegiadas, disponibilidad, solicitudes, notificaciones, permisos, auditoría y validación del lado servidor.
- **Archivos:** storage privado para documentos de propiedad/identidad/revisión; solo fotos aprobadas en el espacio público.
- **Mapas:** elegir proveedor revisando cobertura local, precio, licencias, geocodificación, caché y atribución.
- **Analítica:** eventos agregados con datos mínimos.
Supabase es opción por evaluar, no una decisión implementada. Su documentación oficial requiere RLS y revisión de grants en tablas expuestas: https://supabase.com/docs/guides/database/postgres/row-level-security

## 3. Identidad única, roles separados
No crear dos bases de usuarios totalmente independientes. Una persona puede viajar y también ser anfitriona. Usar identidad central y roles/membresías separados:
- profiles: datos mínimos de cuenta.
- user_roles: roles asignados bajo proceso autorizado.
- host_organizations: persona/empresa operadora.
- host_members: relación usuario-organización con permisos.
- traveler_profiles: preferencias opcionales del viajero y su vehículo.
- leads: etapa de validación, separada de perfiles de producto y nunca visible públicamente.

## 4. Entidades principales del modelo
### Oferta/catálogo
- destinations: nombre público, descripción, comuna/sector, coordenadas públicas aproximadas, estado y responsable.
- destination_units: plazas/cupos individuales si un destino ofrece varias unidades de estancia.
- destination_services: servicio exacto, estado (declarado/revisado/no disponible), evidencia, revisor y caducidad.
- destination_rules: horario, residuos, ruido, fuego, mascotas, visitas, duración y restricciones.
- destination_media: foto/archivo, titular, permiso/licencia, audiencia, aprobación.
- rate_plans: precio, unidad, temporada, moneda, vigencia, cargos y condiciones.
- availability_blocks: bloqueos, mantenimiento y capacidad por fecha/unidad.

### Confianza y cumplimiento
- destination_assessments: versión metodológica, alcance, fecha, revisor, resultado y limitaciones.
- assessment_items / evidence_records: criterio, resultado, documento/foto, fecha, origen y próxima revisión.
- external_credentials: tipo exacto de registro/sello, entidad emisora, vigencia y evidencia.
- corrective_actions: brecha, gravedad, responsable, vencimiento y cierre.
- publication_approvals: versión aprobada por el anfitrión.
- incident_reports: incidentes/reclamaciones con acceso restringido.
- audit_logs: actor, acción, entidad, fecha e información mínima no sensible.

### Interacción
- inquiries: consultas no transaccionales.
- bookings: viajero, destino/unidad, fechas, personas/vehículo, precio, desglose, estado y versión de condiciones aceptadas.
- booking_events: historial de transiciones.
- messages: comunicación entre partes con retención definida.
- policy_versions / policy_acceptances: términos y política de cancelación aceptados y su versión.
- payments / refunds: fase posterior con referencias de proveedor y estados, nunca datos completos de tarjetas.
- reviews: asociadas a estancia verificable y moderadas.
- partner_organizations / partner_referrals: socios, acuerdos, incentivos y autorización de marca.

## 5. Relaciones de datos
Perfil 1:N roles/membresías; organización anfitriona 1:N destinos; organización N:M usuarios; destino 1:N unidades, servicios, tarifas, reglas, evidencias, evaluaciones, incidencias y bloqueos; reserva N:1 viajero y destino/unidad; reserva 1:N eventos y 0:N pagos/reembolsos. Implementar solo lo requerido por cada etapa.

## 6. Permisos de base de datos
- Público: fichas publicadas y campos explícitamente aprobados.
- Viajero: sus consultas, reservas, mensajes y perfil.
- Anfitrión: sus destinos/unidades/reservas; no puede acceder a los de otro anfitrión ni alterar unilateralmente una revisión independiente.
- Revisor: evidencia asignada y resultado de revisión.
- Soporte: caso/incidente en alcance autorizado.
- Administrador: privilegios explícitos auditados.
Aplicar mínimo privilegio, Row Level Security, grants revisados, validación server-side, límites de solicitudes y pruebas negativas entre usuarios y anfitriones. Las vistas filtradas no equivalen a controles de acceso.

## 7. Datos públicos vs privados
**Públicos, con aprobación:** nombre comercial, comuna/sector, descripción, fotos autorizadas, servicios y sus estados, condiciones de acceso, reglas, precios, capacidad, fecha de revisión y certificaciones externas comprobadas.

**Privados:** nombre legal/contacto personal, documentación de propiedad, dirección exacta o coordenadas sensibles hasta que proceda compartirlas, evidencias internas, incidencias, mensajes, reservas y datos personales.
No recopilar RUT, identificación, datos bancarios ni documentación sensible en formularios exploratorios sin necesidad demostrada y proceso seguro.

## 8. Estados del destino y reserva
Destino: lead → precalificación → antecedentes pendientes → evaluación → publicable dentro del alcance revisado → publicado → revisión requerida/suspendido/retirado. Los estados no equivalen a permiso o certificación.

Reserva futura: consulta → solicitud recibida → pendiente de anfitrión → confirmada → en estancia → completada. Alternativas: rechazada, cancelada, no presentada o disputada. No activar esta máquina de estados hasta implementar y probar roles, políticas, notificaciones y soporte.
Evitar dobles reservas con transacciones y controles de concurrencia en base de datos, no solo en el frontend.

## 9. Privacidad y seguridad
La Ley 21.719 tiene entrada en vigor señalada por BCN para el 1 de diciembre de 2026: https://www.bcn.cl/leychile/Navegar?idNorma=1209272&idVersion=2026-12-01
Preparar inventario de datos/fines/base de licitud, aviso claro, consentimiento donde corresponda, canal de acceso/corrección/eliminación/oposición, retención y borrado, contratos con proveedores, procedimiento de incidentes y control de accesos. Revisar requisitos jurídicos concretos con asesoría competente.
- Nada de secretos, tokens, claves tributarias o datos de identificación en GitHub.
- Separar dev/staging/prod y variables por ambiente.
- Backups cifrados con prueba de restauración y objetivos RPO/RTO.
- Logs sin información sensible; gestión de sesiones, rate limits, validación de entradas y pruebas de autorización.
- Fotografías únicamente con permiso o licencia.
- Marketing y contacto para responder una consulta se controlan de manera clara y no se presume consentimiento indefinido.

## 10. Pagos: fuera del primer MVP
Mercado Pago documenta Split Payments para marketplaces y requiere credenciales por vendedor vía OAuth; también detalla complejidades de devolución: https://www.mercadopago.cl/developers/es/docs/split-payments/split-1-1/integration-configuration/integrate-marketplace
No integrar hasta definir quién presta/vende la estancia, quién factura, quién cobra, comisiones, fondos insuficientes, reembolsos, impuestos y soporte. Si se habilita, usar proveedor especializado sin almacenar datos de tarjetas.

## 11. Criterios de aceptación
- Ningún destino se publica sin autorización y campos obligatorios suficientes.
- Viajero no ve datos privados ni reservas de terceros.
- Anfitrión no puede leer/editar destinos de otra organización.
- Cada afirmación de evaluación muestra fecha, alcance, evidencia y limitaciones.
- El consentimiento lleva finalidad y registro verificable; se puede tramitar una solicitud de eliminación.
- No existe doble reserva bajo solicitudes concurrentes.
- Backups se restauran en un ensayo registrado.
- Responsive móvil/escritorio y accesibilidad básica probadas.
- Pagos permanecen deshabilitados hasta decisión contractual/tributaria explícita.
