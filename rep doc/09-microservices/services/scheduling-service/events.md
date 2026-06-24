# Eventos - scheduling-service

## Publicados

| Evento | Versión | Payload | Consumidores |
|---|---:|---|---|
| schedule.requested | v1 | `{ scheduleId, programId, term, requestedBy }` | monitoring-service, audit-service |
| schedule.completed | v1 | `{ scheduleId, status, summary, occurredAt }` | actors-service, document-service, audit-service |

## Consumidos

| Evento | Versión | Propósito |
|---|---:|---|
| course.created | v1 | Recalcular si aplica |
| reference.updated | v1 | Re-evaluar restricciones |
| actor.updated | v1 | Actualizar disponibilidad local |
| environment.updated | v1 | Re-evaluar asignaciones |

## Contratos

- Todos los eventos incluyen metadata con `eventId`, `correlationId`, `occurredAt`.
