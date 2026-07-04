# Predicción del riesgo de burnout estudiantil asociado al uso de IA generativa

## Integrante

- Arnaldo González — Usuario GitHub: `@arnalytox`


## Descripción del problema

El uso de herramientas de inteligencia artificial generativa se ha vuelto cada vez más común en actividades académicas. Estas herramientas pueden apoyar el estudio, la redacción, la programación y la resolución de dudas, pero también pueden asociarse a nuevos patrones de dependencia, cambios en hábitos de estudio y posibles efectos sobre el bienestar estudiantil.

Este proyecto aborda el problema desde un enfoque de aprendizaje supervisado: se busca predecir el nivel de riesgo de burnout estudiantil usando variables relacionadas con uso de IA, desempeño académico, hábitos de estudio y bienestar.

## Dataset utilizado

- **Nombre:** Impact of Ai on Students.
- **Fuente:** Kaggle.
- **Archivo usado en este repositorio:** `data/ai_student_impact.csv`.
- **Dimensiones del CSV:** 50.000 filas y 16 columnas.
- **Variable objetivo:** `Burnout_Risk_Level`.

El dataset contiene variables como:

- `Student_ID`.
- `Major_Category`.
- `Year_of_Study`.
- `Pre_Semester_GPA`.
- `Weekly_GenAI_Hours`.
- `Primary_Use_Case`.
- `Prompt_Engineering_Skill`.
- `Tool_Diversity`.
- `Paid_Subscription`.
- `Traditional_Study_Hours`.
- `Perceived_AI_Dependency`.
- `Institutional_Policy`.
- `Anxiety_Level_During_Exams`.
- `Post_Semester_GPA`.
- `Skill_Retention_Score`.
- `Burnout_Risk_Level`.

## Pregunta de investigación

¿Es posible predecir el riesgo de burnout de un estudiante a partir de sus patrones de uso de IA generativa, desempeño académico, hábitos de estudio y variables asociadas al bienestar?

## Objetivo del proyecto

### Objetivo general

Construir y evaluar modelos de clasificación supervisada para predecir el nivel de riesgo de burnout estudiantil utilizando variables asociadas al uso de IA generativa y al contexto académico.

### Objetivos específicos

1. Explorar el dataset mediante análisis exploratorio de datos.
2. Identificar la distribución de la variable objetivo `Burnout_Risk_Level`.
3. Construir variables derivadas interpretables mediante feature engineering.
4. Aplicar feature selection mediante información mutua.
5. Entrenar y comparar modelos revisados en clase.
6. Evaluar el desempeño mediante métricas de clasificación.
7. Controlar posible overfitting comparando desempeño en entrenamiento y test.
8. Interpretar los resultados y plantear limitaciones del análisis.

## Modelos utilizados

Se trabajó con modelos revisados en la asignatura:

- Regresión logística.
- K-Nearest Neighbors.
- Decision Tree.
- Random Forest.
- Naive Bayes.
- PCA como herramienta exploratoria de reducción de dimensionalidad.

## Justificación de los modelos

- **Regresión logística:** se utiliza como línea base interpretable para clasificación.
- **KNN:** permite clasificar estudiantes según similitud entre observaciones.
- **Decision Tree:** permite generar reglas de decisión simples e interpretables.
- **Random Forest:** permite capturar relaciones no lineales y reducir parte de la inestabilidad de un árbol individual.
- **Naive Bayes:** se incluye como comparación rápida y simple.
- **PCA:** se usa solo como apoyo visual para analizar la separación de clases en dos componentes, no como modelo predictivo final.

## Metodología aplicada

1. **Carga de datos:** el CSV se carga desde `data/ai_student_impact.csv` usando rutas robustas para que el notebook funcione desde la raíz del proyecto o desde `notebooks/`.
2. **EDA:** se revisan dimensiones, columnas, tipos de datos, valores faltantes, duplicados, distribución de la variable objetivo y variables numéricas.
3. **Feature engineering:** se crean variables derivadas:
   - `ratio_ia_estudio`: razón entre horas de uso de IA y horas de estudio tradicional.
   - `uso_intensivo_ia`: indicador de uso de IA sobre la mediana.
   - `dependencia_ia_alta`: indicador de dependencia percibida alta.
   - `delta_gpa`: diferencia entre `Post_Semester_GPA` y `Pre_Semester_GPA`.
4. **Muestra de modelado:** el EDA usa los 50.000 registros, pero el entrenamiento usa una muestra estratificada de 15.000 casos para que el notebook sea reproducible en computadores personales.
5. **Preprocesamiento:** se imputan valores faltantes, se escalan variables numéricas y se codifican variables categóricas con One-Hot Encoding.
6. **Feature selection:** se usa `SelectKBest` con `mutual_info_classif` para seleccionar las 30 variables transformadas más informativas.
7. **Entrenamiento y testeo:** se usa separación train/test estratificada con `random_state=42`.
8. **Evaluación:** se comparan accuracy, precision macro, recall macro y F1-score macro.
9. **Control de overfitting:** se compara F1-score macro en train y test.
10. **Visualización:** los gráficos se muestran directamente en el notebook; no se guardan imágenes externas.

## Resultados obtenidos

La tabla siguiente resume el desempeño de los modelos en el conjunto de test:

