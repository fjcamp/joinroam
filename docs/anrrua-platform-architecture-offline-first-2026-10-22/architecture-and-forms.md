# Ánrrua — formularios, modelo de datos y arquitectura offline-first
Fecha: 2026-10-22
Estado: PROPUESTA DE ARQUITECTURA — no implementada ni desplegada.
Alcance: diseño funcional, datos, stack recomendado y plan de validación. No demuestra que existan formularios públicos, destinos activos, reservas ni mapas descargables.

## 1. Decisión ejecutiva
- Backend/API y panel de administración: Ruby on Rails.
- Base de datos central: PostgreSQL + PostGIS para coordenadas, consultas por distancia y relaciones espaciales.
- App móvil: Flutter + Dart para Android/iOS.
- Base local móvil: SQLite mediante una capa tipada (por ejemplo Drift), con repositorios que leen desde la base local y sincronizan con API.
- Mapas offline: evaluar MapLibre Native para Flutter y regiones descargables; probar licencias, proveedor de tiles, atribución, almacenamiento, límites y comportamiento real en Android/iOS antes de elegir.
- Rust: no usar en MVP. Considerarlo solo ante una necesidad medida de rendimiento o un componente aislado.
- Panel web y app comparten API y reglas de negocio; no compartir acceso directo a la base central con el cliente móvil.
- MVP: primero formularios de captación y panel interno; luego piloto web/PWA si conviene; app móvil offline cuando la oferta de destinos y las necesidades de ruta estén validadas.

## 2. Formularios de captación

### Formulario A — viajero/a
Objetivo: validar necesidades reales y captar participantes para entrevistas/pruebas; no simular reservas.
Campos mínimos:
1. Nombre o alias (opcional al inicio).
2. Un medio de contacto: email o WhatsApp (uno requerido; explicar uso).
3. Tipo de vehículo: motorhome/autocaravana; camper; casa rodante con remolque; van adaptada; otro; aún no tengo.
4. Zona que recorre o quiere recorrer: Araucanía; Los Ríos; ambas; otra zona del sur (texto opcional).
5. Etapa: planificando; viajando ahora; viajo varias veces al año; recién estoy explorando.
6. Prioridades (múltiple): acceso y maniobrabilidad; dimensiones máximas; electricidad; agua; descarga de aguas grises/negras; baño/ducha; precio total; reglas y restricciones; disponibilidad/contacto; cobertura/señal; cercanía a naturaleza/servicios.
7. Problema más frecuente al buscar dónde pernoctar (texto corto opcional).
8. Función más útil sin internet: ver mapa y destinos guardados; fichas y reglas; coordenadas; ruta/GPX; contactos guardados; otra.
9. Interés en entrevista/prueba (sí/no).
10. Consentimiento explícito, no premarcado, para responder a esta solicitud. Consentimiento de marketing separado y opcional.

CTA: “Quiero participar en la validación de Ánrrua”.
Texto visible: “Ánrrua está en etapa de validación. Este formulario no confirma disponibilidad ni realiza reservas. Tus datos se usarán para responder y, solo con permiso separado, invitarte a pruebas o novedades.”
No pedir RUT, dirección residencial, datos bancarios ni ubicación precisa en esta etapa.

### Formulario B — propietario/a o administrador/a
Objetivo: entender oferta, permiso de publicación y brechas de preparación; no prometer certificación ni clientes.
Campos mínimos:
1. Nombre de contacto y rol (propietario/a, administrador/a, representante autorizado/a, otro).
2. Un medio de contacto: email o WhatsApp (uno requerido).
3. Comuna/sector y región; no pedir coordenada exacta ni dirección pública en el primer contacto.
4. Tipo de lugar: camping establecido; parcela/terreno privado; viña/granja/establecimiento rural; área turística; otro.
5. Estado actual: recibe viajeros; recibe solo con coordinación; aún no recibe; idea en evaluación.
6. Vehículos que podría admitir: camper/van; motorhome; casa rodante con remolque; por evaluar.
7. Servicios disponibles (marcar solo lo que existe): electricidad; agua potable; baño; ducha; descarga de aguas; gestión de residuos; señal; área de maniobra; ninguno/por evaluar.
8. Capacidad estimada y restricciones conocidas (texto opcional).
9. Interés: aparecer en una red futura; evaluar potencial; recibir orientación sobre requisitos y brechas; conocer costos; recibir consultas/reservas en una fase futura.
10. Permiso para ser contactado y consentimiento separado para conservar/publicar información. No publicar fotos, coordenadas ni datos del predio sin autorización explícita y alcance acordado.

CTA: “Quiero evaluar mi lugar con Ánrrua”.
Texto visible: “Enviar este formulario no incorpora automáticamente el lugar a la red, no certifica su seguridad ni acredita cumplimiento normativo. La publicación requiere revisión y autorización del responsable.”

