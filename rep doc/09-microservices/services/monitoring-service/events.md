# Eventos - monitoring-service

Publicados

| Evento | Versión | Payload |
|---|---:|---|
| metric.alert | v1 | `{ metricName, value, severity, occurredAt }` |

Consumidos

- `schedule.requested`, `schedule.completed` para calcular SLAs y tiempos.
