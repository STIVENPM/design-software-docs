# Estrategia de Almacenamiento Documental y Evidencias

Este documento define la estrategia para gestionar documentos, PDFs, evidencias, reportes y trazabilidad en el Sistema de Gestión de Horarios SENA.

## Principios

- Metadatos en PostgreSQL; objetos binarios en almacenamiento orientado a objetos (S3 compatible: MinIO, Azure Blob Storage, AWS S3).
- Control de versiones por documento mediante `document_version` y checksum.
- Retención y políticas de privacidad según normativa institucional.

## Arquitectura

- `document-service` expone API para upload/download, generación de PDF y signed URLs.
- Metadatos (owner, type, createdAt, sourceService, relatedEntityId) almacenados en PostgreSQL.
- BLOBs almacenados en bucket con prefijo `sena/{service}/{yyyy}/{mm}/{id}`.

## Seguridad

- Acceso a objetos restringido por signed URLs con expiración corta (default 10 minutos).
- Todas las transferencias TLS.
- Logs de acceso y descarga registrados en `audit-service`.

## Flujos de generación de PDF

1. `document-service` recibe petición de `scheduling-service` para generar reporte.
2. Genera PDF en worker asíncrono, sube al bucket y crea metadato en PostgreSQL.
3. Publica `document.generated.v1` con `documentId` y `url` (signed URL temporal).

## Integridad y verificación

- Cada archivo almacenado incluye `sha256` y `size` en metadatos.
- Verificación periódica (job de reconciliación) para detectar objetos huérfanos.

## Retención y purgado

- Política por tipo de documento (e.g., audit logs: 7 años, evidencias educativas: 5 años).

## Observabilidad y auditoría

- Cada operación (upload/download/delete) genera un evento hacia `audit-service`.
# Estrategia de almacenamiento y gestión documental

> Estado: 🟡 En progreso | Última actualización: 2026-06-24
> Autor: Arquitectura de documentación técnica

## Propósito

Establecer la estrategia de almacenamiento, ciclo de vida y gobierno de los documentos generados, versionados y consumidos por el ecosistema de microservicios del **Sistema de Gestión de Horarios SENA**.

Este documento define las reglas de almacenamiento de contenido binario, la persistencia de metadatos, la clasificación de documentos y la integración operativa con `07-document-service`.

## Alcance

Aplica a:

- Archivos generados por `document-service` y sus consumidores.
- PDFs de horarios, fichas, certificados y reportes.
- Plantillas institucionales de documentos.
- Metadatos, versiones y rutas de acceso.
- Ciclo de vida, expiración y purga de almacenamiento.

No aplica a datos relacionales de transacción interna contenidos exclusivamente en las bases de datos propietarias de cada servicio.

## Modelo de almacenamiento

### Metadatos

- Se guardan en `document_db` dentro de `07-document-service`.
- Tablas principales:
  - `documento`
  - `version_documento`
  - `plantilla`
- Los metadatos contienen referencia completa al `servicio_origen`, `referencia_id`, estado y rutas de almacenamiento.

### Contenido binario

- **DEV**: filesystem local en el host de la aplicación.
  - Ruta base: `DOCUMENT_STORAGE_PATH=/data/documentos`
  - Ejemplo de archivo: `/data/documentos/2026/06/scheduling-service/HORARIO_PDF/3f1d7c4e-9f6a-4b3e-a1d0-2b492f903e7a.pdf`
- **QA / PROD**: almacenamiento de objetos compatible con S3.
  - Variable: `DOCUMENT_STORAGE_ADAPTER=S3`
  - Bucket/contendor: `sena-documentos`
  - Endpoint configurable en `DOCUMENT_S3_ENDPOINT`.

### Estructura de rutas

Formato estándar:

```
/documentos/{ano}/{mes}/{servicio_origen}/{tipo}/{uuid}.{ext}
```

Ejemplos:

- `/documentos/2026/06/scheduling-service/HORARIO_PDF/3f1d7c4e-9f6a-4b3e-a1d0-2b492f903e7a.pdf`
- `/documentos/2026/06/document-service/TEMPLATE/4d2b1f7a-5c6d-47f0-b8a0-7e52f9c9a493.html`

## Tipos de documentos y ownership

| Tipo | Servicio propietario | Uso principal | Persistencia | Retención mínima |
|------|----------------------|---------------|--------------|------------------|
| `HORARIO_PDF` | `document-service` | Exportación de horario publicado | Objeto binario + metadatos | 2 años |
| `FICHA` | `document-service` | Documento de caracterización de ficha | Objeto binario + metadatos | 2 años |
| `CERTIFICADO` | `document-service` | Certificados de formación | Objeto binario + metadatos | 5 años |
| `REPORTE` | `document-service` | Reportes de KPI y seguimiento | Objeto binario + metadatos | 1 año |
| `PLANTILLA` | `document-service` | Plantillas HTML y PDF | Objeto binario + metadatos | Indefinida mientras esté activa |
| `EVIDENCIA_AUDIT` | `audit-service` | Soporte de auditoría | Objeto binario + metadatos | 5 años |

## Integración con `07-document-service`

`07-document-service` es el único servicio autorizado para:

- crear, actualizar y eliminar registros en `document_db`
- versionar documentos y plantillas
- gestionar el ciclo de vida de archivos binarios
- exponer APIs de consulta y generación de documentos

### Rol del servicio

- Recibe solicitudes de generación de documento desde `scheduling-service`, `academic-management-service` y `actors-service`.
- Renderiza PDFs usando `pdf-renderer-worker`.
- Publica eventos de documento cuando el archivo está disponible.
- Expone URLs de descarga prefirmadas o rutas seguras con tokens.

