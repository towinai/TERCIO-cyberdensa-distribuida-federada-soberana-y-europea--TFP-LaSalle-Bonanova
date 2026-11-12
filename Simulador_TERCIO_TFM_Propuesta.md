**EXPERTO UNIVERSITARIO**

**DATA SCIENCE & BIG DATA**

**SIMULADOR TERCIO**

Detección Distribuida de Amenazas

*Modelado y análisis de una red de inteligencia colectiva*

*sectorial para ciberseguridad empresarial soberana*

**Enmanuel Torras Mata**

La Salle — Universitat Ramon Llull

Abril 2026

TYBSI SL — TOWIN-IA — Zamora, España

1\. Resumen Ejecutivo

Este proyecto desarrolla un simulador completo de la red TERCIO de TOWIN-IA: un sistema de detección distribuida de amenazas de ciberseguridad basado en inteligencia colectiva sectorial. El simulador modela miles de nodos desplegados en empresas españolas, genera eventos de seguridad sintéticos por sector (sanidad, legal, industria, finanzas) y aplica técnicas de ciencia de datos para demostrar que la detección cooperativa supera significativamente a la protección individual.

El entorno de ejecución es Google Colab, garantizando reproducibilidad total del proyecto sin dependencia de infraestructura propietaria.

|                 |                                                                                               |
|-----------------|-----------------------------------------------------------------------------------------------|
| **Parámetro**   | **Detalle**                                                                                   |
| **Título**      | Simulador TERCIO: Detección Distribuida de Amenazas mediante Inteligencia Colectiva Sectorial |
| **Alumno**      | Enmanuel Torras Mata                                                                          |
| **Programa**    | Experto Universitario en Data Science & Big Data — La Salle, Universitat Ramon Llull          |
| **Entorno**     | Google Colab (Python 3.x, GPU T4 opcional)                                                    |
| **Empresa**     | TYBSI SL — TOWIN-IA (Zamora, España)                                                          |
| **Sector**      | Ciberseguridad soberana — IA on-premise                                                       |
| **Tecnologías** | Python, Pandas, Scikit-learn, NetworkX, Plotly, Faker                                         |

2\. Contexto y Motivación

2.1 El problema

Las PYMEs españolas se enfrentan a amenazas de ciberseguridad cada vez más sofisticadas con recursos limitados. Un antivirus tradicional protege un nodo individual, pero carece de visión de red: no sabe que la clínica dental de Burgos recibió el mismo ransomware que la farmacia de León hace dos horas.

2.2 La hipótesis TERCIO

TOWIN-IA propone una red de inteligencia colectiva donde nodos agrupados por sector económico comparten patrones de amenaza anonimizados mediante un protocolo gossip. La hipótesis central es:

***H₀: Una red de N nodos sectoriales con gossip protocol detecta amenazas emergentes***

***en un tiempo T significativamente menor que N nodos aislados operando de forma independiente.***

2.3 Marco regulatorio

La Directiva NIS2 (transpuesta en España en 2024) obliga a sectores críticos a implementar medidas de ciberseguridad avanzadas. Esto crea un mercado de más de 6.900 empresas españolas que necesitan soluciones de detección y respuesta, posicionando este proyecto como validación académica de un producto con demanda real.

3\. Objetivos del Proyecto

3.1 Objetivo general

Diseñar, implementar y validar un simulador de red de detección distribuida de amenazas que demuestre cuantitativamente la ventaja de la inteligencia colectiva sectorial frente a la protección individual.

3.2 Objetivos específicos

1.  Construir un generador de datos sintéticos de eventos de seguridad parametrizado por sector, con perfiles de amenaza realistas (ransomware, phishing, BEC, APT).

2.  Modelar una red de nodos TERCIO con topología sectorial y protocolo gossip configurable (TTL, vecinos, latencia).

3.  Implementar un pipeline de detección de anomalías sobre los flujos de eventos agregados: Isolation Forest, DBSCAN, y análisis de series temporales.

4.  Simular la propagación de alertas gossip y medir el tiempo medio de detección (MTTD) comparando red cooperativa vs. nodos aislados.

5.  Modelar el mecanismo de consenso sectorial (regla de 3+ nodos) y su efecto en la reducción de falsos positivos.

6.  Visualizar la dinámica de la red: grafos de propagación, heatmaps sectoriales, curvas de detección acumulada.

7.  Documentar conclusiones y recomendaciones para el despliegue real de la red TERCIO.

4\. Metodología

El proyecto sigue la metodología CRISP-DM adaptada a simulación, con cinco fases ejecutadas en notebooks de Google Colab:

|          |                    |                                                                                                       |                     |
|----------|--------------------|-------------------------------------------------------------------------------------------------------|---------------------|
| **Fase** | **Nombre**         | **Descripción**                                                                                       | **Notebook**        |
| **1**    | Data Generation    | Generador paramétrico de eventos de seguridad por sector. Perfiles de ataque basados en MITRE ATT&CK. | 01_data_gen.ipynb   |
| **2**    | Network Modeling   | Construcción de la topología de red sectorial con NetworkX. Asignación de vecinos gossip.             | 02_network.ipynb    |
| **3**    | Anomaly Detection  | Pipeline ML: Isolation Forest, DBSCAN, z-score sobre series temporales de eventos.                    | 03_detection.ipynb  |
| **4**    | Gossip Simulation  | Simulación de propagación de alertas. Medición de MTTD, cobertura, consenso.                          | 04_gossip_sim.ipynb |
| **5**    | Analysis & Results | Visualizaciones, pruebas estadísticas, conclusiones. Comparativa aislado vs. red.                     | 05_results.ipynb    |

5\. Arquitectura del Simulador

5.1 Capa de datos sintéticos

El generador produce eventos de seguridad con la siguiente estructura por registro:

- timestamp: momento del evento (resolución de segundos)

- node_id: identificador único del nodo TERCIO (ej: TERCIO-SAN-0042)

- sector: código sectorial (SEC-SAN, SEC-LEG, SEC-IND, SEC-FIN, SEC-ADM, SEC-EDU, SEC-HOS, SEC-INF)

- event_type: tipo de evento (normal_traffic, port_scan, brute_force, ransomware, phishing, lateral_movement, c2_beacon, data_exfiltration)

- severity: nivel 0–4 (info, low, medium, high, critical)

- hash_proceso: SHA-256 del proceso asociado (para correlación gossip)

- is_threat: etiqueta binaria ground truth (para evaluación del modelo)

Parámetros configurables: número de nodos por sector, ratio amenaza/ruido, distribución temporal de campañas, correlación entre sectores.

5.2 Capa de red

Cada nodo TERCIO se modela como un vértice en un grafo dirigido sectorial. La topología sigue el diseño real documentado en la doctrina TOWIN-IA:

- Cada nodo tiene 5–10 vecinos del mismo sector asignados por Zamora (nodo central)

- Los mensajes gossip tienen TTL=3 (máximo 3 saltos de propagación)

- Latencia de propagación configurable (simula retardos reales de red)

- Heartbeat cada 60s — nodos sin heartbeat en 24h se eliminan del vecindario

5.3 Capa de detección

Tres algoritmos de detección de anomalías operan en paralelo sobre los flujos de eventos:

|                      |                                                                                               |                     |
|----------------------|-----------------------------------------------------------------------------------------------|---------------------|
| **Algoritmo**        | **Aplicación**                                                                                | **Librería**        |
| **Isolation Forest** | Detección de eventos atípicos en el espacio de features (frecuencia, severidad, hora, sector) | scikit-learn        |
| **DBSCAN**           | Clustering de procesos sospechosos por hash — identifica campañas coordinadas                 | scikit-learn        |
| **Z-score temporal** | Análisis de series temporales de eventos por sector — detecta picos anómalos                  | scipy / statsmodels |

5.4 Capa de consenso gossip

Implementación de la regla de consenso TERCIO: si 3 o más vecinos sectoriales confirman el mismo hash de proceso sospechoso en menos de 2 horas, el nivel de alerta sube automáticamente y se genera una alerta consolidada sectorial (potencial PROCLAMA).

Este mecanismo se evalúa midiendo: tasa de verdaderos positivos del consenso, reducción de falsos positivos respecto a detección individual, y tiempo medio hasta consenso (MTTC).

6\. Plan de Notebooks

Notebook 01: Generación de Datos Sintéticos

- Definición de perfiles de amenaza por sector (basados en MITRE ATT&CK)

- Generación de dataset: 1.000–5.000 nodos, 500K–2M eventos, ventana temporal de 30 días

- Inyección controlada de campañas de ataque sectoriales con ground truth

- EDA inicial: distribución de eventos por sector, severidad, tipo

- Persistencia en Google Drive como Parquet

Notebook 02: Modelado de Red

- Construcción de grafos sectoriales con NetworkX

- Algoritmo de asignación de vecinos (5–10 por nodo, mismo sector)

- Análisis de topología: grado medio, diámetro, coeficiente de clustering por sector

- Visualización de la red con colores por sector

Notebook 03: Detección de Anomalías

- Feature engineering: ventanas temporales, frecuencias por nodo, entropía de tipos

- Entrenamiento de Isolation Forest y DBSCAN

- Evaluación con ground truth: precision, recall, F1-score, AUC-ROC

- Análisis de series temporales con z-score y detección de changepoints

Notebook 04: Simulación Gossip

- Motor de simulación de propagación gossip (event-driven, discrete time)

- Implementación de regla de consenso: 3+ nodos confirman → alerta sectorial

- Experimentos: variando TTL (1–5), número de vecinos (3–15), latencia (1s–60s)

- Métricas principales: MTTD (Mean Time To Detection), MTTC (Mean Time To Consensus), cobertura

- Comparativa: red cooperativa vs. nodos aislados vs. nodo centralísta

Notebook 05: Resultados y Conclusiones

- Visualizaciones finales: heatmaps sectoriales, curvas de detección acumulada, grafos de propagación animados

- Pruebas estadísticas de significancia (test de hipótesis sobre MTTD)

