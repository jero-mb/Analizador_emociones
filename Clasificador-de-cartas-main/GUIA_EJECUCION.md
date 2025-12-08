# Guía de Ejecución - Clasificador de Cartas

## Estado Actual
✅ **Proyecto completamente funcional** — Todos los scripts ejecutándose correctamente y tests pasando (3/3).

## Instalación (una sola vez)

### Opción 1: Usando el venv existente (ya instalado)
El venv ya está creado en `.venv/` con todas las dependencias necesarias.

Simplemente actívalo:
```powershell
.\.venv\Scripts\Activate.ps1
```

### Opción 2: Crear un nuevo venv
Si necesitas crear uno nuevo desde cero:

```powershell
# Crear venv
python -m venv .venv

# Activar
.\.venv\Scripts\Activate.ps1

# Actualizar herramientas
python -m pip install --upgrade pip setuptools wheel

# Instalar dependencias (sin wordcloud que requiere compilador)
pip install numpy pandas scikit-learn nltk matplotlib seaborn imbalanced-learn joblib pytest flask
```

## Cómo ejecutar el proyecto

### 1. Activar el entorno virtual
```powershell
Set-Location "C:\Users\jerom\Downloads\Clasificador-de-cartas-main\Clasificador-de-cartas-main"
.\.venv\Scripts\Activate.ps1
```
Verás `(.venv)` al inicio del prompt si está activado.

### 2. Ejecutar los scripts en orden

#### a) Organizar proyecto (crear estructura de carpetas)
```powershell
python organizar_proyecto.py
```
- Crea carpetas: `data/`, `models/`, `notebooks/`, `tests/`, `src/`
- Crea `__init__.py` en módulos de `src/`

#### b) Generar datos de ejemplo
```powershell
python src\data\prepare_data.py
```
- Genera 300 ejemplos de cartas con 3 emociones: enamoramiento, ruptura, confusión
- Guarda en: `data/processed/cartas_dataset.csv`
- **Salida esperada:**
  - 100 ejemplos de cada emoción
  - Distribuición mostrada en consola

#### c) Entrenar modelo de clasificación
```powershell
python src\models\train.py
```
- Entrena RandomForestClassifier + SMOTE + TF-IDF
- Evalúa con validación cruzada (5-fold)
- Guarda modelo en: `models/modelo_emociones.pkl`
- Guarda encoder en: `models/label_encoder.pkl`
- **Salida esperada:**
  - Exactitud del modelo (~99-100%)
  - Reporte de clasificación (precision, recall, f1-score)
  - Análisis de 5 ejemplos de prueba

#### d) Ejecutar tests
```powershell
python -m pytest -q
```
O con más detalle:
```powershell
python -m pytest -v
```
- **Salida esperada:** 3 tests pasados (3/3 ✓)
  - `test_model_trains_and_predicts` — Verifica que el modelo entrena y predice correctamente
  - `test_generar_datos_ejemplo_produces_expected_shape` — Verifica generación de datos
  - `test_generar_datos_ejemplo_is_deterministic_with_seed` — Verifica reproducibilidad

### 3. Salir del entorno (opcional)
```powershell
deactivate
```

## Estructura de archivos clave

```
Clasificador-de-cartas-main/
├── organizar_proyecto.py          # Script de organización
├── requirements.txt                # Dependencias
├── GUIA_EJECUCION.md              # Esta guía
├── data/
│   └── processed/
│       └── cartas_dataset.csv      # Dataset generado (300 ejemplos)
├── models/
│   ├── modelo_emociones.pkl       # Modelo entrenado
│   └── label_encoder.pkl          # Codificador de etiquetas
├── src/
│   ├── data/
│   │   └── prepare_data.py        # Generador de datos
│   └── models/
│       └── train.py               # Trainer del modelo
└── tests/
    ├── test_prepare_data.py       # Tests de generación de datos
    └── test_model_train.py        # Tests del modelo
```

## Dependencias instaladas

- **numpy** — Cálculos numéricos
- **pandas** — Manipulación de datos (CSV)
- **scikit-learn** — Machine Learning (RandomForest, TF-IDF, cross_val_score)
- **imbalanced-learn** — SMOTE para balanceo de clases
- **nltk** — Procesamiento de lenguaje natural (stopwords)
- **matplotlib**, **seaborn** — Visualización (opcional)
- **joblib** — Serialización de modelos (guardar/cargar .pkl)
- **pytest** — Ejecución de tests
- **flask** — Web framework (para futuro uso)

**Nota:** `wordcloud==1.9.2` no está instalado porque requiere compilador C++ en Windows. Es opcional para este proyecto.

## Solución de problemas

### Error: "No module named 'pandas'"
- **Causa:** Entorno virtual no activado o dependencias no instaladas
- **Solución:**
  ```powershell
  .\.venv\Scripts\Activate.ps1
  pip install -r requirements.txt  # Omite wordcloud si falla
  ```

### Error: "Failed building wheel for wordcloud"
- **Causa:** Requiere Microsoft Visual C++ Build Tools
- **Solución:** Omite `wordcloud` (es solo para visualización, no es crítico)
  ```powershell
  pip install numpy pandas scikit-learn nltk matplotlib seaborn imbalanced-learn joblib pytest flask
  ```

### Tests no se encuentran
- **Causa:** Pytest no instalado o ruta incorrecta
- **Solución:**
  ```powershell
  pip install pytest
  python -m pytest tests/ -v
  ```

## Flujo completo (ejecución rápida)

```powershell
# 1. Sitúate en la carpeta
Set-Location "C:\Users\jerom\Downloads\Clasificador-de-cartas-main\Clasificador-de-cartas-main"

# 2. Activa venv
.\.venv\Scripts\Activate.ps1

# 3. Ejecuta todos los scripts
python organizar_proyecto.py
python src\data\prepare_data.py
python src\models\train.py
python -m pytest -q
```

**Tiempo esperado:** ~10-15 segundos total.

---

**Última actualización:** Diciembre 7, 2025  
**Estado:** ✅ Proyecto funcional - Todos los tests pasando
