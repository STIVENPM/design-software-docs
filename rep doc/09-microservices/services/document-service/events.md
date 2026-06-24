# Eventos - document-service

Publicados

| Evento | Versión | Payload |
|---|---:|---|
| document.generated | v1 | `{ documentId, ownerId, type, url, occurredAt }` |

Consumidos

- `schedule.completed` para generar reporte de horarios automáticamente.