- Análisis de sensibilidad: ¿cuántos nodos necesita cada sector para que el gossip sea efectivo?

- Conclusiones y recomendaciones para despliegue real

- Limitaciones y trabajo futuro

7\. Stack Tecnológico

|                       |                             |                                           |
|-----------------------|-----------------------------|-------------------------------------------|
| **Categoría**         | **Herramienta**             | **Uso en el proyecto**                    |
| **Entorno**           | Google Colab + Drive        | Ejecución y persistencia                  |
| **Datos**             | Pandas, Faker, NumPy        | Generación y manipulación de datasets     |
| **ML / Anomalías**    | Scikit-learn, SciPy         | Isolation Forest, DBSCAN, z-score         |
| **Grafos / Red**      | NetworkX                    | Topología TERCIO, propagación gossip      |
| **Series temporales** | Statsmodels                 | Detección de changepoints, tendencias     |
| **Visualización**     | Plotly, Matplotlib, Seaborn | Grafos interactivos, heatmaps, dashboards |
| **Almacenamiento**    | Parquet en Google Drive     | Persistencia entre sesiones Colab         |

8\. Métricas de Evaluación

|               |                                                                                          |                    |
|---------------|------------------------------------------------------------------------------------------|--------------------|
| **Métrica**   | **Definición**                                                                           | **Objetivo**       |
| **MTTD**      | Mean Time To Detection — tiempo medio desde inyección de amenaza hasta primera detección | Red \< Aislado     |
| **MTTC**      | Mean Time To Consensus — tiempo medio hasta que 3+ nodos confirman la amenaza            | \< 2 horas         |
| **Cobertura** | Porcentaje de nodos del sector que reciben la alerta gossip antes del impacto            | \> 80%             |
| **FPR**       | False Positive Rate — tasa de falsas alarmas del consenso vs. detección individual       | Consenso \< Indiv. |
| **F1-Score**  | Media armónica precision/recall sobre detección de amenazas inyectadas                   | \> 0.75            |
| **Speedup**   | Ratio MTTD(aislado) / MTTD(red) — cuántas veces más rápido detecta la red                | \> 3x              |

9\. Cronograma Estimado

|            |                                                      |                 |                |
|------------|------------------------------------------------------|-----------------|----------------|
| **Semana** | **Tarea**                                            | **Entregable**  | **Horas est.** |
| **1–2**    | Diseño del generador de datos + perfiles sectoriales | NB 01 completo  | 12h            |
| **3**      | Modelado de red y topología gossip                   | NB 02 completo  | 6h             |
| **4–5**    | Pipeline de detección de anomalías + evaluación      | NB 03 completo  | 12h            |
| **6–7**    | Simulación gossip + experimentos comparativos        | NB 04 completo  | 12h            |
| **8**      | Resultados, visualizaciones, memoria final           | NB 05 + Memoria | 8h             |

*Total estimado: 50 horas de trabajo efectivo.*

10\. Valor Añadido

10.1 Valor académico

- Aplica técnicas reales de data science (anomaly detection, graph analysis, time series) a un problema de ciberseguridad

- Incluye generación de datos sintéticos — competencia clave en data science cuando no hay datos reales disponibles

- Combina simulación de sistemas distribuidos con análisis estadístico riguroso

- Reproducible al 100% en Google Colab — cualquier evaluador puede ejecutar los notebooks

10.2 Valor empresarial

- Valida cuantitativamente la arquitectura de red TERCIO antes del despliegue real

- Proporciona parámetros óptimos de configuración: número de vecinos, TTL, threshold de consenso

- Genera material publicable para LinkedIn y comunicación técnica de TOWIN-IA

- Un TFM universitario de La Salle Ramon Llull validando la arquitectura es credibilidad que no se compra

10.3 Potencial de publicación

- Publicación del simulador como repositorio open-source en GitHub

- Artículo en LinkedIn con resultados clave — demostración pública del concepto TERCIO

- Base para un posible paper académico en conferencias de ciberseguridad o sistemas distribuidos

11\. Referencias y Fuentes

- TOWIN-IA: Manifiesto Fundacional v2 — Operación Reconquista Digital (TYBSI SL, 2025)

- TOWIN-IA: Doctrina Secretario · Red · Doctrina — Gossip Sectorial y Conclaves (TYBSI SL, 2026)

- Directiva (UE) 2022/2555 (NIS2) — Parlamento Europeo y Consejo, 14 de diciembre de 2022

- MITRE ATT&CK Framework — https://attack.mitre.org/

- Liu, F.T., Ting, K.M. & Zhou, Z.H. (2008). Isolation Forest. IEEE ICDM.

- Ester, M. et al. (1996). A Density-Based Algorithm for Discovering Clusters (DBSCAN). KDD.

- Demers, A. et al. (1987). Epidemic Algorithms for Replicated Database Maintenance. ACM PODC.

- NetworkX — https://networkx.org/

- Scikit-learn: Machine Learning in Python — https://scikit-learn.org/

**TYBSI SL — TOWIN-IA**

Zamora, España — onpremise@towinia.es

*«La red no existe para sobrevivir. Existe para defender.»*
