# Proyecto integrador · Equipo Online Shopper

Curso Despliegue y monitoreo de Machine Learning.

| | |
|---|---|
| Dataset | [Online Shoppers](https://archive.ics.uci.edu/dataset/468/online+shoppers+purchasing+intention+dataset) (UCI): 12 330 sesiones de una tienda online, 17 variables |
| Pregunta de negocio | ¿Una sesión de navegación terminará en compra (`Revenue`)? Sirve para dirigir promociones o ayuda en tiempo real a las sesiones con alta probabilidad de comprar. |
| Métrica de éxito | F1 macro de al menos 0.75 en test (las clases están desbalanceadas: ~15 % de las sesiones compran). Actual: F1 macro 0.78 · accuracy 0.89 |
| Integrantes y roles | Melani Tirado · [Colaborador] <br> Luis Orduz · [Colaborador] <br> Yirleidis Marquez · [Colaborador] |

## Cómo reproducir
```bash
pip install -r requirements.txt
dvc pull
dvc repro
```

## Estructura
```
data/raw/        dataset original (versionado con DVC) 
data/prepared/   salida de la etapa prepare
models/          modelo entrenado
src/             scripts de cada etapa
labs/            guías de los laboratorios, una carpeta por fin de semana (FDS1, FDS2, FDS3)
params.yaml      parámetros del pipeline
dvc.yaml         definición del pipeline (se crea en el laboratorio 3)
```

## Entregas
Cada hito se entrega con una rama `entrega/eN` y un Pull Request hacia `master`. Nadie hace commit directo en `master` ni en `entrega/*`: todo entra por ramas de trabajo (`feat/`, `fix/`, `exp/`, `docs/`) y PR aprobados. Convención de ramas y paso a paso: [docs/entregas.md](docs/entregas.md).

## Laboratorios del fin de semana 1
1. [Entorno de trabajo](labs/FDS1/01-entorno.md)
2. [Versionar el dataset con DVC](labs/FDS1/02-dvc.md)
3. [Pipeline reproducible](labs/FDS1/03-pipeline.md)
4. [MLflow en el pipeline](labs/FDS1/04-mlflow.md)
