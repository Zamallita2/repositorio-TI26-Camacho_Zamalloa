# Proyecto TI26 — Detección de Fraude Bancario

Modelo de machine learning para la detección de fraude en transacciones bancarias usando PySpark.

---

## Estructura del proyecto

```
repositorio-TI26-Camacho_Zamalloa/
├── data/
│   ├── raw/
│   │   ├── Bank.zip            # Dataset comprimido (se sube al repo)
│   │   └── bank_fraud.csv      # ⚠️ NO subir al repo (.gitignore)
│   └── processed/
│       └── bank_fraud_clean.csv  # Generado por el notebook de preprocesamiento
├── notebooks/
│   ├── 01_EDA.ipynb            # Análisis Exploratorio de Datos
│   ├── 02_preprocessing.ipynb  # Limpieza y balanceo de datos
│   ├── 03_model_main.ipynb     # Modelo principal
│   └── 04_model_comparison.ipynb  # Comparación de modelos
├── requirements.txt
└── README.md
```

---

## Requisitos previos

- Python 3.10 o superior
- Java 11 o superior (requerido por PySpark)

Verificar que Java está instalado:
```bash
java -version
```

Si no está instalado:
```bash
sudo apt install openjdk-11-jdk
```

---

## Instalación

### 1. Clonar el repositorio

```bash
git clone <url-del-repositorio>
cd repositorio-TI26-Camacho_Zamalloa
```

### 2. Descomprimir los datos

El archivo CSV es demasiado grande para subirse al repositorio, por lo que solo se incluye el ZIP. Descomprímelo en la carpeta correcta:

```bash
cd data/raw
unzip Bank.zip
cd ../..
```

Esto generará el archivo `data/raw/bank_fraud.csv`.

### 3. Crear el entorno virtual e instalar dependencias

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt pyspark
```

> **Nota:** La instalación de PySpark puede tardar varios minutos (~450 MB).

---

## Uso

Asegúrate de tener el entorno virtual activado antes de correr cualquier notebook:

```bash
source .venv/bin/activate
```

Luego lanza Jupyter:

```bash
jupyter notebook
```

Ejecuta los notebooks en el siguiente orden:

| Notebook | Descripción |
|---|---|
| `01_EDA.ipynb` | Análisis exploratorio: estadísticas, gráficos y correlaciones con `is_fraud` |
| `02_preprocessing.ipynb` | Limpieza de nulos, selección de columnas y balanceo del dataset (1:3) |
| `03_model_main.ipynb` | Entrenamiento del modelo principal |
| `04_model_comparison.ipynb` | Comparación de modelos |

---

## Datos del dataset

El dataset `bank_fraud.csv` contiene ~1,000,000 de transacciones bancarias con las siguientes columnas relevantes:

**Columnas numéricas utilizadas:**
`hour_of_day`, `is_weekend`, `is_night_transaction`, `customer_age`, `credit_score`, `account_age_years`, `account_balance`, `transaction_amount`, `num_prev_transactions`, `transaction_freq_monthly`, `distance_from_home_km`, `time_since_last_txn_hrs`, `is_international`, `failed_attempts`, `pin_changed_recently`

**Columnas categóricas utilizadas:**
`merchant_category`, `payment_method`, `device_type`

**Variable objetivo:** `is_fraud` (0 = legítima, 1 = fraude)

**Tipos de fraude presentes:** Phishing, Account Takeover, Synthetic Identity, Card Cloning, Friendly Fraud, Identity Theft

---

## Preprocesamiento aplicado

1. **Eliminación de nulos** — se descartan todas las filas incompletas.
2. **Selección de columnas** — se conservan 15 columnas numéricas + 3 categóricas + el target.
3. **Balanceo de clases (Undersampling 1:3)** — el dataset original tiene ~55k fraudes vs ~944k no-fraudes. Se conservan todos los fraudes y se toma una muestra aleatoria de no-fraudes en proporción 1:3, resultando en ~220k filas totales. El ratio es configurable en el notebook.
