# **Introducción al Proyecto**

## **Contexto y Motivación Clínica**

Las enfermedades cardiovasculares constituyen una de las principales causas de mortalidad a nivel global. La capacidad de predecir la presencia de enfermedades cardíacas a partir de atributos clínicos y demográficos representa una herramienta invaluable para el sector salud, permitiendo diagnósticos tempranos e intervenciones preventivas. Sin embargo, en el ámbito médico, la **interpretabilidad** del modelo es tan crucial como su precisión: los clínicos necesitan comprender por qué un paciente es clasificado como de riesgo, no solo saber que lo es.

## **Dataset: Heart Disease UCI**

El presente estudio utiliza el dataset **Heart Disease** de la UCI Machine Learning Repository, que contiene información de 303 pacientes. Aunque el dataset original incluye 76 atributos, se emplea el subconjunto estándar de 14 características clave ampliamente validadas en la literatura de minería de datos médica.

- **Identificador en UCI ML Repository:** `id=45`
- **Enlace oficial:** [https://archive.ics.uci.edu/dataset/45/heart+disease](https://archive.ics.uci.edu/dataset/45/heart+disease)

## **Objetivo y Enfoque Metodológico**

El objetivo central es **desarrollar y comparar modelos de clasificación supervisada** para determinar la presencia o ausencia de cardiopatía, con énfasis especial en los **clasificadores bayesianos (Naive Bayes)** debido a su idoneidad para entornos clínicos:

- **Rapidez y bajo costo computacional:** Ideales para integrarse en sistemas de apoyo al diagnóstico en tiempo real.
- **Robustez ante datos ruidosos o incompletos,** algo frecuente en registros médicos.

No obstante, se reconoce la principal limitación del modelo bayesiano: la **asunción de independencia condicional** entre las variables predictoras (dado el estado de la enfermedad). Para evaluar el impacto práctico de esta asunción, se comparará su desempeño con un modelo **no bayesiano pero altamente interpretable**: la **regresión logística**, ampliamente utilizada en investigación médica.

## **Modelos Implementados**

| Modelo | Característica clave | Cuándo es apropiado |
|--------|----------------------|----------------------|
| `GaussianNB` | Asume distribución normal en las variables numéricas | Por defecto para datos mixtos (numéricos + categóricos) |
| `BernoulliNB` | Variables binarias (0/1) | Tras binarizar características |
| `MultinomialNB` | Variables discretas (conteos o frecuencias) | Tras discretización de variables continuas |
| `LogisticRegression` | No asume independencia de predictores | Línea base comparativa no bayesiana |

## **Flujo de Trabajo**

El análisis se estructura siguiendo un pipeline reproducible de ciencia de datos:

1. **Exploración y limpieza:** Detección de valores faltantes, análisis univariado, detección de outliers.
2. **Preprocesamiento:** Codificación de variables categóricas (tipo de dolor torácico, talasemia, electrocardiograma).
3. **Modelado mediante `Pipeline` de scikit-learn:** Cada modelo se encapsula con su propio preprocesamiento para evitar fugas de datos.
4. **Evaluación rigurosa:** Validación cruzada, matriz de confusión, precisión, sensibilidad (recall), especificidad, F1-score, curva ROC y AUC.
5. **Análisis de relevancia clínica:** Identificación de qué variables influyen más en la estimación de probabilidad posterior.

## **Variables de Estudio**

A continuación se describen las 14 variables utilizadas en el modelo, agrupadas por naturaleza clínica:

### **Variables Demográficas**
- `age`: Edad del paciente (años).
- `sex`: Sexo (0 = femenino, 1 = masculino).

### **Variables Clínicas y Sintomatología**
- `cp`: Tipo de dolor torácico (1 = angina típica, 2 = angina atípica, 3 = dolor no anginoso, 4 = asintomático). *Variable particularmente relevante en diagnóstico diferencial.*
- `trestbps`: Presión arterial en reposo (mm Hg).
- `chol`: Colesterol sérico (mg/dl).
- `fbs`: Glucemia en ayunas > 120 mg/dl (0 = no, 1 = sí).
- `restecg`: Resultados electrocardiográficos en reposo (0 = normal, 1 = anormalidad ST-T, 2 = hipertrofia ventricular izquierda).
- `thalach`: Frecuencia cardíaca máxima alcanzada durante prueba de esfuerzo.
- `exang`: Angina inducida por ejercicio (0 = no, 1 = sí).
- `oldpeak`: Depresión del segmento ST inducida por ejercicio (valor continuo). *Potente predictor de isquemia.*

### **Variables de Pruebas Diagnósticas Especializadas**
- `slope`: Pendiente del segmento ST en el esfuerzo máximo (1 = ascendente, 2 = plana, 3 = descendente).
- `ca`: Número de vasos principales coloreados por fluoroscopia (0–3).
- `thal`: Talasemia (3 = normal, 6 = defecto fijo, 7 = defecto reversible). *Marcador de anomalías sanguíneas.*

### **Variable Objetivo**
- `num`: Diagnóstico de enfermedad cardíaca (0 = ausencia, 1 = presencia). Se trabaja con la versión binaria estándar (presencia si > 0).

## **Relevancia del Análisis**

Este conjunto de variables abarca desde características demográficas básicas hasta resultados de pruebas especializadas como fluoroscopia y talasemia. La combinación de modelos bayesianos con un pipeline estructurado permitirá:

1. Evaluar la **precisión diagnóstica** de cada modelo en términos clínicos (sensibilidad/especificidad).
2. Determinar si la **ganancia en interpretabilidad** de Naive Bayes justifica su posible pérdida de precisión frente a regresión logística.
3. Generar un **prototipo reproducible** que pueda adaptarse a otros datasets médicos.

El código completo, visualizaciones y análisis crítico se presentan en las secciones siguientes, respetando los principios de ciencia reproducible y documentación transparente.