# Proyecto: Arquitectura de Ecosistema Big Data (Tesis de Maestría)

## Rol de Claude en este proyecto
Actuar como **arquitecto de soluciones de Big Data**, apoyando al estudiante/cliente a:
1. Diagnosticar los gaps técnicos señalados por el tutor de tesis. ✅ Hecho.
2. Producir un documento de decisión (cloud pago vs. on-premise) para comunicar al cliente. ✅ Enviado.
3. Diseñar **dos propuestas de arquitectura** (AWS/GCP y On-Premise) fuera de la carpeta `informacion-entregada/`. 🔜 Siguiente entregable.

## Contexto del proyecto
Tesis de maestría: **"Arquitectura de Big Data, aprendizaje automático aplicado al análisis"** (tema exacto a confirmar con el estudiante — mencionado de forma cortada en la transcripción).

Caso de uso actual (visto en el notebook): comparación de rutas críticas de tránsito/taxis entre **NYC (dataset TLC)** y **Quito**, usando `kagglehub` para descarga de datos, limpieza, árboles de decisión y visualización con Folium/mapas.

## Estructura de la carpeta
- `informacion-entregada/` — insumos originales, **NO modificar**:
  - `01-revision-tesis.md` — transcripción de la tutoría con las observaciones clave del tutor (fuente principal del diagnóstico).
  - `02-Bigdata2.ipynb` — notebook actual del estudiante: ingesta desde Kaggle (`kagglehub`), limpieza, modelado (árbol de decisión), comparación NYC vs Quito, visualización con mapas.
  - `03-PHOTO-2026-09-08-08-25-28.jpg` — captura de referencia mostrada por el tutor.
  - `Tesis NIG DATA2.docx` / `.pdf` — documento de tesis en curso.
- `ejemploArquitecturaAws.png`, `ejemploArquitecturaGCP.png`, `ejemploArquiteturaOnpremise.png` — capturas de arquitecturas de referencia que el tutor mostró en la tutoría (una por entorno). Usarlas como base visual al construir las dos propuestas de arquitectura del entregable 2.
- `entregables/` — lo que se produce y se envía al cliente:
  - `01-gaps-decision-cliente.md` — memo de gaps + decisión on-premise vs. cloud (ya enviado).
  - `report_01.pdf` — versión en PDF del memo anterior.

## Gap principal identificado por el tutor
El estudiante tiene **buena programación** (limpieza de datos, modelo, visualización en Colab) pero **NO tiene una arquitectura de ecosistema Big Data real**. Todo el flujo vive como archivos sueltos en Google Drive, lo cual el tutor considera **inseguro e insuficiente** para el nivel de la maestría (título del proyecto exige explícitamente "arquitectura").

### Lo que el tutor exige ver (patrón de referencia)
Un ecosistema con **capas separadas y explícitas**, sin importar el entorno elegido:
- **Ingesta**: herramienta que lee/mueve los datos (ej. Apache NiFi, Kafka, Cloud Dataflow, Kinesis, Event Hubs).
- **Almacenamiento**: repositorio gestionado, no una carpeta de Drive suelta. Debe implementar **arquitectura medallón (Bronze/Silver/Gold)**. Ejemplo local: **MinIO** (compatible con S3), gestionado como si fuera un servidor real, con instancias separadas por capa.
- **Procesamiento**: transformación/limpieza real de los datos que ya están en el storage (no directo sobre archivos de Drive). Ejemplo: Apache Spark (con GPU si aplica).
- **Analítica**: motor de consulta/ML propio del ecosistema (ej. BigQuery, Amazon ML, Azure Cognitive).
- **Visualización**: dashboard (Power BI, Grafana, Tableau).
- **Orquestación** (plus): automatización con Apache Airflow.
- **Seguridad** (plus): gestión de secretos/accesos con Vault o el mecanismo nativo del proveedor cloud.

El script de Python que el estudiante ya tiene (limpieza, modelo) **se mantiene**, pero debe re-apuntar sus lecturas/escrituras al storage gestionado (MinIO/S3/Cloud Storage/Blob Storage) en vez de a la carpeta de Drive directamente.

### Opciones de entorno válidas (a decidir con el cliente)
1. **On-Premise / local con Docker** (gratis): NiFi + Kafka (ingesta), MinIO (storage medallón), Spark (procesamiento), Airflow (orquestación), Vault (seguridad), Power BI (visualización). Cada capa en su propio contenedor Docker.
2. **Cloud pago** — AWS, GCP o Azure:
   - **GCP**: Drive/Cloud Storage (ingesta/origen) → Cloud Dataflow (procesamiento) → BigQuery + Cloud Storage (storage/analítica) → Apache Beam/Spark → Dashboard.
   - **AWS**: S3 (storage) → Kinesis (ingesta streaming) → Amazon ML (analítica) → Grafana/Tableau/Power BI (visualización).
   - **Azure**: Event Hubs (ingesta) → Azure Blob Storage / Data Lake → Data Fabric → Azure Cognitive/Cosmos (analítica) → Power BI.
   - Ventaja mencionada: **USD 300 de crédito gratis** al registrar tarjeta, generalmente suficiente para completar el proyecto sin costo real si se gestiona bien.
   - Restricción mencionada por el cliente: posible limitación para poner tarjeta de crédito (a confirmar — es justo la pregunta pendiente del memo enviado).
3. Cualquier opción elegida **debe justificarse técnicamente** en el documento de tesis (capítulo 2), incluyendo seguridad y control de accesos — especialmente si se decide simular con Drive, hay que explicar cómo se aseguran permisos/accesos.