### Reglas compartidas de ambos formularios
- Guardar fuente de captación, fecha, versión del aviso de privacidad, estado del consentimiento y fecha de retiro.
- Validación de email/teléfono; limitar longitud de texto; protección anti-spam y acceso por roles.
- Mensaje de confirmación honesto; no prometer contacto en plazo fijo hasta contar con capacidad operativa.
- Minimización de datos, plazo de conservación definido y mecanismo para solicitar acceso/corrección/eliminación, sujeto a obligaciones legales.
- Separar captación de clientes de consentimiento para marketing.
- Registrar consentimiento con texto/versión y timestamp, no solo un booleano.
- Los formularios pueden comenzar como formularios web simples conectados al backend; no requieren app móvil ni mapa para validar demanda.

## 3. Modelo de datos inicial (PostgreSQL)
Diseñar relacional primero; no crear una tabla gigante con campos de propietario, destino, servicios y reservas mezclados.

### Entidades núcleo
- users: id, email_normalized, phone, auth_provider, status, created_at.
- profiles: user_id, display_name, preferred_language, role flags. Evitar recopilar datos no necesarios.
- consent_events: id, subject_type, subject_id, purpose, consent_text_version, granted, captured_at, withdrawn_at, source.
- leads: id, segment (traveler/host), contact fields, source_campaign, status, notes, created_at, consent references.
- host_organizations: id, primary_contact_user_id, legal/public display name if applicable, status.
- destinations: id, host_organization_id, public_name, region, commune, public_location geometry/geography, exact_location restricted, publication_status, capacity, created_at, updated_at.
- destination_access: destination_id, vehicle_type, max_length_cm nullable, max_height_cm nullable, max_width_cm nullable, surface, slope, turning_space, towing/trailer constraints, access_notes, last_reviewed_at.
- services: id, canonical service code/name (electricity, potable_water, bathroom, shower, greywater, blackwater, waste, signal, lighting).
- destination_services: destination_id, service_id, availability/status, details, source_status, evidence_id, last_confirmed_at.
- destination_rules: destination_id, rule_type, value/text, effective_from, effective_until, evidence_id.
- evidence_records: id, destination_id, claim/field, evidence_type (host_declared, document_reviewed, field_observed, competent_source_confirmed, official_certificate_verified, pending, disputed), source_reference, reviewed_by, reviewed_at, scope, expires_at, notes.
- media_assets: id, destination_id, storage_key, media_type, usage_permission_status, permission_scope, captured_at.
- availability_windows (later): destination_id, start_at, end_at, status, confirmed_by, confirmed_at, expires_at. Only show “available” when the host has confirmed it within a defined freshness window.
- inquiries: id, destination_id, traveler/contact reference, requested dates, vehicle profile, message, status, created_at, synced_at.
- reviews (later): id, destination_id, author, stay/inquiry reference where feasible, rating, text, moderation_status, submitted_at.
- saved_destinations: user/device id, destination_id, saved_at.
- sync_changes / sync_cursor: versioned changes, client mutation id, sync state, timestamps and conflict outcome.
- audit_events: actor, action, object type/id, timestamp, minimal before/after diff where appropriate.

### Data integrity and privacy
- UUIDs for entities synchronized to devices; server timestamps plus client mutation IDs for idempotency.
- Store exact coordinates separately with stricter authorization than public coordinates; only disclose them with host permission.
- Do not store payment card data; use a certified payment provider if/when payments are introduced.
- Keep evidence per specific claim/field. A document review of one service does not verify all aspects of a destination.
- Keep source, reviewer, date, scope and expiration. Stale evidence should be labelled, not silently presented as current.
- Database constraints, foreign keys, uniqueness, indexes and migrations from the beginning.
- PostGIS spatial index (e.g. GiST) for nearby-destination searches; normal indexes for status, region, updated_at and host references.

## 4. Offline-first design: what works without signal
Minimum offline functions:
- View a previously downloaded area map.
- Browse downloaded destination cards and their last-known information.
- See coordinates, access notes, services, restrictions, evidence status and last-confirmed date.
- Save destinations and a shortlist locally.
- Open locally stored contact details and copy coordinates; opening WhatsApp/calling still depends on device/service availability.
- Record an inquiry or correction as “pending to send”; do not claim it reached the host until server acknowledgement.
- Show download size, progress, last sync time, storage use and delete-download option.

Not guaranteed offline:
- Current availability, live prices, new messages, payment, reservation confirmation, host response, live weather/traffic, live routing or new destinations not downloaded.
- GPS location can work without mobile data on many devices, but GPS quality and map coverage vary; offline map tiles do not automatically provide offline routing.
- Operating systems can remove cached map files under storage pressure; downloaded regions need integrity checks and an explicit re-download path.

