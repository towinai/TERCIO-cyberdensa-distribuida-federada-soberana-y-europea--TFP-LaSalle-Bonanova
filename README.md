# 🛡️ Simulador TERCIO

> Simulador de detección distribuida de amenazas de ciberseguridad basado en inteligencia colectiva sectorial. Modela miles de nodos de empresas españolas, genera eventos de seguridad sintéticos por sector y demuestra cuantitativamente que la detección cooperativa supera a la protección individual.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-ready-F9AB00.svg)](https://colab.research.google.com/)
[![Gemini API](https://img.shields.io/badge/Gemini-API-4285F4.svg)](https://ai.google.dev/)
[![Estado](https://img.shields.io/badge/estado-en%20desarrollo-yellow.svg)]()
[![License](https://img.shields.io/badge/license-pendiente-lightgrey.svg)]()

---

## 📑 Tabla de contenidos

- [Descripción](#-descripción)
- [Demostración](#-demostración)
- [Primeros pasos](#-primeros-pasos)
- [Uso](#-uso)
- [Tecnologías utilizadas](#-tecnologías-utilizadas)
- [Estructura del proyecto](#-estructura-del-proyecto)
- [Cómo contribuir](#-cómo-contribuir)
- [Créditos y autoría](#-créditos-y-autoría)
- [Licencia](#-licencia)

---

## 📖 Descripción

Las PYMEs españolas afrontan amenazas de ciberseguridad cada vez más sofisticadas con recursos limitados. Un antivirus tradicional protege un nodo aislado, pero no ve la red: no sabe que una clínica recibió el mismo ransomware que otra empresa del mismo sector horas antes.

**TERCIO** es la red de inteligencia colectiva sectorial de [TOWIN-IA](https://towinia.es) (TYBSI SL, Zamora). Este simulador permite:

- Generar **datos sintéticos de eventos de seguridad** parametrizados por sector (sanidad, despachos legales, industria, finanzas, administración, educación, hostelería e informática), con perfiles de amenaza basados en MITRE ATT&CK.
- Inyectar **campañas de ataque coordinadas** con ground truth para evaluar modelos.
- Clasificar eventos con un **LLM (Gemini)** actuando como analista CISO.
- Modelar la propagación **gossip** de alertas entre nodos del mismo sector y medir el tiempo de detección (MTTD) frente a nodos aislados.

Es útil como validación cuantitativa de la arquitectura TERCIO antes de su despliegue real, y como caso práctico reproducible de data science aplicado a ciberseguridad. Todo el proyecto se ejecuta en Google Colab, sin infraestructura propietaria.

---

## 🎬 Demostración

<!-- Sustituir por capturas reales cuando estén disponibles -->
| Captura | Descripción |
|---|---|
| `docs/img/eda_sectores.png` | Distribución de eventos y amenazas por sector (NB 01) |
| `docs/img/heatmap_amenazas.png` | Heatmap de amenazas por sector y día (NB 01) |
| `docs/img/home_tercio.png` | Sección de presentación de TERCIO (`seccion_tercio_home_v4.html`) |

Notebook de demostración: *(pendiente de enlazar, por ejemplo un enlace de Colab a `NB01_Simulador_TERCIO_v2.ipynb`)*

---

## 🚀 Primeros pasos

### Prerrequisitos

- Cuenta de **Google** con acceso a [Google Colab](https://colab.research.google.com/)
- **API key de Gemini** obtenida en [Google AI Studio](https://aistudio.google.com/)
- **Python 3.10+** (ya incluido en Colab; solo necesario si se ejecuta en local)
- Opcional: GPU T4 en Colab (no es obligatoria)

### Instalación

El proyecto no requiere instalación local: las dependencias se instalan desde la primera celda de cada notebook.

```bash
git clone https://github.com/<usuario>/simulador-tercio.git
cd simulador-tercio
```

Para ejecución en local, instala las dependencias del notebook 01:

```bash
pip install -q google-genai pandas numpy faker plotly pyarrow
```

### Configuración de la API key

En Google Colab, guarda la clave en **Secrets** (icono de llave en la barra lateral) con el nombre `GOOGLE_API_KEY`. El notebook la lee automáticamente con `google.colab.userdata`.

> ⚠️ **No escribas nunca la API key directamente en el código ni la subas al repositorio.** Si la clave se ha compartido en algún fichero, revócala en Google AI Studio y genera una nueva.

---

## 💻 Uso

1. Abre `NB01_Simulador_TERCIO_v2.ipynb` en Google Colab.
2. Configura `GOOGLE_API_KEY` en Colab Secrets.
3. Ejecuta las celdas en orden (`Entorno de ejecución → Ejecutar todo`).
4. El notebook:
   - Verifica la conexión con Gemini.
   - Genera 8 sectores con sus nodos TERCIO (`TERCIO-{SECTOR}-{ID}`).
   - Produce el dataset de eventos (30 días, ~5 % de amenazas) con 15 campañas inyectadas.
   - Realiza el análisis exploratorio con visualizaciones Plotly.
   - Clasifica una muestra de eventos con el LLM.
   - Guarda los resultados en Google Drive como Parquet en `SIMULADOR_TERCIO/data/`.

Ejemplo de evento generado:

```json
{
  "timestamp": "2026-03-14T03:22:15",
  "node_id": "TERCIO-SAN-0042",
  "sector": "SEC-SAN",
  "event_type": "ransomware",
  "severity": 4,
  "hash_proceso": "3f2a…",
  "is_threat": true
}
```

---

## 🧰 Tecnologías utilizadas

| Categoría | Herramientas |
|---|---|
| Lenguaje | Python 3.10+ |
| Datos | Pandas, NumPy, Faker, PyArrow (Parquet) |
| Machine Learning | Scikit-learn (Isolation Forest, DBSCAN), SciPy, Statsmodels |
| Grafos y red | NetworkX |
| Visualización | Plotly, Matplotlib, Seaborn |
| LLM | Google Gemini API (`google-genai`) |
| Entorno | Google Colab + Google Drive |

---

## 📁 Estructura del proyecto

```text
simulador-tercio/
├── NB01_Simulador_TERCIO_v2.ipynb          # Generación de datos + conexión Gemini (versión vigente)
├── NB01_Simulador_TERCIO_Data_Generation.ipynb  # Versión anterior de NB 01 (referencia)
├── NB02_*.ipynb                             # Modelado de red (planificado)
├── NB03_*.ipynb                             # Detección de anomalías (planificado)
├── NB04_*.ipynb                             # Simulación gossip (planificado)
├── NB05_*.ipynb                             # Resultados y conclusiones (planificado)
├── seccion_tercio_home_v4.html              # Sección de presentación de TERCIO
├── Simulador_TERCIO_TFM_Propuesta.md       # Propuesta del TFM en Markdown
├── docs/
│   └── img/                                 # Capturas y figuras
└── README.md
```

Los datos generados (`eventos.parquet`, `nodos.parquet`, `campanas.parquet`) no se versionan en el repositorio: se persisten en Google Drive durante la ejecución.

---

## 🗺️ Plan de notebooks

| Notebook | Contenido | Estado |
|---|---|---|
| 01 · Data Generation | Perfiles de amenaza, nodos, eventos, campañas, EDA y prueba LLM | ✅ Disponible |
| 02 · Network Modeling | Topología sectorial con NetworkX y asignación de vecinos gossip | 🔜 Planificado |
| 03 · Anomaly Detection | Isolation Forest, DBSCAN y z-score sobre series temporales | 🔜 Planificado |
| 04 · Gossip Simulation | Propagación de alertas, regla de consenso 3+ nodos, MTTD y MTTC | 🔜 Planificado |
| 05 · Results | Visualizaciones finales, pruebas estadísticas y conclusiones | 🔜 Planificado |

Las métricas objetivo (MTTD, MTTC, cobertura, FPR, F1 y speedup) están definidas en la [propuesta del TFM](Simulador_TERCIO_TFM_Propuesta.md#8-métricas-de-evaluación).

---

## 🤝 Cómo contribuir

Las contribuciones son bienvenidas.

1. Haz un *fork* del repositorio.
2. Crea una rama descriptiva: `git checkout -b feature/nb02-topologia-gossip`.
3. Haz *commits* claros, en imperativo y en español o inglés, por ejemplo: `Añade asignación de vecinos por sector`.
4. Abre un **Pull Request** explicando qué cambia y cómo se ha probado.

**Reportar errores:** abre un *Issue* indicando el notebook, la celda, el mensaje de error y la versión de las librerías.

**Ideas o mejoras:** abre un *Issue* con la etiqueta `enhancement` antes de implementar cambios grandes.

---

## 👤 Créditos y autoría

**Enmanuel Torras Mata**
Experto Universitario en Data Science & Big Data — La Salle, Universitat Ramon Llull
Proyecto desarrollado en el marco de **TYBSI SL — TOWIN-IA** (Zamora, España).

- 🌐 [towinia.es](https://towinia.es)
- ✉️ onpremise@towinia.es
- 💼 LinkedIn: *(pendiente de enlazar)*

---

## 📄 Licencia

Licencia Apache 2.0

---

*«La red no existe para sobrevivir. Existe para defender.»*
