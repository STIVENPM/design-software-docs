# Eventos - audit-service

Publicados

| Evento | Versión | Payload |
|---|---:|---|
| audit.recorded | v1 | `{ recordId, resourceType, resourceId, actorId, occurredAt }` |

Consumidos

- Varios servicios publican eventos que se traducen en registros de auditoría (p.ej. `user.created`, `schedule.completed`).
