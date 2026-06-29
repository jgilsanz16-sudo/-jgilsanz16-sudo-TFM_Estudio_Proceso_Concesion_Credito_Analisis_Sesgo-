# Modelos de riesgo de crédito: rendimiento, sesgo e interpretabilidad

Este repositorio recoge el trabajo técnico de mi TFM del máster de Ciencia de Datos en CUNEF Universidad. El proyecto estudia la concesión de crédito hipotecario en Estados Unidos desde tres perspectivas complementarias: capacidad predictiva, equidad entre grupos e interpretabilidad de los modelos.

El objetivo no es únicamente obtener buenas métricas de clasificación. También se analiza si los errores y las decisiones del modelo presentan diferencias por sexo, raza o etnia, y hasta qué punto esas diferencias pueden mitigarse sin deteriorar de forma excesiva el rendimiento.

## Pregunta de investigación

¿Es posible construir un modelo de concesión de crédito con buen rendimiento predictivo y, al mismo tiempo, identificar, explicar y reducir posibles diferencias entre grupos demográficos?

La variable objetivo es binaria:

- `target = 1`: préstamo concedido.
- `target = 0`: préstamo denegado.

## Pipeline

Los notebooks están numerados en el orden en que deben leerse y ejecutarse.

| Etapa | Notebook | Contenido |
|---|---|---|
| 1 | `01_analisis_preparacion_datos.ipynb` | Integración de los datos anuales, control de calidad, transformaciones y análisis exploratorio. |
| 2 | `02_seleccion_variables_lasso.ipynb` | Selección de diez variables mediante regresión logística con penalización L1. |
| 3 | `03_regresion_logistica.ipynb` | Comparación de modelos logísticos con y sin atributos sensibles. |
| 4 | `04_fairness_equalized_odds.ipynb` | Mitigación de diferencias entre grupos mediante postprocesamiento con Equalized Odds. |
| 5 | `05_benchmark_machine_learning.ipynb` | Comparación de modelos de clasificación con validación cruzada estratificada. |
| 6 | `06_red_neuronal.ipynb` | Entrenamiento y evaluación de un perceptrón multicapa, con análisis operativo y económico. |
| 7 | `07_interpretabilidad_shap.ipynb` | Explicabilidad global y local de la red neuronal mediante valores SHAP. |

El flujo principal de datos es:

```text
CSV anuales
    -> df_model.pkl
    -> selección LASSO
    -> df_lasso2.csv
    -> modelos, fairness e interpretabilidad
```

## Metodología

El proyecto combina:

- análisis exploratorio y preparación de datos;
- selección de variables mediante regularización L1;
- regresión logística como modelo de referencia;
- modelos de árboles y métodos de *boosting*;
- red neuronal multicapa;
- métricas de equidad por sexo, raza y etnia;
- mitigación con `ThresholdOptimizer` y Equalized Odds;
- interpretación de predicciones mediante SHAP.

La comparación de modelos utiliza particiones estratificadas y separa la selección del modelo de su evaluación final. Entre las métricas analizadas se encuentran ROC AUC, *average precision*, *balanced accuracy*, precisión, *recall* y F1. La auditoría de equidad incorpora tasas de selección, TPR, FPR, paridad demográfica y *disparate impact*.

## Estructura del repositorio

```text
.
├── 01_analisis_preparacion_datos.ipynb
├── 02_seleccion_variables_lasso.ipynb
├── 03_regresion_logistica.ipynb
├── 04_fairness_equalized_odds.ipynb
├── 05_benchmark_machine_learning.ipynb
├── 06_red_neuronal.ipynb
├── 07_interpretabilidad_shap.ipynb
├── requirements.txt
└── README.md
```

## Datos

El análisis parte de registros hipotecarios de Estados Unidos correspondientes al periodo 2019-2024. Los archivos de datos no se incluyen en el repositorio para evitar publicar ficheros voluminosos y mantener separada la distribución del código.

La carpeta `data/README.md` detalla los nombres esperados y los ficheros intermedios generados por el pipeline.

## Ejecución

Se recomienda usar Python 3.10 o posterior.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Los archivos anuales deben situarse en `data/raw/`. Después se pueden ejecutar los notebooks en orden numérico desde la raíz del repositorio.

Las etapas de LASSO, benchmark, red neuronal y SHAP pueden requerir bastante memoria y tiempo de cálculo debido al tamaño del conjunto de datos.

## Limitaciones

- El análisis es académico y no constituye un sistema de decisión crediticia listo para producción.
- Las métricas de equidad ayudan a detectar diferencias entre grupos, pero no demuestran por sí solas la existencia o ausencia de discriminación.
- La mitigación de sesgo implica un equilibrio entre equidad, rendimiento y coste de los errores.
- Los resultados dependen de la calidad, cobertura y codificación de los datos utilizados.

## Autor

Javier Gilsanz Muñoz  
Trabajo Fin de Máster, CUNEF Universidad
