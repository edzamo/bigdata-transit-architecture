# Ecosistema de analítica Big Data para la tesis
### Memo de decisión — consultoría de arquitectura
**Fecha:** 08 de septiembre de 2026
**Ref:** revisión de tutoría 01 (`informacion-entregada/01-revision-tesis.md`)

Diagnóstico de los gaps señalados por el tutor sobre la arquitectura del proyecto de tesis, y las dos rutas disponibles — on-premise o cloud — para cerrarlos antes del corte de evaluación.

> ⚠️ **Hoy.** El tutor pidió ver el ecosistema armado **hoy martes 8 de septiembre**, con al menos **70% de avance técnico**. Sin ese avance, la materia se reprueba. La decisión de entorno (sección 3) no puede esperar.

> ✅ **Actualización 08-sept-2026.** El cliente confirmó: **on-premise con Docker**, no se usará ningún proveedor cloud. Alcance de tesis tratado como **POC/MVP**. Diagrama de arquitectura y patrones aplicados en `entregables/02-arquitectura-onpremise.md`.

---

## 1. Qué existe hoy

El notebook actual (`02-Bigdata2.ipynb`) ya resuelve la parte de ciencia de datos: descarga vía Kaggle, limpieza, modelado con árbol de decisión y comparación de rutas críticas NYC / Quito con mapas. Es código correcto — el tutor lo confirmó explícitamente. El problema no es el análisis, es que todo el flujo corre sobre archivos sueltos de Drive, sin capas de ingesta, almacenamiento ni seguridad separadas:

| Ingesta | Storage | Procesamiento | Analítica | Visualización |
|---|---|---|---|---|
| kagglehub → Drive | carpeta de Drive | Colab / pandas | árbol de decisión | Folium en el notebook |

Estado actual: cinco pasos de un mismo script, no cinco capas de un ecosistema.

---

## 2. Gaps que el tutor exige cerrar

| # | Gap | Severidad |
|---|---|---|
| 1 | **No hay arquitectura, hay un script.** El título de tesis promete una arquitectura de big data; el tutor exige ver capas separadas (ingesta, storage, procesamiento, analítica, visualización), no un pipeline lineal dentro de un notebook. | 🔴 Crítico |
| 2 | **Falta almacenamiento gestionado con patrón medallón.** Se necesita un storage real (tipo S3) con instancias Bronze / Silver / Gold, no una carpeta de Drive donde cualquiera puede leer o editar los archivos. | 🔴 Crítico |
| 3 | **Sin seguridad ni control de accesos justificado.** El tutor lo dijo directo: *"Drive no me garantiza absolutamente nada"*. Cualquier defensa va a preguntar qué seguridades tiene la infraestructura. | 🔴 Crítico |
| 4 | **Entorno de despliegue sin decidir.** On-premise (Docker, gratis) o cloud (AWS / GCP / Azure, con USD 300 de crédito inicial). Es la decisión que bloquea todo lo demás — y el motivo de este memo. | 🔴 Crítico |
| 5 | **El notebook debe re-apuntar su storage.** La limpieza y el modelo ya existentes se conservan; solo cambia el destino de lectura/escritura: de la carpeta de Drive a la instancia de storage gestionado que se elija. | 🟠 Alto |
| 6 | **Capítulo 2 sin la arquitectura documentada.** El diagrama y su justificación técnica — por qué esas herramientas, por qué ese entorno — todavía no están escritos en el documento de tesis. | 🟠 Alto |

---

## 3. La decisión: on-premise o cloud

Ambas rutas satisfacen al tutor — él mismo lo dijo: *"no le digo que utilice estas herramientas, usted tiene que ver la mejor manera"*. La diferencia es costo, tiempo de montaje y qué tan defendible se ve el resultado.

### On-premise — $0
Contenedores Docker en tu propio equipo

**Herramientas:** NiFi · Kafka · MinIO · Spark · Airflow · Vault · Power BI

**A favor**
- Cero costo, cero tarjeta
- Ya existe un ejemplo de un compañero para replicar
- Control total, sin depender de aprobaciones externas

**En contra**
- Exige PC con buenos recursos (RAM / GPU)
- Montar 5–6 contenedores a mano toma más horas
- Menos "vistoso" como caso de estudio en la defensa

**Mejor si:** no hay tarjeta disponible hoy, o el equipo local ya tiene Docker corriendo.

### Cloud — $0 inicial*
AWS · GCP · Azure — servicios gestionados

**Herramientas (ej. GCP):** Cloud Storage · Dataflow · BigQuery · Looker

**A favor**
- Sigue funcionando con Drive como origen (ya lo usas)
- SQL gestionado, mucho más fuerte que pandas para el capítulo de resultados
- Se ve como "estado del arte" en la defensa

**En contra**
- Requiere tarjeta de crédito para activar la cuenta
- Curva de aprendizaje de la consola si es la primera vez
- *Gratis solo dentro de los $300 de crédito inicial

**Mejor si:** hay tarjeta disponible hoy y se puede activar la cuenta en la próxima hora.

---

## 4. Cronograma crítico

| Fecha | Hito | Detalle |
|---|---|---|
| **Mar 08 sept — hoy** | Checkpoint del tutor | Debe verse la arquitectura armada y al menos iniciando la visualización, con ≥70% de avance técnico. De esto depende aprobar la materia. |
| Sáb 12 sept | Cierre oficial de la materia | Corte de notas: técnico + metodológico. |
| Lun 21 sept | Proyecto completo (fecha extraoficial) | Todo listo para defensa, según lo solicitado por el tutor a coordinación. |
| Mié 23 – Jue 24 sept | Defensa (fecha propuesta) | Sujeta a confirmación de coordinación académica. |
| ⚠️ Si no se llega al 70% | Se reprueba la materia | Segunda matrícula (costo económico) o egreso solo con el trabajo de titulación pendiente — próxima ventana de defensa recién ~abril 2027. |

---

## 5. Recomendación

**Con el checkpoint de hoy, la velocidad manda.**

Si hay tarjeta disponible y se puede activar la cuenta cloud hoy mismo, **GCP** es el camino de menor fricción: el origen sigue siendo Drive (tal como ya se usa), y Dataflow + BigQuery dan un salto grande en cómo se ve el capítulo de resultados. Si la tarjeta no está lista hoy — y esto es lo que hay que confirmar con el cliente antes de las 11am — el camino **on-premise con Docker** es la apuesta más segura para llegar al 70% sin depender de una aprobación externa.

> **Acción inmediata:** confirmar con el cliente en las próximas horas si autoriza el uso de tarjeta para el crédito cloud. Esa única respuesta define cuál de las dos arquitecturas (entregable siguiente) se construye primero.

---

## 6. Próximos pasos

1. **Confirmar entorno con el cliente** — respuesta sobre disponibilidad de tarjeta / presupuesto, hoy antes del mediodía.
2. **Entregar diagrama de arquitectura** — propuesta on-premise y propuesta cloud, ambas mapeadas al caso de uso NYC / Quito ya construido.
3. **Adaptar el notebook** — re-apuntar lectura/escritura del script actual al storage gestionado elegido (medallón Bronze/Silver/Gold).
4. **Redactar capítulo 2** — documentar el diagrama y su justificación técnica para el porcentaje de avance metodológico.

---
*Preparado para el checkpoint del 08 sept 2026 · Fuente: tutoría de tesis, transcripción 01*
