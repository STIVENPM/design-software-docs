# actors-service

## Objetivo

Gestionar actores del sistema: estudiantes, instructores y administradores, incluyendo disponibilidad y perfiles académicos.

## Responsabilidades

- CRUD de actores y disponibilidad.
- Exponer endpoints para obtener disponibilidad por rango.

## Límites de negocio

- No gestiona autenticación; delega en `iam-service`.

## Casos de uso

- Registrar disponibilidad de un instructor.
- Obtener lista de instructores disponibles para franja horaria.

## Entidades

- `actor { id, type, fullName, email, qualifications[] }`
- `availability { actorId, date, start, end }`

## Reglas de validación

- availability.intervals no deben solaparse por actor.

## Persistencia e índices

- Índice en `availability(actor_id, date, start, end)`.

## Eventos

- Publica `actor.updated.v1` cuando cambia perfil o disponibilidad.

## Riesgos y escalabilidad

- Mucha churning en disponibilidad; favorecer diseños append-only para auditoría y usar Redis para caching de disponibilidad agregada.
