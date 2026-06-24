# audit-service

## Objetivo

Registrar y conservar trazas de auditoría de acciones críticas para cumplimiento y trazabilidad.

## Responsabilidades

- Ingesta de eventos de auditoría y almacenamiento seguro.
- Proveer API para búsqueda y export de registros de auditoría.

## Límites de negocio

- No reemplaza logging operativo; enfocado en eventos de negocio y acceso a datos sensibles.

## Casos de uso

- Registrar creación/actualización/eliminación de entidades críticas.
- Consultar historial de auditoría por entidad y usuario.

## Entidades

- `audit_record { id, actorId, action, resourceType, resourceId, timestamp, details JSONB }`

## Reglas y seguridad

- Acceso restringido: solo roles `auditor`, `security`.
- Encriptación at-rest para datos sensibles.

## Persistencia e índices

- Tabla `audit_records`; índices en `resourceType, resourceId` y `actorId, timestamp`.

## Eventos

- Publica `audit.recorded` tras confirmación de almacenamiento.

## Riesgos

- Volumen de datos y cumplimiento de retención; plan de archiving a frío.