| Modelo | Accuracy test | Precision macro test | Recall macro test | F1-score macro test |
| --- | ---: | ---: | ---: | ---: |
| Random Forest | 0.5233 | 0.5277 | 0.5471 | 0.5279 |
| Regresión logística | 0.5127 | 0.5144 | 0.5419 | 0.5135 |
| Decision Tree | 0.5030 | 0.5103 | 0.5212 | 0.5100 |
| Naive Bayes | 0.5083 | 0.5246 | 0.5239 | 0.5072 |
| KNN | 0.4563 | 0.4645 | 0.4441 | 0.4498 |

El mejor modelo según F1-score macro fue **Random Forest**, con F1-score macro de **0.5279** y accuracy de **0.5233**.

## Control de overfitting

| Modelo | F1 macro train | F1 macro test | Brecha F1 train-test |
| --- | ---: | ---: | ---: |
| Random Forest | 0.6247 | 0.5279 | 0.0968 |
| Decision Tree | 0.5570 | 0.5100 | 0.0470 |
| KNN | 0.4720 | 0.4498 | 0.0223 |
| Regresión logística | 0.5090 | 0.5135 | -0.0044 |
| Naive Bayes | 0.4988 | 0.5072 | -0.0084 |

Random Forest obtuvo el mejor rendimiento, pero también presentó una brecha train-test mayor que los otros modelos. La brecha no es extrema, pero sí indica que el modelo aprende patrones del entrenamiento con mayor intensidad que modelos más simples. Por eso, la interpretación debe considerar tanto el mejor F1-score como la posibilidad de sobreajuste moderado.

## Variables más relevantes

Según la importancia de variables de Random Forest, las variables más relevantes fueron:

1. `Weekly_GenAI_Hours`.
2. `ratio_ia_estudio`.
3. `Perceived_AI_Dependency`.
4. `Pre_Semester_GPA`.
5. `uso_intensivo_ia`.

Esto sugiere que los patrones de uso de IA, la relación entre IA y estudio tradicional, la dependencia percibida y antecedentes académicos aportan información útil para clasificar el riesgo de burnout.

## PCA

La varianza explicada acumulada por las dos primeras componentes fue **0.3489**. Esto indica que dos componentes resumen solo una parte de la información total, por lo que PCA se interpreta como apoyo visual y no como reemplazo del modelo predictivo.

## Conclusiones

El proyecto permitió construir un flujo completo de aprendizaje supervisado para predecir el riesgo de burnout estudiantil usando un dataset real sobre uso de IA generativa. El mejor modelo fue **Random Forest**, con un F1-score macro de **0.5279** en test.

Los resultados muestran que el problema es desafiante: las métricas son moderadas y ninguna técnica separa perfectamente las clases. Aun así, el modelo logra capturar señales útiles en variables asociadas al uso de IA, hábitos de estudio, dependencia percibida y desempeño académico.

El análisis debe interpretarse como una tarea predictiva, no causal. No se puede afirmar que el uso de IA cause burnout, sino que ciertas variables relacionadas con el uso de IA y el contexto académico ayudan a predecir el nivel de riesgo en este dataset.

Como trabajo futuro, se podría probar validación externa, análisis por carrera o año académico, ajuste más profundo de hiperparámetros y técnicas de interpretabilidad para explicar mejor las predicciones.

## Estructura del repositorio

```text
ai-burnout-estudiantil/
├── data/
│   └── ai_student_impact.csv
├── notebooks/
│   └── proyecto_ia_burnout_estudiantil.ipynb
├── outputs/
│   ├── figures/
│   │   └── .gitkeep
│   └── metrics/
│       ├── classification_report_mejor_modelo.txt
│       ├── descripcion_variables_numericas.csv
│       ├── distribucion_variable_objetivo.csv
│       ├── feature_selection_mutual_info_top20.csv
│       ├── importancia_variables_random_forest.csv
│       ├── matriz_confusion_mejor_modelo.csv
│       ├── metricas_modelos.csv
│       ├── overfitting.csv
│       ├── parametros_modelos.csv
│       ├── resumen_dataset_y_proceso.csv
│       ├── roc_auc_mejor_modelo.txt
│       ├── valores_faltantes.csv
│       └── varianza_pca.txt
├── README.md
├── environment.yml
├── requirements.txt
├── .gitignore
└── LICENSE
```

## Instrucciones de ejecución

### Opción recomendada con Conda

```bash
conda env create -f environment.yml
conda activate ai-burnout-estudiantil
jupyter notebook notebooks/proyecto_ia_burnout_estudiantil.ipynb
```

Luego ejecutar en Jupyter:

```text
Kernel -> Restart & Run All
```

### Opción alternativa con pip

```bash
python -m venv .venv
source .venv/bin/activate  # macOS/Linux
# En Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/proyecto_ia_burnout_estudiantil.ipynb
```

## Dependencias principales

Las dependencias están en `environment.yml`. Las principales son:

- Python 3.11.
- pandas.
- numpy.
- matplotlib.
- scikit-learn.
- jupyter.
- notebook.
- ipykernel.
- kaggle.

## Uso de inteligencia artificial

Se utilizó inteligencia artificial como apoyo para estructurar el proyecto, revisar código, mejorar documentación y detectar errores de reproducibilidad. Las decisiones sobre el problema, dataset, modelos, interpretación de resultados y conclusiones fueron revisadas y justificadas por el estudiante.

## Entrega

- Repositorio de GitHub: `pegar enlace del repositorio`.
- Integrante: Arnaldo González.
- Usuario GitHub: `@arnalytox`.

> Antes de entregar, reemplazar `pegar enlace del repositorio` por el link real de GitHub.
