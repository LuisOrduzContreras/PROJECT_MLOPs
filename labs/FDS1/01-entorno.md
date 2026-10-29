# Laboratorio 1 · Entorno de trabajo del equipo

Sábado del fin de semana 1 · 45 min · por equipos · hito E1

## Objetivo
Que todos los integrantes tengan el repositorio del equipo funcionando en su máquina, con DVC y MLflow instalados, las ramas protegidas y el primer Pull Request hecho.

## Pasos

### 1. Repositorio del equipo (una sola persona)
1. En la página de este repositorio plantilla: **Use this template › Create a new repository**. Nombre: `mlops-<equipo>`, en minúsculas y con guiones. Visibilidad: **Public** (en GitHub gratis, la protección de ramas solo funciona en repositorios públicos).
2. En el repositorio nuevo: *Settings › Collaborators* y agregar al resto del equipo y al docente.
3. Anotar la URL del repositorio en la hoja compartida del curso.

No usen *Fork*: en un fork, los Pull Requests apuntan por defecto a la plantilla del curso y no a su propio repositorio.

### 2. Rama de entrega y protección de ramas (la misma persona)
1. Crear la rama de entrega desde GitHub: en la pestaña *Code*, abrir el menú de ramas (dice `master`), escribir `entrega/e1` y pulsar **Create branch entrega/e1 from master**.
2. *Settings › Branches › Add classic branch protection rule*:
   - **Branch name pattern**: `master`
   - Marcar **Require a pull request before merging** y, dentro, **Require approvals** con 1 aprobación.
   - Marcar **Do not allow bypassing the above settings**.
   - **Create**.
3. Repetir el paso anterior con el patrón `entrega/*`.

Desde ahora GitHub rechaza cualquier push directo a `master` o a `entrega/e1`: todo entra por un Pull Request aprobado por otra persona. La convención de nombres de ramas está en [docs/entregas.md](../../docs/entregas.md).

### 3. Clonar y crear el entorno (todos)
En Windows, clonen en una carpeta de ruta corta, por ejemplo `C:\mlops\`. Las rutas muy largas hacen fallar a DVC.
```bash
git clone https://github.com/<usuario>/mlops-<equipo>.git
cd mlops-<equipo>
git switch entrega/e1
python -m venv .venv
```
Activar el entorno:

| Sistema | Comando |
|---|---|
| Windows (PowerShell) | `.venv\Scripts\Activate.ps1` |
| Windows (cmd) | `.venv\Scripts\activate.bat` |
| macOS / Linux | `source .venv/bin/activate` |

Instalar dependencias y verificar:
```bash
pip install -r requirements.txt
dvc --version
mlflow --version
```

### 4. Dataset del equipo (una sola persona)
Copiar el CSV del equipo a `data/raw/dataset.csv`. **No hacer commit del CSV**: lo versiona DVC en el laboratorio 2.

Editar `params.yaml`:
- `target`: nombre de la columna que se quiere predecir.
- `drop_columns`: identificadores o columnas que no aportan (por ejemplo `customerID`).
- `experiment` y `model_name`: cambiar `X` por el nombre del equipo.

### 5. Primera rama de trabajo: inicializar DVC (la misma persona)
```bash
git switch -c feat/inicializa-dvc
dvc init
git add .dvc .dvcignore params.yaml
git commit -m "Inicializa DVC y configura params"
git push -u origin feat/inicializa-dvc
```

### 6. Primer Pull Request (revisa otra persona)
1. En GitHub: **Pull requests › New pull request**, con `base: entrega/e1` ← `compare: feat/inicializa-dvc`.
2. La descripción trae la plantilla de entrega: para un PR de trabajo, bórrenla y escriban en una o dos líneas qué cambia. **Create pull request**.
3. **Otro integrante** abre el PR: *Files changed › Review changes › Approve › Submit review*. Luego **Merge pull request › Confirm merge** y **Delete branch**.
4. Todos:
   ```bash
   git switch entrega/e1
   git pull
   ```

## Listo cuando
- [ ] `dvc --version` y `mlflow --version` responden en todas las máquinas del equipo.
- [ ] `master` y `entrega/*` tienen la regla de protección.
- [ ] El PR de `feat/inicializa-dvc` está aprobado por otro integrante y unido a `entrega/e1`.
- [ ] `git branch` muestra `* entrega/e1` en todas las máquinas.
- [ ] El docente es colaborador del repositorio.

## Errores frecuentes
- **PowerShell no deja activar el entorno** («la ejecución de scripts está deshabilitada»): ejecutar una vez `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` y volver a activar.
- **`dvc` o `mlflow` no se reconocen**: el entorno virtual no está activado. Activarlo y repetir.
- **`python` abre la Microsoft Store en Windows**: usar `py` en lugar de `python`, o desactivar el alias en *Configuración › Aplicaciones › Alias de ejecución*.
- **No se puede instalar nada en la máquina**: abrir el repositorio en GitHub Codespaces (*Code › Codespaces › Create*), que ya trae Python.
- **No aparece la opción de proteger ramas o no se aplica**: el repositorio es privado. Cambiarlo a público en *Settings › General › Danger Zone › Change visibility*.
- **`GH006: Protected branch update failed`** o **`protected branch hook declined`**: intentaron hacer push directo a `master` o a `entrega/e1`. Es lo esperado: trabajen en una rama `feat/…` y abran un PR.
- **«Pull request authors can't approve their own pull request»**: lo debe aprobar otro integrante del equipo.
- **Hice commit en `entrega/e1` o en `master` por error** (y aún no hice push): guardar el commit en una rama nueva y dejar la rama como estaba:
  ```bash
  git branch feat/<nombre>
  git reset --hard origin/entrega/e1
  git switch feat/<nombre>
  ```
