# Proyecto integrador · Equipo Online Shopper

Curso Despliegue y monitoreo de Machine Learning.

| | |
|---|---|
| Dataset | [Online Shoppers Purchasing Intention](https://archive.ics.uci.edu/dataset/468/online+shoppers+purchasing+intention+dataset) (UCI) · `data/raw/online_shoppers_intention.csv`: 12 330 sesiones de una tienda online, 17 variables + `Revenue` |
| Pregunta de negocio | ¿Una sesión de navegación terminará en compra (`Revenue`)? Sirve para dirigir promociones o ayuda en tiempo real a las sesiones con alta probabilidad de comprar. |
| Métrica de éxito | F1 macro ≥ 0.75 en test (clases desbalanceadas: 15.5 % de las sesiones compran) |
| Integrantes y roles | **Melani Tirado** · versionado del dataset con DVC, remote y registro en MLflow <br> **Luis Orduz** · pipeline de tres etapas (`dvc.yaml`) y revisión de PR <br> **Andrea Marquez** · revisión y aprobación de PR |

## Cómo reproducir
```bash
pip install -r requirements.txt
dvc pull
dvc repro
```

## Pipeline y resultados
`dvc repro` ejecuta tres etapas definidas en `dvc.yaml`:

```
data/raw/online_shoppers_intention.csv → prepare → data/prepared → train → models/model.pkl → evaluate → metrics.json
```

| Archivo | Qué contiene |
|---|---|
| `params.yaml` | Parámetros del pipeline: `target: Revenue`, `test_size: 0.2`, `random_state: 42`, `n_estimators: 200`, `max_depth: 10`, y el experimento y modelo de MLflow |
| `dvc.lock` | Hash de cada entrada, salida y parámetro de la última ejecución; DVC lo usa para saber qué etapas volver a correr |
| `metrics.json` | Métricas del modelo en test: **accuracy 0.8933 · F1 macro 0.7803** (cumple la meta ≥ 0.75) |

Cada ejecución de `train` queda registrada en MLflow (experimento `integrador-equipo-online-shopper`, modelo `modelo-equipo-online-shopper`). Para verlo:
```bash
mlflow ui --backend-store-uri sqlite:///mlflow.db
```

## Estructura
```
data/raw/        dataset original (versionado con DVC)
data/prepared/   salida de la etapa prepare
models/          modelo entrenado
src/             scripts de cada etapa (prepare.py, train.py, evaluate.py)
labs/            guías de los laboratorios, una carpeta por fin de semana (FDS1, FDS2, FDS3)
params.yaml      parámetros del pipeline
dvc.yaml         definición del pipeline
dvc.lock         estado de la última ejecución del pipeline
metrics.json     métricas del modelo en test
```

## Entregas
Cada hito se entrega con una rama `entrega/eN` y un Pull Request hacia `master`. Nadie hace commit directo en `master` ni en `entrega/*`: todo entra por ramas de trabajo (`feat/`, `fix/`, `exp/`, `docs/`) y PR aprobados. Convención de ramas y paso a paso: [docs/entregas.md](docs/entregas.md).

## Laboratorios del fin de semana 1
1. [Entorno de trabajo](labs/FDS1/01-entorno.md)
2. [Versionar el dataset con DVC](labs/FDS1/02-dvc.md)
3. [Pipeline reproducible](labs/FDS1/03-pipeline.md)
4. [MLflow en el pipeline](labs/FDS1/04-mlflow.md)
