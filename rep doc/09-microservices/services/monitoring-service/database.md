# Database - monitoring-service

## Estrategia

- No usar PostgreSQL para series temporales de métricas; integrar con Prometheus/Tempo.
- Utilizar PostgreSQL para metadatos y alertas (tabla `alerts`).

| Tabla | PK | Campos |
|---|---|---|
| alerts | id | metric_name, severity, triggered_at, resolved_at |
