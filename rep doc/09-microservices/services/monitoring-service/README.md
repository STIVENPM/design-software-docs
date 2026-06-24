# monitoring-service

## Objetivo

Recolectar métricas, traza y alertas del ecosistema; proporcionar dashboards y endpoints para health-check.

## Responsabilidades

- Recolección de métricas Prometheus.
- Trazas con OpenTelemetry.
- Generación de alertas y notificaciones.

## Límites de negocio

- No almacena datos de negocio sensibes; solo metadata de operación.

## Casos de uso

- Alertar a operaciones cuando latencia de `scheduling-service` excede umbral.

## Entidades

- `metric`, `trace`, `alert` (metadatos)

## Persistencia

- Utilizar sistemas especializados: Prometheus para métricas, Jaeger/Tempo para trazas.

## Eventos

- Publica `metric.alert` cuando se detectan anomalías.

## Riesgos

- Alta cardinalidad de etiquetas; controlar para evitar explosión de series.
