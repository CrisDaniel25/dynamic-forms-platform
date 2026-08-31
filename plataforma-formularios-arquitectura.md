# Plataforma de Formularios y Data

## 1. Objetivo

Definir una plataforma web para captura y administración de formularios dinámicos, con soporte para:

- registro base de usuarios,
- formularios variables en el tiempo,
- consentimientos,
- expedientes,
- imágenes y adjuntos,
- administración de versiones,
- trazabilidad,
- pruebas automatizadas,
- operación inicial para un rango de 100 a 500 registros.

## 2. Stack recomendado

### Frontend

- Angular
- `formio.js` integrado en Angular para render dinámico de formularios

### Backend

- Node.js
- Strapi como CMS/headless backend para administración, API, autenticación y gobierno de contenidos

### Base de datos

- PostgreSQL como base principal
- Redis opcional como complemento de cache y colas

### Storage

- MinIO para documentos, imágenes, consentimientos escaneados y demás adjuntos

### Distribución

- Docker
- Docker Compose para ambientes local, QA e integración

## 3. Decisión de base de datos

## 3.1 Comparativa Redis vs MongoDB

### Redis

**Pros**

- Excelente para cache de catálogos, sesiones y consultas frecuentes.
- Útil para colas, rate limiting y procesamiento asíncrono.
- Bueno como apoyo para mejorar respuesta del sistema.

**Contras**

- No es recomendable como base principal de negocio.
- El modelo de persistencia no está orientado a trazabilidad documental compleja.
- Introduce complejidad operativa si el volumen y concurrencia siguen siendo bajos.

### MongoDB

**Pros**

- Flexible para almacenar formularios y respuestas en JSON.
- Se adapta bien a estructuras dinámicas y poco rígidas.

**Contras**

- Las relaciones entre usuarios, consentimientos, expedientes, adjuntos y versiones se vuelven más complejas.
- Los reportes y filtros transversales suelen requerir más trabajo.
- El control transaccional y la integridad referencial quedan más débiles que en SQL.

## 3.2 Recomendación final

Para este proyecto no conviene usar ni Redis ni MongoDB como base principal.

La mejor opción es:

- PostgreSQL como base transaccional principal.
- Redis solo si se necesita cache, colas o control de sesiones.

### Razón

El sistema no es únicamente un motor de formularios dinámicos. También necesita:

- relaciones consistentes entre entidades,
- versionado de formularios,
- listados y filtros,
- auditoría,
- historial,
- enlaces con adjuntos y expedientes.

Ese tipo de modelo funciona mejor con una base relacional.

## 4. Arquitectura propuesta

## 4.1 Vista lógica

### Capa de presentación

- Portal Angular para captura de formularios.
- Módulo admin Angular para operación y consulta.

### Capa de formularios dinámicos

- `formio.js` renderiza formularios a partir de una definición JSON versionada.
- El frontend solicita al backend la versión activa del formulario.

### Capa API y negocio

- Strapi expone APIs REST.
- Strapi administra autenticación, permisos, catálogos y metadatos.
- Servicios custom en Node/Strapi manejan:
  - publicación de formularios,
  - versionado,
  - persistencia de respuestas,
  - asociación de adjuntos,
  - consolidación de expediente.

### Capa de persistencia

- PostgreSQL guarda entidades maestras, relaciones, auditoría y metadatos.
- MinIO guarda archivos binarios.
- Redis, si se usa, guarda cache y jobs temporales.

## 4.2 Flujo principal

1. Un administrador crea o modifica un formulario.
2. El sistema genera una nueva versión de definición.
3. El formulario se publica.
4. El frontend consulta la versión vigente.
5. El usuario captura información.
6. La respuesta se guarda asociada a:
   - usuario,
   - formulario,
   - versión exacta,
   - adjuntos relacionados.
7. Los documentos e imágenes se almacenan en MinIO.
8. El expediente del usuario se construye a partir del registro base más sus formularios relacionados.

## 5. Modelo de datos inicial

## 5.1 Entidades principales

### Usuario

- `id`
- `codigo`
- `nombres`
- `apellidos`
- `documentoIdentidad`
- `fechaNacimiento`
- `sexo`
- `telefono`
- `email`
- `estado`
- `createdAt`
- `updatedAt`

### Formulario

- `id`
- `codigo`
- `nombre`
- `tipo`
- `descripcion`
- `estado`

Tipos sugeridos:

- registro-base
- consentimiento
- expediente
- seguimiento
- adjunto-metadato

### VersionFormulario

- `id`
- `formularioId`
- `version`
- `schemaJson`
- `uiSchemaJson` opcional
- `vigenciaDesde`
- `vigenciaHasta` opcional
- `estado`
- `publicadoPor`
- `publishedAt`

Estados sugeridos:

- draft
- published
- archived

### RespuestaFormulario

- `id`
- `usuarioId`
- `formularioId`
- `versionFormularioId`
- `payloadJson`
- `estado`
- `fechaRespuesta`
- `respondidoPor`

Estados sugeridos:

- borrador
- finalizado
- anulado

### Consentimiento

- `id`
- `usuarioId`
- `respuestaFormularioId`
- `tipoConsentimiento`
- `fechaFirma`
- `vigente`
- `adjuntoId`

### Expediente

- `id`
- `usuarioId`
- `codigo`
- `estado`
- `fechaApertura`
- `fechaCierre` opcional

### Adjunto

