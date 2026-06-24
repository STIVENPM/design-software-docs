# Eventos - actors-service

Publicados

| Evento | Versión | Payload |
|---|---:|---|
| actor.updated | v1 | `{ actorId, changes, occurredAt }` |

Consumidos

- `schedule.completed` para reconciliar asignaciones finales si es necesario.
