# ⏰ Scheduler

Microservicio de tareas automáticas para CryptoLens. Funciona de forma autónoma en segundo plano sin intervención manual.

## Responsabilidad

Gestiona todas las tareas programadas del sistema: resúmenes diarios, evaluación de señales, alertas de usuarios, backups y limpieza de datos.

## Tareas programadas

| Tarea | Frecuencia | Descripción |
|---|---|---|
| Resumen diario | 08:00 cada día | Genera informe del mercado via LLM Service |
| Evaluación de señales | Cada 5 minutos | Rellena `was_correct` en señales pasadas |
| Monitor de alertas | Continuo | Dispara alertas cuando se cumplen umbrales |
| Backup PostgreSQL | 03:00 cada noche | pg_dump y subida a Google Drive |
| Limpieza de datos | Semanal | Elimina datos antiguos para no saturar la DB |

## Stack

- `APScheduler` — gestor de tareas programadas
- `httpx` — llamadas HTTP a otros servicios
- `SQLAlchemy` — ORM para PostgreSQL
- `kafka-python` — producer de alertas disparadas

## Kafka

```
Producer: alerts → {user_id, symbol, message, price_at_trigger}
```

## Configuración

```bash
cp .env.example .env
docker compose up scheduler
```

## Parte de CryptoLens

[crypto-lens-img](https://github.com/crypto-lens-img) · [data-pipeline](https://github.com/crypto-lens-img/data-pipeline) · [ml-engine](https://github.com/crypto-lens-img/ml-engine) · [api-gateway](https://github.com/crypto-lens-img/api-gateway)
