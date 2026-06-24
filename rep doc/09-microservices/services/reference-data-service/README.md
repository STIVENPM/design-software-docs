# reference-data-service

## Objetivo del servicio

Proveer catálogos de datos de referencia (sedes, turnos, modalidades, tipos de recursos) consumibles por otros servicios.

## Responsabilidades

- CRUD de datos de referencia.
- Versionado y publicación de cambios por evento.

## Límites de negocio

- No contiene información personal; solamente catálogos compartidos.

## Casos de uso

- Actualizar capacidad de una sede.
- Publicar nueva tipología de turno.

## Entidades del dominio

| Entidad | Descripción |
|---|---|
| site | Sede física o virtual (id, name, address, capacity) |
| shift | Turno (id, name, start, end) |

## Reglas de validación

- Capacidad de sede >=0; los turnos deben tener `start < end`.

## Persistencia e índices

- Tablas `sites(id UUID)`, `shifts(id UUID)`.
- Índices: `sites(name)`, `shifts(start, end)`.

## Flujo de negocio (Mermaid)

```mermaid
flowchart LR
  UI[Admin UI] --> RDS[reference-data-service]
  RDS -->|publish| EventBus
```

## Riesgos técnicos

- Cambios a datos de referencia requieren coordinación para evitar inconsistencia en horarios existentes.

## Escalabilidad

- Bajo uso transaccional; replicar lectura mediante read-replicas si hay muchos consumidores.
