# Eventos del servicio

> Estado: 🔴 Pendiente | Última actualización: YYYY-MM-DD
> Autor: Nombre Apellido | Equipo: nombre-del-equipo

## Propósito

Listar los eventos que publica y consume este servicio.

## Eventos publicados

| Evento | Descripción | Consumidor(es) | Garantía |
|--------|-------------|----------------|----------|
| `service.event.created` | Descripción del evento publicado. | `otro-servicio` | `at-least-once` |

## Eventos consumidos

| Evento | Productor | Propósito | Acción |
|--------|-----------|-----------|--------|
| `other-service.event.updated` | `otro-servicio` | Razón por la cual se consume. | Reaccionar actualizando estado. |

## Reglas de diseño

- Usar nombres de evento en formato `dominio.acción`.
- Mantener el schema de payload separado por versión.
- Documentar siempre productor y consumidor principal.
