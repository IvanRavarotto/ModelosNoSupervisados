# Análisis y Predicción de Enfermedades Cardiovasculares mediante Agrupación No Supervisada

Este proyecto de Google Colab explora la aplicación de técnicas de clustering no supervisado, K-Means y Agrupación Jerárquica, para analizar y predecir el riesgo de enfermedades cardiovasculares. El objetivo principal es identificar patrones en un conjunto de datos médicos y agrupar a los pacientes en categorías de 'Sano' o 'Enfermo' basándose únicamente en las características de los datos, sin etiquetas predefinidas durante el entrenamiento.

## 📊 Dataset

El análisis se basa en el dataset de Enfermedades Cardiovasculares (`Cardiovascular_Disease_Dataset.csv`), cargado directamente desde un repositorio de GitHub. Este dataset contiene diversas métricas de salud de pacientes, incluyendo:

*   `age`: Edad
*   `gender`: Género (0: Femenino, 1: Masculino)
*   `chestpain`: Tipo de dolor de pecho
*   `restingBP`: Presión arterial en reposo
*   `serumcholestrol`: Colesterol sérico
*   `fastingbloodsugar`: Azúcar en sangre en ayunas
*   `restingrelectro`: Resultados electrocardiográficos en reposo
*   `maxheartrate`: Frecuencia cardíaca máxima alcanzada
*   `exerciseangia`: Angina inducida por ejercicio
*   `oldpeak`: Depresión del ST inducida por el ejercicio
*   `slope`: La pendiente del segmento ST pico del ejercicio
*   `noofmajorvessels`: Número de vasos principales coloreados por fluoroscopia
*   `target`: Variable objetivo (0: Sano, 1: Enfermo) - **Utilizada solo para evaluación, no para el entrenamiento de los modelos no supervisados**.

## 🚀 Metodología

### 1. Preprocesamiento de Datos

Antes de aplicar los algoritmos de clustering, los datos fueron preprocesados:

*   **División:** El dataset se dividió en conjuntos de entrenamiento (80%) y prueba (20%).
*   **Escalado:** Las características numéricas se estandarizaron utilizando `StandardScaler` para asegurar que todas las variables contribuyan de manera equitativa a la formación de clústeres, evitando que características con rangos de valores más amplios dominen la distancia euclidiana.

### 2. Agrupación K-Means

K-Means es un algoritmo de clustering iterativo que busca particionar `n` observaciones en `k` clústeres. Se utilizó con `n_clusters=2` para alinearlo con las dos categorías (Sano/Enfermo) del problema.

*   **Entrenamiento:** El modelo se entrenó con el conjunto de entrenamiento escalado.
*   **Evaluación:** Se predijeron las etiquetas de los clústeres para el conjunto de prueba. Para evaluar la precisión de este modelo no supervisado, se realizó un **mapeo** de las etiquetas de clúster obtenidas a las etiquetas verdaderas del dataset (0 o 1) utilizando la clase más frecuente dentro de cada clúster. Esto permitió generar una **matriz de confusión** y un **reporte de clasificación**.
*   **Visualización:** Se aplicó **Análisis de Componentes Principales (PCA)** para reducir la dimensionalidad de los datos a 2 componentes, permitiendo la visualización de los clústeres y sus centroides en un gráfico de dispersión.
*   **Predicción para Paciente Nuevo:** Se implementó una función para clasificar un nuevo paciente, escalando sus datos y utilizando el modelo K-Means entrenado para asignarlo a uno de los dos clústeres.

### 3. Agrupación Jerárquica (Agglomerative Clustering)

La Agrupación Jerárquica construye una jerarquía de clústeres. En este proyecto, se utilizó el enfoque aglomerativo (`AgglomerativeClustering`), que comienza con cada punto como un clúster individual y los fusiona iterativamente.

*   **Entrenamiento:** El modelo se ajustó al conjunto de entrenamiento.
*   **Dendrograma:** Se generó un dendrograma para visualizar la estructura jerárquica de los clústeres y las distancias de fusión entre ellos.
*   **Evaluación:** Similar a K-Means, se mapearon las etiquetas de clúster a las etiquetas verdaderas para evaluar el rendimiento mediante una **matriz de confusión** y un **reporte de clasificación**.
*   **Predicción para Paciente Nuevo:** Dado que `AgglomerativeClustering` no tiene un método `predict` directo para nuevas muestras, se aproximó la clasificación de un nuevo paciente calculando la distancia a los centroides de los clústeres formados y asignándolo al más cercano.

## 📈 Resultados y Comparación de Modelos

Se realizó una comparación detallada del rendimiento de ambos modelos en términos de precisión, tiempo de entrenamiento y tiempo de predicción (tanto en lotes como para un paciente individual).

| Modelo                                | Tiempo Entrenamiento (s) | Tiempo Predicción (Batch) (s) | Tiempo Predicción (Paciente Nuevo) (s) | Precisión (%) |
| :------------------------------------ | :----------------------- | :---------------------------- | :------------------------------------- | :------------ |
| K-Means                               | 0.0223                   | 0.0097                        | 0.0033                                 | 94.57         |
| Agrupación Jerárquica                 | 0.1804                   | 0.0313                        | 0.0011                                 | 89.26         |

Los resultados se visualizan mediante gráficos de barras comparativos para facilitar la interpretación.

## ✅ Conclusión

El análisis concluye que el modelo **K-Means** demostró ser la opción más robusta y eficiente para la tarea de diagnóstico de enfermedades cardiovasculares con los datos actuales. Ofreció la **mayor precisión (94.57%)** y un **tiempo de predicción para pacientes individuales muy rápido**, lo que lo hace ideal para aplicaciones en tiempo real.

Aunque la Agrupación Jerárquica proporcionó una visión estructural interesante y un tiempo de predicción de paciente nuevo muy bajo, su precisión fue ligeramente inferior (89.26%), y su aplicación práctica para predicciones individuales es menos directa. Este estudio establece una base sólida para futuros trabajos, que podrían incluir la exploración de otros algoritmos, la incorporación de más datos o la transición a técnicas de aprendizaje supervisado para afinar aún más las predicciones.
