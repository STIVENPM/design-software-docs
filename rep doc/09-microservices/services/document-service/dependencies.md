# Dependencies - document-service

Consume:

- `scheduling-service` (para generar reportes al completarse un horario).

Publica:

- `document.generated` (consumido por `audit-service`, `monitoring-service`).

Consideraciones:

- Usar bucket con políticas de acceso segregadas por entorno (dev/stage/prod).
