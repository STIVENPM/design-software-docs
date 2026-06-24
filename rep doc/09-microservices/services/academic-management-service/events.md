# Eventos - academic-management-service

Publicados

| Evento | Versión | Payload |
|---|---:|---|
| course.created | v1 | `{ courseId, code, name, weeklyHours }` |
| course.updated | v1 | `{ courseId, changes }` |

Consumidos

- `reference.updated` para mantener coherencia de sedes o modalidades si aplica.
