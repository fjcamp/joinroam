# JoinHook / JoinRoam — Red de destinos y plataforma de reservas
**Fecha:** 2026-10-20  
**Estado:** DISEÑADO / PROPUESTA DE PRODUCTO  
**Relación con trabajo previo:** amplía el modelo de diagnóstico y habilitación de destinos; no implica que exista una plataforma implementada.

## Concepto
JoinHook es la organización paraguas que presta diagnóstico, asesoría y gestión. JoinRoam es la línea/plataforma orientada a viajeros con campers, motorhomes y casas rodantes y a propietarios de espacios preparados para recibirlos.

La plataforma busca que un usuario descubra varios destinos independientes, consulte sus características y condiciones, y reserve una estancia en el destino elegido. Cada destino administra su propia disponibilidad, capacidad, tarifas, reglas y operaciones; la red comparte catálogo, búsqueda, estándares de evaluación y una experiencia de reserva coherente.

## Terminología recomendada
- **Conexión viajero–destino:** relación general entre usuario y espacio anfitrión.
- **Reserva de estancia:** transacción concreta para fechas y número de vehículos/personas.
- **Red JoinRoam de destinos evaluados:** catálogo de lugares que cumplen criterios publicados y verificables.
- **Estándar JoinRoam de preparación del destino:** checklist y evaluación privada de la plataforma; no debe llamarse certificación oficial salvo que exista acreditación/autorización válida.
- **Destino verificado:** etiqueta limitada a verificaciones documentadas, con alcance y fecha visibles. No implica que JoinHook sustituya autoridades ni garantiza ausencia de riesgos.
- **Destino con certificación oficial:** solo si una entidad competente ha emitido la certificación y se puede comprobar su vigencia.

## Modelo funcional
1. Catálogo y búsqueda centralizados de destinos.
2. Página independiente por destino, con ubicación, servicios, capacidad, reglas, accesibilidad, fotos autorizadas, políticas y evidencias de estado.
3. Motor de reservas con inventario/calendario independiente por destino.
4. Gestión del anfitrión: tarifas, disponibilidad, bloqueos, reglas, cancelaciones y comunicaciones.
5. Flujo del viajero: búsqueda, filtros, detalle, consulta, reserva, confirmación, pago cuando se habilite, instrucciones de llegada y reseña/soporte.
6. Consola de JoinHook: alta y revisión de destinos, auditoría documental, estados de evaluación, reclamaciones y suspensión.
7. Control de calidad: criterios versionados, fechas de revisión, evidencias, acciones correctivas y vencimientos.
8. Administración financiera y comercial: comisiones/planes transparentes, facturas/documentos tributarios según la estructura contractual y conciliación de pagos.

## Arquitectura conceptual
Un portal de red y catálogo común, con registros separados por destino. Cada destino posee su propio calendario y reglas de inventario; no debe poder modificar reservas ni datos privados de otro anfitrión. La plataforma central presenta y coordina, pero cada reserva conserva el identificador del destino, el anfitrión responsable, la política aplicable y el registro de consentimiento/confirmación.

## Decisiones que requieren validación
- ¿La plataforma solo genera solicitudes de reserva o confirma reservas directamente?
- ¿Se cobrarán pagos en línea desde el lanzamiento o en una fase posterior?
- ¿JoinHook será intermediario, agente de reservas, proveedor tecnológico o marketplace? La definición contractual y tributaria debe revisarse profesionalmente.
- ¿Comisión por reserva, plan mensual al anfitrión, tarifa por publicación o modelo mixto?
- Política de cancelación, devoluciones, no presentación, disputas y daños.
- Protección de datos personales, pagos, impuestos, atención al consumidor y términos para anfitriones/viajeros.
- Estándar de evaluación, auditoría, uso de distintivos y tratamiento de destinos que dejan de cumplir requisitos.

## No prometer
No afirmar que todos los lugares son “certificados por JoinHook” sin una definición pública del esquema y evidencia. No asegurar que un destino está legalmente habilitado solo por estar listado. No afirmar reservas, disponibilidad, pagos ni integraciones hasta que estén implementados y probados.

## Documentos del paquete
- `modelo-red-y-terminologia.md`
- `requisitos-sistema-reservas.md`
- `estandar-evaluacion-y-gobernanza.md`
- `roadmap-plataforma.md`
