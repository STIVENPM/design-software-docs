# Runbook del servicio

> Estado: 🔴 Pendiente | Última actualización: YYYY-MM-24
> Autor: Nombre Apellido | Equipo: nombre-del-equipo

## Propósito

Definir los procedimientos operativos para despliegue, monitoreo y resolución de incidentes del servicio.

## Despliegue

- Entorno: DEV / QA / PROD.
- Artefacto: JAR de Spring Boot construido con Maven.
- Variables de entorno clave:
  - `SPRING_DATASOURCE_URL`
  - `SPRING_PROFILES_ACTIVE`
  - `SERVICE_PORT`

## Verificación posterior al despliegue

- Comprobar endpoint de salud (`/actuator/health`).
- Validar conexiones a PostgreSQL.
- Verificar integración con `iam-service`.

## Troubleshooting

### El servicio no inicia

- Revisar logs de Spring Boot.
- Verificar credenciales de base de datos.
- Confirmar que el puerto no esté en uso.

### Errores de conexión a dependencias

- `iam-service`: token de autenticación inválido.
- `reference-data-service`: catálogo no disponible.

## Operación diaria

- Monitorear métricas de latencia y disponibilidad.
- Confirmar que las colas/eventos se procesan sin errores.

## Escalamiento

- Si se detectan fallos continuos en el subtipo de evento, notificar al equipo de backend.
