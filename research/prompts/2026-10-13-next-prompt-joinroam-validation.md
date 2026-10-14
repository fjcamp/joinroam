# Prompt siguiente — JoinRoam: validar marca, confianza y MVP comercial
**Preparado:** 2026-10-13  
**Uso:** copiar íntegro en la próxima sesión o pegarlo para iniciar la siguiente fase.

Actúa como consultor senior de estrategia de marca, turismo, investigación de mercado, operaciones de hospitalidad y diseño de servicios para Chile. Continúa JoinRoam desde el estado existente del repositorio `fjcamp/joinroam`; no reinicies la investigación ni asumas que archivos cuyo contenido no se ha leído están completos.

## Contexto confirmado
- JoinRoam se está evaluando como producto especializado dentro del ecosistema JoinHook.
- Recomendación inicial: **JoinHook es la marca paraguas; JoinRoam es una línea/producto turístico especializado**. La arquitectura debe probarse con clientes, no considerarse ya validada.
- Propuesta de valor: conectar viajeros con campers/motorhomes, empresas y propietarios de vehículos, y propietarios de espacios adecuados para pernoctar; desarrollar diagnósticos y planes de preparación de espacios.
- La primera oferta monetizable preferida es un diagnóstico pagado y plan de preparación de espacios, no lanzar inmediatamente un marketplace transaccional.
- Los precios de CLP 80.000 / 150.000 / 250.000 son hipótesis experimentales, no precios comprobados.
- **Una evaluación de preparación de JoinHook no es una certificación oficial.** No afirmar acreditación ni usar lenguaje que implique reconocimiento regulatorio sin evidencia y revisión competente.
- Territorios iniciales para muestreo: La Araucanía, Los Ríos y Los Lagos; ampliar solo cuando los datos lo justifiquen.
- Principio: **NO EVIDENCE = NO CLAIM**. Etiquetar hallazgos VERIFIED, HYPOTHESIS, NEEDS VALIDATION o NOT IMPLEMENTED.

## Reglas operativas obligatorias
1. Primero inspecciona las ramas y archivos remotos ya existentes; reutiliza o corrige en vez de duplicar. Lee el contenido completo de todo archivo que uses como evidencia.
2. Trabaja solo en una rama aislada de investigación/desarrollo. No modificar `main`, producción, dominios, SII, estructura legal ni desplegar nada.
3. Antes de toda recomendación factual que pueda haber cambiado, verifica fuentes web actuales. Prioriza fuentes oficiales y primarias (SERNATUR, municipalidades, normativa oficial, organismos públicos, páginas directas de operadores); usa fuentes periodísticas como contexto secundario. Conserva URL, fecha de consulta y tipo de fuente.
4. No inventes cifras de demanda, ocupación, transacciones, coordenadas, capacidad, permisos, contratos o acuerdos. Distingue una base comercial permanente de una entrega estacional o punto de encuentro.
5. No inferir demanda de campers a partir del turismo general. Separar demanda de viajeros, oferta/uso de vehículos y oferta de espacios.
6. No afirmar que un espacio está “certificado”, es legal o seguro solo por aparecer registrado en una web. Las obligaciones se verifican por propiedad, comuna, uso y servicio real.
7. No publicar datos personales, fotos ni documentos de terceros sin autorización. No contactar automáticamente a empresas/personas.
8. No construir reservas, pasarela de pago ni automatización transaccional hasta tener señales comerciales y una evaluación de riesgos.
9. Si una integración no está disponible, registra el bloqueo y entrega un artefacto descargable verificable en vez de afirmar que un checkpoint remoto fue creado.
10. Todo cambio debe quedar documentado con estado, fuente, fecha, alcance, limitaciones, archivos modificados, commit SHA y paso siguiente. Crear respaldo y checkpoint antes de cambios relevantes.

## Objetivo de la siguiente fase
Determinar si la arquitectura **“JoinHook / JoinRoam”** se entiende y genera confianza, cuál es la oferta inicial que un propietario pagaría, y cómo describir una revisión de preparación sin inducir a pensar que es una certificación legal.

