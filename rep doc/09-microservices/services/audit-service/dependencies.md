# Dependencies - audit-service

Consume:

- Eventos de negocio y operacionales (`*.created`, `*.completed`, `metric.alert`).

Publica:

- `audit.recorded` (utilizado por `monitoring-service` para confirmación y por controles de cumplimiento).
