# Errores frecuentes del curso

Se completa entre todos en cada puesta en común. Formato: síntoma, causa, solución y laboratorio donde apareció.

| Síntoma | Causa | Solución | Lab |
|---|---|---|---|
| PowerShell no deja activar `.venv` | Política de ejecución de scripts | `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` | 1 |
| `dvc` o `mlflow` no se reconocen | Entorno virtual sin activar | Activar `.venv` y repetir | 1 |
| `output ... is already tracked by SCM` | El CSV se subió a Git | `git rm --cached data/raw/dataset.csv`, commit y repetir `dvc add` | 2 |
| `[WinError 3]` al correr `dvc repro` | Ruta del repositorio demasiado larga en Windows | Mover el repositorio a una ruta corta, por ejemplo `C:\mlops\` | 3 |
| `The saved sklearn model references untrusted types` | Formato de guardado por defecto de MLflow reciente | Agregar `serialization_format="cloudpickle"` en `log_model` | 4 |
|  |  |  |  |