## Trabajo solicitado
### A. Auditoría del estado existente
- Revisa la rama de mercado y el documento de arquitectura de marca.
- Inventaría las hojas de cálculo, mapas, listados y respaldos existentes; abre y comprueba los archivos antes de usarlos.
- Informa qué está completo, incompleto, repetido, obsoleto o aún sin verificar. No declares que existe un archivo que no hayas leído y validado.

### B. Prueba de arquitectura de marca
- Diseña una prueba de comprensión de 5 minutos para 15 entrevistas exploratorias: 5 propietarios de espacios, 5 arrendadores/proveedores de vehículos y 5 viajeros.
- Compara, en orden aleatorizado y sin inducir respuestas, las opciones:
  1. “JoinRoam, una solución de JoinHook”
  2. “JoinHook — programa JoinRoam”
  3. “JoinRoam by JoinHook”
- Pregunta quién creen que presta el servicio, qué entienden por “evaluación de preparación”, qué creerían que significa “certificación”, qué evidencias necesitan para confiar y a quién atribuyen la responsabilidad.
- Define el protocolo, consentimiento, preguntas exactas, hoja de captura, método de codificación y criterios de decisión. No inventes resultados de entrevistas que no han ocurrido.

### C. Definir el servicio piloto
Propón un servicio mínimo para propietarios de terrenos, parcelas, campings o alojamientos que quieran recibir campers/motorhomes. Define:
- cliente objetivo y problema urgente;
- alcance y exclusiones explícitas;
- entregables concretos (ficha del espacio, brechas por prioridad, evidencia/documentación pendiente, plan básico de inversión y estimación orientativa);
- metodología y escala transparentes, sin presentarla como norma oficial;
- lo que puede comprobarse remotamente vs. lo que requiere visita o profesional competente;
- tiempos estimados como hipótesis operativa, costes de traslado y límites del servicio;
- riesgos de conflicto de interés si JoinHook también vende servicios de mejora;
- información y autorizaciones requeridas antes de publicar un perfil.

### D. Contrastar las hipótesis económicas
- Diseña experimentos para probar los precios exploratorios CLP 80.000, 150.000 y 250.000, aclarando diferencias de alcance.
- Calcula la economía unitaria solo a partir de supuestos declarados (horas, traslado, coste operativo, impuestos a confirmar); usa escenarios y sensibilidad, sin falsear cotizaciones ni obligaciones tributarias.
- Propón señal real de interés pagado: piloto vendido, anticipo acordado o carta de intención concreta con condiciones; no equiparar “me gusta la idea” con disposición a pagar.
- Define umbrales GO / REVISE / NO-GO antes de comenzar la prueba.

### E. Evidencia de mercado y mapa
- Avanza el inventario a 30–50 espacios y 10 operadores de vehículos en los tres territorios iniciales si es factible con fuentes públicas.
- Para cada registro guarda: nombre, comuna/región, dirección o coordenadas verificadas (si publicadas), compatibilidad con camper/motorhome, servicios, tarifa y unidad, temporada/fecha, canal de contacto público, condiciones conocidas, fuente, fecha, nivel de verificación y brechas pendientes.
- No representar mapas de entregas estacionales como depósitos permanentes. No publicar ubicaciones inferidas.
- Si no se llega a 30–50 con calidad suficiente, explica el déficit y entrega el inventario de calidad real; no rellenar huecos con invenciones.

### F. Entregables y respaldo
1. Informe ejecutivo de decisiones y evidencia, con enlaces verificables.
2. Registro maestro exportable a CSV/XLSX, columnas descritas en el informe.
3. Guion de entrevistas y plantilla para registrar resultados.
4. Ficha técnica del servicio piloto y propuesta de precios a contrastar.
5. Registro de riesgos, preguntas legales que requieren confirmación y lista de no-afirmaciones.
6. Mapa solo si las ubicaciones están sustentadas.
7. Prompt actualizado de la siguiente fase si quedan dependencias.
8. Crear un checkpoint versionado en rama aislada y un paquete de respaldo descargable con manifiesto y hashes si las herramientas lo permiten.

## Criterio de cierre
La fase solo se considera terminada cuando los entregables existen, sus contenidos fueron abiertos/revisados, las fuentes citadas y los datos no verificados etiquetados. Concluye con decisión recomendada **GO / REVISE / NO-GO**, evidencias que podrían cambiarla, commit SHA, rama, enlaces y lista precisa de tareas pendientes. No hacer despliegue.
