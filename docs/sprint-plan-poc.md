# Sprint Plan to POC

## Cadencia

- Sprint semanal
- Duracion: 6 semanas
- Inicio sugerido: 2026-09-07
- Meta: llegar a un POC funcional al cierre de la semana del 2026-10-16

## Definicion de POC

El POC se considera logrado si cumple lo siguiente:

- login base con roles minimos,
- registro de usuario,
- listado de usuarios,
- formulario dinamico renderizado con Form.io,
- versionado y publicacion de formularios,
- captura de consentimiento,
- carga de adjuntos a MinIO,
- vista simple de expediente por usuario,
- despliegue local por Docker Compose,
- suite minima de pruebas unitarias, integracion y E2E sobre flujo critico.

## Sprint 0

- Semana: 2026-09-07 a 2026-09-11
- Objetivo: dejar base tecnica y criterios de arquitectura cerrados

Items:

- cerrar estructura de monorepo,
- definir convenciones de ramas y versionado,
- dejar compose operativo,
- validar modelo inicial PostgreSQL,
- definir content types de Strapi,
- definir contrato inicial frontend-backend.

Entregables:

- infraestructura local levantable,
- esquema inicial aprobado,
- backlog refinado para Sprint 1.

## Sprint 1

- Semana: 2026-09-14 a 2026-09-18
- Objetivo: autenticacion y registro base

Items:

- auth backend con roles admin, operador y consulta,
- login frontend,
- formulario base de usuario,
- persistencia de usuario y respuesta base,
- listado inicial de usuarios.

Entregables:

- flujo end-to-end de alta de usuario,
- listado navegable de usuarios,
- pruebas unitarias base.

## Sprint 2

- Semana: 2026-09-21 a 2026-09-25
- Objetivo: motor de formularios dinamicos

Items:

- integracion de Form.io en Angular,
- endpoint de consulta de formulario vigente,
- persistencia de respuestas versionadas,
- admin inicial de definiciones de formulario,
- historial simple por usuario.

Entregables:

- formulario dinamico consumido desde backend,
- respuestas ligadas a version exacta,
- pruebas de integracion API base.

## Sprint 3

- Semana: 2026-09-28 a 2026-10-02
- Objetivo: versionado y publicacion

Items:

- flujo draft, published y archived,
- publicacion inmutable,
- historial de versiones,
- validaciones de cambio de schema,
- reglas de auditoria para cambios de formulario.

Entregables:

- admin puede publicar nueva version,
- frontend consume version vigente,
- trazabilidad de cambios de formulario.

## Sprint 4

- Semana: 2026-10-05 a 2026-10-09
- Objetivo: consentimientos, adjuntos y expediente

Items:

- captura de consentimiento,
- carga de imagenes y documentos a MinIO,
- asociacion de adjuntos a usuario y respuesta,
- vista de expediente por usuario,
- descarga segura con URLs firmadas.

Entregables:

- consentimiento funcional,
- adjuntos visibles desde expediente,
- expediente navegable de POC.

## Sprint 5

- Semana: 2026-10-12 a 2026-10-16
- Objetivo: estabilizacion y demo POC

Items:

- pruebas E2E del flujo critico,
- pruebas de integracion completas,
- smoke test Docker,
- prueba de carga inicial con k6,
- correccion de hallazgos criticos,
- guion de demo.

Entregables:

- POC demostrable de punta a punta,
- evidencia de pruebas,
- backlog post-POC priorizado.

## Definition of Done por sprint

- criterios de aceptacion cubiertos,
- codigo integrado en rama principal de trabajo,
- pruebas del alcance ejecutadas,
- cambios documentados,
- sin defectos criticos abiertos del sprint.

## Riesgos de cronograma

- intentar construir admin y motor dinamico completos demasiado pronto,
- no fijar reglas de versionado desde Sprint 2,
- subestimar integracion de archivos y seguridad de MinIO,
- dejar E2E para el final sin flujo estable.

## Recomendacion de tablero GitHub Projects

Campos:

- Status
- Sprint
- Epic
- Priority
- Owner
- Estimate

Vistas:

- Board semanal
- Backlog
- Roadmap por sprint
- Issues por epic
