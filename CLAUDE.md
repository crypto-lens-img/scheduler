# Scheduler Service

## Responsabilidad
Gestor de tareas automaticas que corren en segundo plano.
Funciona de forma completamente autonoma sin intervencion manual.
Registra el estado de cada tarea en base de datos.

## Tareas programadas
- 08:00 cada dia: genera resumen diario llamando al LLM Service
- Cada 5 minutos: evalua senales pasadas y rellena was_correct en ml_signals
- Cada noche a las 03:00: backup de PostgreSQL y subida a Google Drive
- Cada semana: limpieza de datos antiguos para no saturar la DB
- Continuo: monitoriza alertas de usuarios y las dispara cuando se cumplen

## Kafka
- Producer: topic "alerts" -> {user_id, symbol, message, price_at_trigger}

## Base de datos - Tablas que gestiona
- scheduled_jobs: registro de cada ejecucion con estado y errores

## Comunicacion con otros servicios
- Llama al LLM Service via HTTP para el resumen diario
- Lee ml_signals de PostgreSQL para evaluar was_correct
- Lee alerts de PostgreSQL para monitorizar umbrales
- Escribe en Kafka cuando una alerta se dispara

## Stack
- APScheduler: gestor de tareas programadas en Python
- httpx: llamadas HTTP a otros servicios
- SQLAlchemy: ORM para PostgreSQL
- kafka-python: producer de alertas disparadas

## Estructura de carpetas esperada
scheduler/
  src/
    jobs/
      daily_summary.py
      signal_evaluator.py
      backup.py
      cleanup.py
      alert_monitor.py
    kafka/
      producer.py
    db/
      models.py
      session.py
  main.py
  requirements.txt
  Dockerfile
  .env.example

## Variables de entorno necesarias
- DATABASE_URL
- KAFKA_BOOTSTRAP_SERVERS
- LLM_SERVICE_URL
- BACKUP_DESTINATION
- GOOGLE_DRIVE_CREDENTIALS