### Download package
A user chooses a region/corridor and downloads a package containing:
1. Map tiles and required styles/fonts/sprites, subject to the map provider's licence and offline policy.
2. Destination records and images resized for offline use.
3. Relevant access/rules/service/evidence data with timestamps.
4. Optional route line or GPX file; routing engine/data evaluated separately.
5. Version, package size, checksum, generated_at and expires_at/refresh recommendation.

### Synchronization policy
- Read path: local SQLite first; UI never waits for a network request to show already downloaded data.
- Download/update: delta sync by version/cursor rather than redownloading the entire catalog.
- Offline writes: persist to local outbox with UUID mutation id; retry with backoff when online; server idempotency prevents duplicate inquiries.
- Conflict rules: never use “last write wins” blindly for critical destination facts. Host changes, evidence review and traveler corrections need source-aware conflict handling and audit trail.
- Separate data that can be stale safely (destination description) from data requiring fresh confirmation (availability, price, temporary closure).
- Provide visible “updated on” and “offline copy; may be out of date” labels.

## 5. What other apps do
- park4night's official offline guide describes downloading a map/database and automatically switching to offline use; its documented offline feature requires park4night+ (the cited guide lists €9.99/year at publication). This demonstrates the pattern, not a requirement to copy its pricing.
- MapLibre's Flutter binding documents downloadable offline regions on Android/iOS. The docs warn that on iOS cached tiles can be evicted under storage pressure and that web does not provide the same offline API. Treat offline downloads as testable, managed packages rather than assuming permanent storage.
- Flutter's official offline-first guidance describes local storage plus network data, synchronization, retries and conflict handling. These are architectural requirements, not features that appear automatically just by choosing Flutter.
- PostGIS documents spatial indexes for efficient geographic queries.
Sources:
  - https://park4night.com/download/manual/park4night_offline_es.pdf
  - https://github.com/maplibre/flutter-maplibre-gl/blob/main/website/docs/advanced/offline-regions.md
  - https://docs.flutter.dev/app-architecture/design-patterns/offline-first
  - https://developer.android.com/topic/architecture/data-layer/offline-first
  - https://postgis.net/documentation/faq/spatial-indexes/
  - https://guides.rubyonrails.org/

## 6. Proposed system flow
Traveler/host forms → Rails API + validation → PostgreSQL/PostGIS → review/admin workflow → published destination data → Flutter sync API → SQLite + downloaded map package.
Host availability and new inquiries return through Rails; requests created offline remain queued until acknowledged.
JoinHook monitors operational health and evidence freshness but must not become a second source of truth or edit production data autonomously without controls.

## 7. Implementation phases and gates
Phase 0 — discovery and decisions
- Confirm product scope, user segments, first geographic corridor, privacy notice and owner permission.
- Audit existing repos/hosting constraints before selecting deployment target.
Gate: requirements and hosting feasibility reviewed.

Phase 1 — forms and internal workflow
- Implement two forms, consent logs, anti-spam, lead states and export/backup.
- Build internal destination review workflow; no public listings until authorized.
Gate: submit/test both forms end-to-end and verify deletion/access workflow.

Phase 2 — data model and web catalogue
- Rails API, PostgreSQL/PostGIS schema, roles, migrations, seed/test data and audit trail.
- Internal-only destination records; use fictional data for prototypes.
Gate: tests cover permissions, evidence status, stale data and exact-location access.

Phase 3 — offline mobile proof of concept
- Flutter + SQLite; 20–50 test destinations; saved list; one small map region; no bookings.
- Airplane-mode tests, app restart, low storage, interrupted downloads, duplicate sync, Android and iOS where available.
Gate: user can find a downloaded destination in airplane mode and understand when information is stale.

Phase 4 — pilot
- 3–5 host locations only with written permission; test traveler interviews and manually coordinated inquiries.
- Measure download size, sync success, stale-data rate, host response time, field completeness and cost per inquiry.
Gate: evidence from pilot supports continuing before adding payments or automated bookings.

Phase 5 — transactional features (later)
- Confirm availability workflow, cancellation/refund rules, payment provider, tax/accounting, legal terms, insurance/liability and support operations.
- Only then decide whether reservation payments belong in MVP.

## 8. Open decisions
- First geographic corridor and exact traveler segment.
- Android-first or Android+iOS from pilot start.
- Map provider, tile source/licence, offline coverage size and whether offline turn-by-turn routing is truly needed.
- Hosting target and costs after checking existing JoinHook infrastructure; no deployment decision is made here.
- Legal/privacy review and retention schedule.
- Whether the initial pilot should be web-only before committing to a native mobile build.

## Status
DESIGNED: this proposal and initial schema.
NOT IMPLEMENTED: public forms, production schema, API, offline sync, map downloads, bookings or payments.
NOT VERIFIED: host supply, permissions, market-wide demand, offline performance or operating cost.
NO EVIDENCE = NO CLAIM.
