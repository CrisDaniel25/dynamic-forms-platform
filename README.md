# Plataforma Formularios Monorepo

Monorepo base para una plataforma de formularios dinamicos con arquitectura modular y limites claros entre frontend, backend, datos e infraestructura.

## Estructura

- `apps/frontend`: aplicación Angular
- `apps/backend`: backend Node.js + Strapi
- `packages/shared`: contratos compartidos, tipos y utilidades
- `infra`: composición Docker y artefactos operativos
- `docs`: backlog, definiciones operativas y documentación auxiliar

## Decisiones base

- Frontend: Angular + `formio.js`
- Backend: Node.js + Strapi
- Base principal: PostgreSQL
- Cache/colas: Redis opcional
- Storage: MinIO
- Distribución: Docker Compose

## Objetivo del monorepo

Aunque la infraestructura inicial sera monolitica, la estructura queda separada como si fueran microservicios para facilitar:

- evolución por módulos,
- imágenes Docker independientes,
- despliegue por componente,
- ownership técnico claro,
- desacoplamiento progresivo.

## Arranque esperado

1. Configurar `.env` a partir de `.env.example`.
2. Levantar infraestructura con `docker compose up -d`.
3. Desarrollar `apps/frontend` y `apps/backend` como imágenes separadas.

## Archivos relevantes

- `docker-compose.yml`
- `apps/backend/database/init/001_schema.sql`
- `docs/github-project-import.csv`
- `docs/backlog-scrum-board.md`
- `plataforma-formularios-arquitectura.md`
