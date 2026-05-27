# Customer Churn Prediction — Telecommunication

# Resultados Destacados

- Recall máximo obtenido: **0.816** (XGBoost)
- Mejor equilibrio general: **Random Forest Optimizado**
- ROC AUC máximo: **0.846**
- Dataset: **7000+ clientes**
- Modelos evaluados:
  - Logistic Regression
  - Random Forest
  - XGBoost

## Objetivo

En este proyecto se buscó predecir la pérdida de clientes (*customer churn*) en una empresa de telecomunicaciones utilizando distintos modelos de Machine Learning.

El objetivo principal fue crear un modelo capaz de predecir clientes con riesgo de abandono, priorizando el **recall** para maximizar la detección de los mismos, ya que para una empresa suele ser más costoso perder un cliente que contactar clientes adicionales mediante campañas de retención.

---

# Dataset

Se utilizó el dataset público **Telco Customer Churn**, que contiene información de más de 7000 clientes.

Entre las variables disponibles se encuentran: tipo de contrato, servicios contratados, método de pago, permanencia del cliente, cargos mensuales, cargos totales, soporte técnico, y la variable objetivo `Churn`.

---

# Análisis Exploratorio de Datos (EDA)

Durante el análisis exploratorio se realizaron:

- Limpieza y transformación de variables
- Detección de valores faltantes
- Conversión de variables categóricas
- Análisis de distribuciones
- Análisis de correlación
- Mapas de calor para variables categóricas
- Análisis de importancia de variables mediante Random Forest

## Principales hallazgos

Entre los principales hallazgos del análisis exploratorio se observaron los siguientes patrones:
### Clientes con menor permanencia presentan más churn
Principalmente, se observó una fuerte relación entre `tenure` y churn: los clientes que llevaban menos tiempo en la empresa eran considerablemente más propensos a abandonar el servicio.

### Contratos mensuales aumentan el riesgo
Los clientes con contratos *month-to-month* mostraron porcentajes de churn muy superiores al promedio general. Especialmente aquellos con cargos mensuales más altos.

### Otras variables también destacaban su relación con churn
Aunque no exploramos con detalle estas relaciones, las observamos en un mapa de calor. Se destacaron categorías como método de pago electrónico, no uso del servicio técnico, gente mayor de 65 y el uso de muchos servicios extra.

---

# Preparación de Datos

Para el modelado se realizaron los siguientes pasos:

- Eliminación de `customerID`
- Transformación de `TotalCharges` a variable numérica
- Eliminación de valores nulos
- Aplicación de One Hot Encoding
- Escalado de variables numéricas para Logistic Regression
- Separación estratificada en train/test

---

# Modelos Utilizados

## Logistic Regression

Se utilizó como modelo base.

Características:
- Usamos `class_weight='balanced'`, debido al desbalanceo en el dataset entre churn y no churn
- Analisis y ajuste de threshold
- Análisis Precision-Recall
- Validación cruzada estratificada

Resultado destacado:
- Modelo muy estable y consistente entre distintas evaluaciones
- Buen recall desde versiones iniciales, sin tuning necesario

---

## Random Forest

Se utilizó para:
- Analizar importancia de variables
- Capturar relaciones no lineales
- Comparar rendimiento con Logistic Regression

Se realizó:
- tuning manual
- análisis y ajuste de threshold
- GridSearchCV
- validación cruzada

Resultado destacado:
- Mejor equilibrio general entre métricas
- Mejora considerable luego del tuning

---

## XGBoost

Finalmente se probó XGBoost buscando maximizar recall.

Se ajustaron:
- `learning_rate`
- `max_depth`
- `scale_pos_weight`
- `subsample`
- `colsample_bytree`

Resultado destacado:
- Mayor recall de todos los modelos
- Aunque acompañado de más falsos positivos

---
# Decisiones y Optimización del Modelo

## Ajuste de Threshold

Uno de los puntos más importantes del proyecto fue el análisis del valor umbral (*threshold*), especialmente en logistic regression y random forest.
En lugar de utilizar únicamente el threshold estándar de `0.5`, se analizaron distintos valores para estudiar el equilibrio entre: recall y precision, y falsos positivos y falsos negativos.
Se observó que valores entre `0.3` y `0.4` ofrecían un equilibrio razonable entre:
- Detectar clientes con churn,
- Limitar contactos innecesarios.
Además, thresholds más bajos permitían aumentar significativamente el recall, aunque incrementando falsos positivos.

## Validación Cruzada

Se utilizaron 5 folds estratificados para asegurar:

- Estabilidad de resultados
- Evaluación más robusta
- Mantenimiento de proporciones de churn en cada fold

## Optimización con GridSearchCV

Se utilizó `GridSearchCV` para optimizar hiperparámetros en Random Forest.
Se probaron combinaciones de: profundidad máxima, cantidad de árboles, tamaño mínimo de hojas, y divisiones mínimas.
Esta optimización permitió mejorar considerablemente el recall de Random Forest, alcanzando valores comparables e incluso superiores a Logistic Regression en algunos casos.

---

# Métricas Prioritarias

Dado el problema de negocio, se priorizaron las siguientes métricas, en orden:

1. Recall
2. Precision
3. F1 Score
4. ROC AUC

Según el costo final de aplicar el modelo, se puede priorizar recall o precisión en este caso. Nosotros usamos recall como métrica principal, ya que representa la capacidad de detectar clientes que efectivamente realizarán churn.

---

# Comparación Final de Modelos

| Modelo | Recall | Precision | F1 Score | ROC AUC | Total Errores |
|---|---|---|---|---|---|
| Logistic Regression | 0.802 | 0.500 | 0.616 | 0.830 | 374 |
| Random Forest | 0.757 | 0.541 | 0.626 | 0.846 | 303 |
| Random Forest Optimizado | 0.810 | 0.501 | 0.619 | 0.842 | 373 |
| XGBoost Ajustado | 0.816 | 0.477 | 0.602 | 0.825 | 403 |

---

# Conclusiones

Los distintos modelos mostraron resultados relativamente similares, aunque cada uno presentó ventajas diferentes según la métrica observada.

## Logistic Regression
- Modelo más simple
- Muy estable
- Excelente recall con poco tuning

## Random Forest
- Mejor equilibrio general en la versión optimizada
- Menor cantidad de errores en versiones iniciales (mayor precisión)

## XGBoost
- Mejor recall, pero mayor cantidad de falsos positivos

En consecuencia, la elección final dependerá de la estrategia de negocio y del costo asociado tanto a la pérdida de clientes como a las campañas de retención.

---

# Tecnologías Utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- XGBoost

---

# Autor

**Francisco Lombroni**
