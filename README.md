# Detección de plagio en código fuente

## Introducción
Este proyecto desarrolla un detector de plagio para código Python basado en técnicas de recuperación de información. La idea central es comparar dos fragmentos de código y determinar si su similitud sugiere que uno es una copia no autorizada del otro.

## Objetivo
El objetivo es construir un prototipo que reciba dos códigos Python y devuelva una predicción binaria: plagio o no plagio. Para lograrlo, se utiliza normalización de código, vectorización TF-IDF y similitud de coseno.

## Definición del proyecto
Es un proyecto para detección de plagio en código fuente. Básicamente, el sistema recibe dos fragmentos de código, los procesa y predice si existe plagio entre ellos.

## Algoritmos estadísticos

## TF-IDF
TF-IDF es una técnica de ponderación de términos que combina la frecuencia de un token en un documento con la rareza de ese token en todo el conjunto de documentos. En este caso se aplica sobre tokens de código normalizado para capturar diferencias de estilo y estructura.

### Funcionamiento
- Normaliza código Python para reducir ruido y enfocar la comparación en estructura y tokens relevantes.
- Convierte los fragmentos normalizados en vectores TF-IDF usando análisis de n-gramas de caracteres (`char_wb`).
- Calcula la similitud de coseno entre pares de códigos para estimar la probabilidad de plagio.
- Ajusta un umbral de decisión en un conjunto de validación para clasificar pares como plagio o no plagio.
- Evalúa el modelo en un conjunto de prueba con métricas como precisión, recall, F1 y ROC AUC.

### Procedimiento
1. Carga y preprocesamiento del dataset.
2. Construcción de pares de código:
   - pares positivos: código original contra sus variantes plagio.
   - pares negativos: código original contra código de asignaciones distintas.
3. Normalización de código:
   - eliminación de comentarios, saltos de línea y sangrías.
   - reemplazo de identificadores no reservados por `ID`.
   - reemplazo de literales de cadena por `STR` y números por `NUM`.
4. Vectorización TF-IDF:
   - `analyzer='char_wb'`
   - `ngram_range=(3, 8)`
   - `sublinear_tf=True`
   - `max_features=50000`
5. Cálculo de similitud y selección de umbral.

## Resultados obtenidos

### Matriz de confusión

![Matriz de confusión](tf_idf/confussion_matrix.png)


Donde:
- Verdaderos Positivos (TP) = 1477
- Falsos Negativos (FN) = 213
- Falsos Positivos (FP) = 218
- Verdaderos Negativos (TN) = 1472

El conjunto de prueba arrojó las siguientes métricas:

| Métrica     | Valor                         |
|------------|-------------------------------|
| accuracy   | 0.8724852071005917            |
| precision  | 0.8735905044510386            |
| recall     | 0.8710059171597633            |
| f1         | 0.8722962962962963            |
| roc_auc    | 0.9412191449879207            |


**Análisis de resultados**: La matriz muestra **1477** Verdaderos Positivos y **1472** Verdaderos Negativos, lo que confirma que el modelo acierta con una alta proporción de ejemplos tanto positivos como negativos.

- `accuracy` de **0.8725** indica que el modelo clasifica correctamente aproximadamente el 87.25% de todos los pares de código (tanto positivos como negativos).
- `precision` de **0.8736** indica que, de los pares predichos como plagio, alrededor del 87.36% efectivamente son plagio; es una medida de cuántas alarmas son correctas (baja tasa de falsos positivos).
- `recall` de **0.8710** muestra que el modelo detecta cerca del 87.10% de los plagios reales; los **213** falsos negativos representan los plagios que no fueron identificados.
- `f1` de **0.8723** resume el equilibrio entre precisión y recall en un único valor, indicando un rendimiento consistente.
- `roc_auc` de **0.9412** confirma que la puntuación de similitud (coseno) separa bien las clases en términos de ranking.

El umbral de decisión elegido `0.705` representa el punto de corte sobre la similitud de coseno que ofrece un balance entre falsos positivos y falsos negativos en el conjunto de validación. 

Aumentar el umbral hace el detector más conservador: reduce falsos positivos, mejora `precision` pero aumenta falsos negativos, peor `recall`. Por otro lado, el disminuir el umbral hace el detector más permisivo: reduce falsos negativos, mejora `recall`, pero incrementa falsos positivos, peor `precision`.

# Autores
- Axel Camacho
- Cristián Chávez
- Benjamín Arauz

# Referencia

[1] Halim, Jimmy & Lasut, Desiyanna. (2024). Document Plagiarism Detection Application Using Web-Based TF-IDF and Cosine Similarity Methods: English. bit-Tech. 7. 202-213. 10.32877/bt.v7i2.1697.