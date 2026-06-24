# Plantilla Events

## Formato de evento

- Todos los eventos incluyen: `eventId`, `occurredAt`, `sourceService`, `version`, `payload`.

## Registro y manejo

- Consumo idempotente con `eventId` y `processedAt`.

## Versionado

- Mantener backward-compatible changes; para breaking changes crear `v2`.
