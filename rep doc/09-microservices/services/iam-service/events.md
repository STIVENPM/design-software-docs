# Eventos - iam-service

## Publicados

| Evento | Versión | Payload resumen | Propósito |
|---|---:|---|---|
| user.created | v1 | `{ userId, email, createdAt }` | Notificar alta de usuario |
| user.role.updated | v1 | `{ userId, roles[] }` | Sincronizar autorizaciones |

## Consumidos

| Evento | Versión | Propósito |
|---|---:|---|
| audit.recorded | v1 | Registrar confirmación de auditoría |

## Esquema y requisitos

- Incluir `eventId`, `occurredAt`, `sourceService`, `correlationId` en metadata.
