# GitHub Project Setup

## Objetivo

Definir la configuracion exacta del tablero Scrum semanal para llevar el proyecto hasta un POC.

## Tipo de tablero

- GitHub Projects
- alcance sugerido: nivel usuario `CrisDaniel25`
- uso: gestion semanal de sprint, backlog y avance a POC

## Nombre sugerido

- `Plataforma Formularios - POC Board`

## Columnas

- `Backlog`
- `Ready`
- `In Progress`
- `Review`
- `Done`

## Campos sugeridos

### `Status`

Valores:

- Backlog
- Ready
- In Progress
- Review
- Done

### `Sprint`

Valores:

- Sprint 0
- Sprint 1
- Sprint 2
- Sprint 3
- Sprint 4
- Sprint 5

### `Epic`

Valores:

- Plataforma
- Usuarios
- Formularios
- Consentimientos
- Expedientes
- Adjuntos
- Seguridad
- Auditoria
- Calidad
- DevOps

### `Priority`

Valores:

- High
- Medium
- Low

### `Estimate`

Valores sugeridos:

- 1
- 2
- 3
- 5
- 8

### `Owner`

- campo persona de GitHub

## Labels sugeridos

- `epic:plataforma`
- `epic:usuarios`
- `epic:formularios`
- `epic:consentimientos`
- `epic:expedientes`
- `epic:adjuntos`
- `epic:seguridad`
- `epic:auditoria`
- `epic:calidad`
- `area:frontend`
- `area:backend`
- `area:data`
- `area:devops`
- `area:e2e`
- `area:observabilidad`
- `priority:high`
- `priority:medium`
- `priority:low`
- `type:sprint`

## Milestones sugeridos

- `Sprint 0`
- `Sprint 1`
- `Sprint 2`
- `Sprint 3`
- `Sprint 4`
- `Sprint 5`
- `POC`

## Flujo semanal

### Lunes

- refinar backlog del sprint
- mover items a `Ready`
- confirmar responsables y estimaciones

### Durante la semana

- mover a `In Progress`
- pasar a `Review` al abrir PR o validacion funcional
- mover a `Done` solo con criterios cumplidos

### Viernes

- demo
- cierre de sprint
- revision de bloqueos
- preparar siguiente sprint

## Reglas operativas

- ningun item entra a `In Progress` sin criterio de aceptacion
- ningun item pasa a `Done` sin evidencia minima
- formularios publicados no se editan, se versionan
- adjuntos deben estar vinculados al menos a usuario
- incidentes de seguridad bloquean cierre del sprint

## Archivos de carga local ya preparados

- backlog general: `docs/github-project-import.csv`
- sprints: `docs/github-sprints.csv`
- plan semanal: `docs/sprint-plan-poc.md`

## Proximo paso cuando GitHub quede escribible desde la sesion

1. crear project `Plataforma Formularios - POC Board`
2. crear columnas y campos
3. crear milestones de sprint
4. importar backlog base
5. importar sprints
6. asignar `Sprint` y `Status` inicial
7. dejar vista board y vista roadmap
