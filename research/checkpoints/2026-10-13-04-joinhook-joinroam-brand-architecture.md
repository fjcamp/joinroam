# CHECKPOINT 2026-10-13-04 — JoinHook / JoinRoam: arquitectura de marca
**Fecha:** 2026-10-13  
**Estado:** DESIGN DECISION PROPOSED / NEEDS COMMERCIAL VALIDATION  
**Repositorio:** `fjcamp/joinroam`  
**Rama de checkpoint:** `research/joinroam-brand-architecture-2026-10-13`  
**Rama padre conservada sin modificación:** `research/joinroam-market-validation-2026-10-13`

## Decisión estratégica registrada
- Recomendación: **JoinHook es la marca paraguas; JoinRoam es su producto/línea especializada de turismo camper y espacios de pernocta**.
- Relación comercial inicial a probar: “JoinRoam — una solución especializada de JoinHook”.
- No crear todavía una empresa jurídica separada solo para JoinRoam; no hacer cambios legales o tributarios.
- No presentar JoinRoam/JoinHook como certificadora oficial ni denominar “certificado” a un diagnóstico interno. Usar lenguaje delimitado, como “diagnóstico de preparación” o “evaluación de preparación JoinHook”, con criterios, versión, fecha, alcance y limitaciones.
- La marca y el modelo comercial siguen pendientes de validación en entrevistas, pruebas de pago y revisión de obligaciones aplicables.

## Evidencia y fundamento
El checkpoint de investigación de mercado `research/2026-10-13-joinroam-market-validation.md` en la rama `research/joinroam-market-validation-2026-10-13` distingue turismo general de demanda específica de campers, identifica oferta de flotas y espacios candidatos y recomienda validar la disposición a pagar antes de construir una plataforma transaccional.

Página de checkpoint relacionado en Notion:
- [CHECKPOINT 2026-10-13-02 — JoinRoam como producto JoinHook](https://app.notion.com/p/3f9b7fe8cfac8143b10ef3ab519c9f69?pvs=204)
- [CHECKPOINT 2026-10-13-03 — JoinRoam: investigación de mercado y validación](https://app.notion.com/p/3f9b7fe8cfac81278b56e1aa4593a1d0?pvs=204)

## Archivos versionados en esta rama
1. `research/2026-10-13-joinroam-brand-architecture.md` — fundamentos, lenguaje recomendado, límites de “certificación”, condiciones de confianza y validaciones pendientes. Commit de archivo: `27df8437f99873230002b23fad36adb1ed816f41`.
2. `research/prompts/2026-10-13-next-prompt-joinroam-validation.md` — instrucciones listas para la siguiente fase de auditoría, entrevistas, definición de MVP, prueba de precios e inventario. Commit de archivo: `c6ed90565ed8f29f5697d12ee0e269fe71650989`.
3. Este archivo registra el checkpoint de la decisión estratégica.

## Límites de alcance
- No se cambió `main`.
- No se desplegó producción ni staging.
- No se modificaron dominios, SII, estructura legal o pagos.
- No se declaró que exista una certificación acreditada.
- No se declaran realizadas entrevistas ni pruebas de pago.
- No se da por validada la arquitectura de marca: es una recomendación para someter a prueba.

## Reglas de evidencia
- `VERIFIED`: afirmación comprobada dentro del alcance escrito, fuente registrada.
- `HYPOTHESIS`: supuesto aún no probado.
- `NEEDS VALIDATION`: requiere más evidencia o confirmación profesional/oficial.
- `NOT IMPLEMENTED`: no construido o no puesto en operación.
- Principio rector: **NO EVIDENCE = NO CLAIM**.

## Próximo paso
Ejecutar el prompt `research/prompts/2026-10-13-next-prompt-joinroam-validation.md`. Empezar por auditar contenido remoto ya existente; probar comprensión de marca con 15 entrevistas exploratorias (5 anfitriones, 5 proveedores de vehículos, 5 viajeros); definir el servicio piloto y umbrales GO / REVISE / NO-GO antes de probar precios. Mantenerse en rama aislada y documentar cada fuente y cambio.