## Cronograma crítico (mencionado en la tutoría)
- **Martes 8-sept-2026**: fecha límite mencionada por el tutor para mostrar el ecosistema armado (arquitectura + al menos empezando la visualización) con **≥70% de avance técnico**. Con eso el tutor podría calificar 70-80% de la nota técnica.
- **Sábado 12-sept-2026**: cierre oficial de la materia — se requiere avance documentado sí o sí.
- **Semana del 21-sept-2026**: fecha extraoficial solicitada por el tutor para defensa (posible miércoles 23 o jueves 24).
- **Riesgo (peor caso)**: si no se alcanza el 70% técnico, el estudiante reprueba la materia → debe repetir matrícula (costo económico) o queda solo con el trabajo de titulación pendiente, con defensa recién hasta ~abril 2027.

## Entregables de este proyecto de consultoría
1. **✅ Documento de gaps y decisión para el cliente** (`entregables/01-gaps-decision-cliente.md` y `report_01.pdf`) — enviado. Resume los gaps técnicos detectados en la tutoría, compara alternativas (on-premise vs. cloud AWS/GCP/Azure) con costos/tiempos, y el cronograma crítico. Queda pendiente la respuesta del cliente sobre disponibilidad de tarjeta/presupuesto cloud.
2. **🔜 Dos propuestas de arquitectura** (a ubicar fuera de `informacion-entregada/`, ej. en `entregables/`):
   - Arquitectura on-premise (Docker: NiFi/Kafka, MinIO, Spark, Airflow, Vault, Power BI).
   - Arquitectura cloud (AWS o GCP, a definir cuál priorizar según decisión del cliente).
   - Ambas deben mapear el caso de uso real del notebook (ingesta datasets Kaggle/TLC NYC + Quito → medallón → procesamiento/limpieza → modelo ML → visualización de rutas).
   - Usar las capturas `ejemploArquitecturaAws.png`, `ejemploArquitecturaGCP.png`, `ejemploArquiteturaOnpremise.png` como referencia visual.
3. Soporte para adaptar el notebook existente a la arquitectura elegida (lectura/escritura contra el storage gestionado en vez de Drive directo).

## Notas de estilo/trabajo
- El tutor es flexible en la herramienta específica, pero exige que las **capas estén claramente separadas y justificadas técnicamente** — no basta con simular todo en una carpeta de Drive sin argumentación de seguridad.
- Cualquier documento entregable a "cliente" debe ser claro, no técnico en exceso, orientado a la decisión (costo vs. tiempo vs. seguridad).

---

## Convenciones de desarrollo (plan — crear solo cuando arranque la implementación)

Repositorio de ecosistema Big Data con arquitectura de medalla (Bronze / Silver / Gold), código modular en `src/`. **No crear estas carpetas todavía** — son el plan para el día que empecemos a escribir código real, no antes. Evitamos scaffolding vacío sin lógica.

### Stack tecnológico
- **Procesamiento**: PySpark (Spark 3.x). ⚠️ Depende de la decisión de entorno pendiente (sección "Opciones de entorno"): si es on-premise, Spark corre en Docker sobre MinIO; si es cloud (GCP), se evalúa Dataflow/BigQuery en su lugar — a confirmar una vez el cliente responda.
- **Lenguaje**: Python 3.11 con type hints explícitos.
- **Storage medallón**: MinIO (compatible S3) si on-premise; Cloud Storage/BigQuery si GCP.
- **Orquestación**: Apache Airflow (mencionado por el tutor como el "plus" para automatizar el pipeline).
- **Calidad y pruebas**: Pytest / Great Expectations — no exigido explícitamente por el tutor, estándar propio de la consultoría para el entregable técnico.

### Estructura de carpetas
- `src/common/` — modelos y lógica reutilizable (ej. conexión a MinIO/S3, config de sesión Spark).
- `src/ingestion/` — lógica de ingesta (Bronze): descarga Kaggle/TLC, carga inicial al storage.
- `src/processing/` — limpieza, deduplicación e idempotencia (Silver): la lógica que ya existe en `02-Bigdata2.ipynb` migrada aquí.
- `src/aggregations/` — agregaciones y métricas finales (Gold): dataset listo para el modelo de árbol de decisión y la visualización.
- `dags/` — orquestación (Airflow) únicamente.
- `tests/unit/` — pruebas unitarias correspondientes a cada módulo nuevo.

### Reglas de código
1. Respetar la estructura de carpetas de arriba — no mezclar lógica de capas distintas en un mismo módulo.
2. Type hints explícitos en todo el código Python (`pyspark.sql.DataFrame`, `Dict`, `Optional`, etc.).
3. Evitar UDFs de Python en Spark salvo estrictamente necesario; priorizar funciones nativas de `pyspark.sql.functions`.
4. Escrituras siempre idempotentes (`overwrite` particionado o `merge` si se adopta Delta Lake — a confirmar, el notebook actual usa Parquet/CSV planos).
5. Al crear o modificar código, indicar siempre el file path exacto (ej. `src/processing/silver_routes_cleaner.py`).
6. Toda función/módulo nuevo lleva su prueba unitaria correspondiente en `tests/unit/`.

### Tarea inicial
Pendiente de habilitar — bloqueada por la decisión de entorno (on-premise vs. cloud, ver sección "La decisión" del memo `entregables/01-gaps-decision-cliente.md`). En cuanto el cliente confirme, la primera tarea técnica es construir `src/common/session.py` (sesión Spark) y `src/ingestion/` apuntando al storage elegido en vez de a Drive.
