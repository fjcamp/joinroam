# Ánrrua — evaluación de hosting para Ruby on Rails
Fecha: 2026-10-22
Estado: recomendación preliminar; no se ha creado infraestructura ni desplegado aplicación.

## Contexto
La infraestructura conocida de JoinHook usa BlueHosting/cPanel/Passenger para Node.js. Esto no demuestra que el plan soporte Rails con versiones, procesos y servicios necesarios. Mantener JoinHook donde está y alojar Ánrrua por separado reduce riesgo; confirmar requisitos con el proveedor antes de cualquier cambio.

## Resumen ejecutivo
Recomendación inicial para evaluar: **Render** como PaaS administrado para prototipo Rails, con PostgreSQL administrado, y región lo más cercana posible a los usuarios dentro de las regiones que el proveedor ofrezca. Confirmar región exacta y disponibilidad en el panel antes de decidir.
- Render soporta Rails, despliegue conectado a Git, dominio propio, TLS y servicios de aplicación/worker. Documentación: https://render.com/docs/deploy-rails-6-7 y https://render.com/docs/web-services
- La página de precios consultada lista web service Free con limitaciones y un plan pequeño de pago de USD 7/mes (512 MB RAM) y USD 25/mes (1 CPU/2 GB RAM). PostgreSQL se cobra por separado; confirmar los precios y límites actuales al contratar: https://render.com/pricing
- Para un prototipo puede servir el nivel gratuito si se acepta sus limitaciones; no lo recomiendo como base para un servicio de reservas en producción sin revisar suspensión/arranque, disponibilidad, persistencia, backups y límites del plan.
- La base de datos de producción debe tener backups y una política de recuperación probada; no asumir que todo plan gratuito incluye las mismas garantías.

## Alternativas
### Render — primera opción para evaluar
Ventajas: experiencia gestionada, Rails nativo, Git deploy, TLS, workers y Postgres disponible. Adecuado para reducir carga de operaciones.
Riesgos: la base se cobra aparte, el plan gratis tiene limitaciones y los costes crecen con workers, almacenamiento, transferencia y base. Revisar región y política de backups.
Enlaces: https://render.com/docs/deploy-rails-6-7 ; https://render.com/pricing

### Fly.io — alternativa técnica
Permite Rails en máquinas y Postgres administrado. Flexible y con facturación por recursos; exige más familiaridad con CLI, regiones, volúmenes y operación.
Precios oficiales consultados: Machines shared 256 MB alrededor de USD 2.19/mes, 512 MB USD 4.39/mes, 1 GB USD 8.78/mes; Managed Postgres Basic USD 38/mes, almacenamiento aparte. Son precios publicados en USD, no una cotización total; egress, volúmenes y otras opciones pueden añadir costes.
Enlaces: https://fly.io/rails/ ; https://fly.io/pricing/

### Heroku — experiencia administrada, posiblemente más costosa
Buen flujo Rails, despliegues desde Git, workers y add-ons. Comparar coste total de dynos, Postgres y workers antes de elegir.
Enlace: https://www.heroku.com/ruby/

### VPS + Hatchbox — menor markup, más responsabilidad
Hatchbox indica USD 15 por servidor/mes más el coste del servidor; tiene prueba gratuita de 7 días. Despliega Rails en servidores propios y gestiona procesos, pero aún hay que presupuestar VPS, backups, monitorización y recuperación.
Enlace: https://hatchbox.io/frameworks/rails

### BlueHosting/cPanel actual
No descartarlo sin verificar, pero no asumir compatibilidad. La documentación de cPanel indica que el soporte depende de paquetes del servidor y habilitación por el proveedor; la interfaz Rails fue retirada en cPanel & WHM v136. El hosting compartido y los límites de procesos pueden ser inadecuados para workers, PostGIS y una API offline-first. Preguntar al proveedor por escrito antes de usarlo.
Enlace: https://docs.cpanel.net/cpanel/software/ruby-on-rails/ y https://docs.cpanel.net/knowledge-base/web-services/using-passenger-applications/

## Costos que deben presupuestarse
1. Proceso web Rails (CPU/RAM).
2. PostgreSQL + PostGIS, almacenamiento y backups.
3. Workers/jobs para correos, importaciones y tareas de sincronización.
4. Almacenamiento de fotos/documentos y transferencia.
5. Dominio/subdominio, correo transaccional, monitoreo y logs.
6. Mapas: proveedor de tiles, licencia, descarga offline y tráfico; el hosting de Rails no cubre automáticamente mapas offline.
7. Conversión de moneda, impuestos y comisiones de pago cuando corresponda.
No estimar costo total final sin saber carga, tamaño de base, volumen de mapas, política de backup y disponibilidad requerida.

## Arquitectura de dominios sugerida
- joinhook.cl: web actual, permanece en BlueHosting.
- api.anrrua.cl o subdominio equivalente: API Rails independiente en PaaS.
- anrrua.cl: landing web; puede estar en hosting estático compatible.
- Base de datos: privada, no expuesta directamente al móvil.
No se ha comprobado la disponibilidad/propiedad del dominio anrrua.cl; verificar NIC Chile antes de usarlo. El nombre es ilustrativo.

## Puerta de decisión antes de contratar
- Auditar repositorios actuales y decidir si Rails sigue siendo la opción correcta.
- Crear prueba de concepto local; no conectar producción ni mover JoinHook.
- Confirmar disponibilidad de PostGIS, backups, región, límites de salida de datos, secretos, logs, jobs y despliegue desde rama de investigación.
- Ejecutar prueba de restauración de base y migración.
- Calcular costo mensual mínimo, piloto y crecimiento.
- Aprobar proveedor solo después de revisar precio actual y límites del plan.

## Estado de evidencia
VERIFICADO EN DOCUMENTACIÓN PÚBLICA: los proveedores citados ofrecen rutas de despliegue compatibles con Rails; las páginas de precios publican los precios orientativos referidos.
PENDIENTE: región óptima disponible, coste total real, plan seleccionado, compatibilidad exacta con la app futura, dominio, acuerdos y despliegue.
NO IMPLEMENTADO: no se ha creado app Rails, base, servicio, dominio ni entorno.
NO CAMBIOS: JoinHook actual permanece sin modificaciones.