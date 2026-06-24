# Eventos - training-environment-service

Publicados

| Evento | Versión | Payload |
|---|---:|---|
| environment.updated | v1 | `{ environmentId, capacity, features, occurredAt }` |

Consumidos

- `schedule.requested` para bloquear disponibilidad temporal si se requiere.
