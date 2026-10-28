# Laboratorio 3 · Pipeline reproducible con DVC

Sábado del fin de semana 1 · 40 min · por equipos · hito E1

## Objetivo
Convertir el entrenamiento en un pipeline de tres etapas que cualquiera puede volver a correr con `dvc repro`.

```
dataset.csv → prepare → data/prepared → train → models/model.pkl → evaluate → metrics.json
```

Los scripts ya están en `src/`: `prepare.py`, `train.py` y `evaluate.py`. Leen su configuración de `params.yaml`.

## Pasos

### 0. Rama de trabajo
```bash
git switch entrega/e1
git pull
git switch -c feat/pipeline-tres-etapas
```

### 1. Revisar params.yaml
Confirmen que `target` y `drop_columns` corresponden a su dataset.

### 2. Escribir dvc.yaml
Creen el archivo `dvc.yaml` en la raíz del repositorio:
```yaml
stages:
  prepare:
    cmd: python src/prepare.py
    deps:
      - src/prepare.py
      - data/raw/dataset.csv
    params:
      - prepare
    outs:
      - data/prepared

  train:
    cmd: python src/train.py
    deps:
      - src/train.py
      - data/prepared
    params:
      - train
    outs:
      - models/model.pkl

  evaluate:
    cmd: python src/evaluate.py
    deps:
      - src/evaluate.py
      - data/prepared
      - models/model.pkl
    metrics:
      - metrics.json:
          cache: false
```

### 3. Correr y guardar
```bash
dvc repro
dvc metrics show
git add dvc.yaml dvc.lock params.yaml metrics.json data/.gitignore models/.gitignore
git commit -m "Pipeline de tres etapas"
dvc push
```
`dvc repro` crea `data/.gitignore` y `models/.gitignore` para que Git no suba las salidas del pipeline. Después del commit, `git status` no debería mostrar nada pendiente.

### 4. Cambiar un parámetro y comparar
Cambien `max_depth` en `params.yaml` (por ejemplo de 5 a 10) y:
```bash
dvc repro          # fíjense que se salta prepare
dvc params diff
dvc metrics diff
```
Si el resultado es mejor, hagan commit. Si no, `git checkout params.yaml` y `dvc repro` para volver.

### 5. Pull Request hacia entrega/e1
```bash
git push -u origin feat/pipeline-tres-etapas
```
En GitHub: **Pull requests › New pull request**, con `base: entrega/e1` ← `compare: feat/pipeline-tres-etapas`. Borren la plantilla de entrega y escriban qué cambia. Otro integrante aprueba (*Files changed › Review changes › Approve*), hace el merge y borra la rama. Después, todos: `git switch entrega/e1` y `git pull`.

## Listo cuando
- [ ] `dvc repro` corre las tres etapas sin error.
- [ ] `metrics.json` y `dvc.lock` están en el historial.
- [ ] El PR de `feat/pipeline-tres-etapas` está aprobado y unido a `entrega/e1`.

## Si su problema es de regresión
(Bike Sharing, Appliances Energy, Wine Quality)
- En `src/train.py`: cambiar `RandomForestClassifier` por `RandomForestRegressor`.
- En `src/evaluate.py`: cambiar las métricas por `mean_absolute_error` y `r2_score` de `sklearn.metrics`.

## Errores frecuentes
- **`[WinError 3] El sistema no puede encontrar la ruta especificada`** al guardar una etapa: la carpeta del repositorio tiene una ruta demasiado larga para Windows. Mover el repositorio a una ruta corta, por ejemplo `C:\mlops\`, y repetir.
- **`KeyError` con el nombre de la columna**: `target` en `params.yaml` no coincide exactamente con el encabezado del CSV (mayúsculas, espacios).
- **El modelo tarda mucho o se queda sin memoria**: bajar `n_estimators` o agregar columnas de texto libre a `drop_columns`.
- **`dvc repro` dice que no hay nada que hacer** o que una etapa «is cached»: no cambió ninguna dependencia ni parámetro, o esa misma combinación ya se corrió antes y DVC reutiliza el resultado guardado (run-cache) en vez de volver a ejecutar. Es lo esperado.
- **`ERROR: output 'data/prepared' is already specified in stage`**: el nombre está repetido en dos etapas; cada salida pertenece a una sola etapa.