- `id`
- `usuarioId`
- `respuestaFormularioId` opcional
- `expedienteId` opcional
- `bucket`
- `objectKey`
- `fileName`
- `mimeType`
- `sizeBytes`
- `checksum`
- `tipo`
- `uploadedBy`
- `uploadedAt`

### Auditoria

- `id`
- `entidad`
- `entidadId`
- `accion`
- `antesJson`
- `despuesJson`
- `actor`
- `fecha`

## 5.2 Regla clave de diseño

Las respuestas deben quedar asociadas a una versión específica del formulario. Nunca deben depender únicamente de la definición "actual", porque los formularios cambiarán con el tiempo.

Esto permite:

- reconstruir capturas históricas,
- evitar incompatibilidades por cambios de campos,
- cumplir trazabilidad y auditoría.

## 6. Diseño de módulos

## 6.1 Frontend Angular

Módulos sugeridos:

- `auth`
- `usuarios`
- `formularios`
- `consentimientos`
- `expedientes`
- `adjuntos`
- `admin-formularios`
- `reportes`

Componentes relevantes:

- listado de usuarios
- detalle de usuario
- visor de expediente
- renderizador dinámico Form.io
- historial de respuestas
- carga de archivos

## 6.2 Backend Strapi

Colecciones o content types sugeridos:

- usuarios
- formularios
- versiones-formulario
- respuestas-formulario
- consentimientos
- expedientes
- adjuntos
- auditorias

Customizaciones necesarias:

- controllers para publicación de formularios
- services para versionado
- lifecycle hooks para auditoría
- políticas de acceso por rol
- endpoints para carga y asociación de archivos

## 7. MinIO

Uso recomendado:

- bucket por ambiente o dominio funcional
- nombres de objeto con prefijo por usuario/expediente
- almacenamiento de binarios, no de metadatos de negocio

Metadatos a persistir en la base:

- bucket
- object key
- nombre original
- tamaño
- mime type
- hash
- fecha de carga
- relación con entidad funcional

## 8. Seguridad

Recomendaciones mínimas:

- autenticación centralizada en Strapi
- roles separados para admin, operador y consulta
- URLs firmadas para descargas desde MinIO
- validación de tipo y tamaño de archivo
- auditoría de cambios en formularios y respuestas
- versionado inmutable de formularios publicados

## 9. Automatización de pruebas

## 9.1 Estrategia

Debe haber pruebas en cuatro niveles.

### Unitarias

- validadores de formularios
- adaptadores Form.io
- servicios Angular
- servicios custom de Strapi
- reglas de publicación y versionado

### Integración

- creación de usuario base
- publicación de formulario
- guardado de respuesta
- asociación de consentimientos
- carga de archivos en MinIO
- consulta del expediente consolidado

### End to End

Escenarios mínimos:

1. Registrar usuario.
2. Completar formulario base.
3. Completar consentimiento.
4. Subir imagen o documento.
5. Consultar listado.
6. Abrir detalle del expediente.
7. Ver historial de formularios y adjuntos.

### Carga

Con un universo de 100 a 500 registros, no se necesita una estrategia masiva, pero sí una validación operativa realista:

- 20 a 50 usuarios concurrentes.
- creación y consulta de registros.
- subida de archivos pequeños y medianos.
- búsqueda y paginación.

## 9.2 Herramientas sugeridas

- Frontend: Vitest o Jest
- E2E web: Playwright
- Backend/API: Jest o Vitest + Supertest
- Carga: k6

## 10. Docker

Servicios sugeridos en `docker-compose`:

- `frontend`
- `backend`
- `postgres`
- `minio`
- `redis` opcional
- `nginx` opcional

## 11. MVP por fases

## Fase 1. Base operativa

- autenticación
- registro base de usuario
- listado de usuarios
- formulario dinámico base
- persistencia de respuestas
- carga de adjuntos

## Fase 2. Gobierno de formularios

- creación de formularios por admin
- versionado
- publicación
- historial de versiones

## Fase 3. Expediente completo

- consentimientos
- expedientes
- visor consolidado por usuario
- trazabilidad de adjuntos

## Fase 4. Calidad operativa

- pruebas E2E
- pruebas de integración
- pruebas de carga
- observabilidad y auditoría reforzada

## 12. Riesgos y controles

### Riesgo 1

Cambiar formularios sin versionado rompe respuestas históricas.

**Control**

Versionado obligatorio e inmutable al publicar.

### Riesgo 2

Guardar archivos sin modelo de asociación genera expedientes inconsistentes.

**Control**

Todo archivo debe quedar vinculado a usuario y a la entidad funcional correspondiente.

### Riesgo 3

Usar MongoDB como base principal puede complicar listados, integridad y trazabilidad.

**Control**

Usar PostgreSQL como origen de verdad.

### Riesgo 4

Subestimar pruebas de formularios dinámicos produce regresiones silenciosas.

**Control**

Cobertura de unitarias, integración y E2E sobre flujos críticos.

## 13. Recomendación final

Arquitectura recomendada para este proyecto:

- Angular + Form.io
- Strapi sobre Node.js
- PostgreSQL como base principal
- Redis opcional para cache/colas
- MinIO para storage
- Docker para despliegue
- Playwright, Jest/Vitest y k6 para automatización

Es una solución más estable que usar MongoDB como base principal y da mejor soporte para formularios dinámicos con historial, expediente documental y crecimiento controlado.
