<<<<<<< HEAD
# bigdata-transit-architecture
big data ecosystem poc
=======
# Bigdata Transit Architecture

Diseño de un ecosistema de Big Data (arquitectura de medalla — Bronze / Silver / Gold) aplicado a un caso de analítica de tránsito: comparación de rutas críticas entre Nueva York (dataset TLC) y Quito.

Trabajo de consultoría de arquitectura para un proyecto académico. Este repositorio documenta el diagnóstico técnico, la decisión de infraestructura (on-premise vs. cloud) y las propuestas de arquitectura resultantes.

## Contenido
- [`CLAUDE.md`](CLAUDE.md) — contexto completo del proyecto: gaps identificados, opciones de entorno evaluadas, plan de desarrollo.
- [`entregables/`](entregables) — memo de gaps y decisión de arquitectura (on-premise vs. cloud AWS/GCP/Azure).
- Diagramas de referencia (AWS, GCP, on-premise) usados como base visual para las propuestas de arquitectura.

## Stack propuesto
Python · PySpark · MinIO (S3-compatible) · Apache Airflow · Power BI — con alternativa cloud (GCP: Dataflow + BigQuery) según se confirme la infraestructura.

---
Nota: los insumos originales del proyecto (transcripciones, notebook del estudiante, documento de tesis) se mantienen fuera de este repositorio por confidencialidad.
>>>>>>> aa4dc4d (Diagnóstico inicial y decisión de arquitectura del ecosistema Big Data)