## Ciclo de vida del documento

1. `07-document-service` recibe solicitud de creación o actualización.
2. Crea un registro en `documento` con estado `GENERANDO`.
3. El `pdf-renderer-worker` genera el contenido binario y lo almacena.
4. Se crea un registro en `version_documento`.
5. El documento pasa a estado `DISPONIBLE`.
6. El servicio publica evento `DocumentoGenerado`.
7. Se expide URL temporal para descarga o visualización.
8. Cuando `expires_at` se alcanza, el estado cambia a `EXPIRADO`.
9. El `document-lifecycle-worker` purga o archiva según la política.

## Política de retención y purga

- Documentos tipo `HORARIO_PDF` y `FICHA` deben retenerse al menos 2 años.
- Documentos tipo `CERTIFICADO` deben retenerse al menos 5 años.
- Reportes e indicadores deben retenerse al menos 1 año.
- Los documentos expirados mantienen metadatos durante 90 días adicionales antes de purgarse físicamente.
- La purga se realiza en el `document-lifecycle-worker` fuera del horario de producción.

## Backup y recuperación

- **Metadatos**: backups diarios de `document_db` con retención mínima de 30 días.
- **Contenido binario**: respaldos automáticos en el almacenamiento de objetos o replicación de buckets.
- **Recuperación**: restauración del bucket/objeto + metadatos relacionados.

## Entrega de acceso y seguridad

- Todas las descargas deben ser autorizadas por `iam-service`.
- Las URLs de acceso se generan como prefirmadas y expiran en un tiempo limitado.
- Los documentos sensibles deben cifrarse en reposo y cifrarse en tránsito.
- El servicio debe auditar accesos mediante eventos de `audit-service`.

## Variables de configuración

| Variable | Ambiente | Descripción |
|----------|----------|-------------|
| `DOCUMENT_STORAGE_ADAPTER` | DEV/QA/PROD | `LOCAL` o `S3` |
| `DOCUMENT_STORAGE_PATH` | DEV | Ruta base en filesystem |
| `DOCUMENT_S3_BUCKET` | QA/PROD | Bucket o contenedor de documentos |
| `DOCUMENT_S3_ENDPOINT` | QA/PROD | Endpoint S3 compatible |
| `DOCUMENT_S3_REGION` | QA/PROD | Región del almacenamiento |
| `DOCUMENT_PRESIGNED_URL_TTL` | Todos | Tiempo de expiración de URLs |

## Ejemplos de rutas y payload

### Ruta estándar de documento

```
/documentos/2026/06/scheduling-service/HORARIO_PDF/3f1d7c4e-9f6a-4b3e-a1d0-2b492f903e7a.pdf
```

### Ejemplo de metadato JSON para `documento`

```json
{
  "id": "3f1d7c4e-9f6a-4b3e-a1d0-2b492f903e7a",
  "nombre": "Horario Ficha 2026-1",
  "tipo": "HORARIO_PDF",
  "servicio_origen": "scheduling-service",
  "referencia_id": "2dc4fb78-9a29-4b1c-8cf0-8d6c9e4c47a3",
  "ruta_almacenamiento": "/documentos/2026/06/scheduling-service/HORARIO_PDF/3f1d7c4e-9f6a-4b3e-a1d0-2b492f903e7a.pdf",
  "hash_sha256": "b1946ac92492d2347c6235b4d2611184",
  "tamaño_bytes": 458712,
  "estado": "DISPONIBLE",
  "created_at": "2026-06-24T09:30:00Z",
  "expires_at": "2028-06-24T09:30:00Z"
}
```

## Criterios de diseño

- El `document-service` es el único responsable del acceso físico a los archivos.
- Los demás servicios consumen documentos solo mediante APIs o URLs controladas.
- No se permite acceso directo al filesystem o bucket desde servicios distintos de `07-document-service`.
- El versionamiento de documento se gestiona en `version_documento` y puede tener múltiples versiones activas.

## Integración con eventos

- `DocumentoGenerado`: producido por `document-service` cuando un archivo está disponible.
- `DocumentoExpirado`: producido por `document-service` cuando un documento supera su fecha de expiración.
- `PlantillaActualizada`: producido por `document-service` cuando se actualiza una plantilla.

El flujo de eventos permite que `audit-service` registre el hecho, que `monitoring-service` calcule métricas de uso y que `scheduling-service` reactive generación si un horario es republicado.

## Flujo de generación de documento

1. `scheduling-service` publica evento `HorarioPublicado`.
2. `document-service` recibe el evento y crea un documento tipo `HORARIO_PDF`.
3. El `pdf-renderer-worker` genera el PDF usando plantilla y datos del horario.
4. Se almacena el archivo y se actualiza el registro de versión.
5. El servicio emite `DocumentoGenerado`.
6. El consumidor obtiene el archivo mediante URL prefirmada.

## Monitoreo y alertas

- Medir latencia de generación de documento.
- Alertar si más del 5% de documentos entra en estado `EXPIRADO` antes de ser consultados.
- Alertar fallos de escritura en almacenamiento de objetos.

## Requisitos operativos

- El adapter local debe ser funcional en DEV sin conexión a S3.
- En QA/PROD debe soportarse failover de almacenamiento compatible S3.
- Las políticas de retención deben ser configurables desde `document-service`.

## Resumen

- `07-document-service` gestiona metadatos y almacenamiento binario.
- DEV usa filesystem local; QA/PROD usa almacenamiento de objetos compatible S3.
- Los documentos tienen rutas estructuradas, expiración y versionamiento.
- El acceso siempre se controla vía IAM y se audita.
