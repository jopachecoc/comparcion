# Reestructuración del pipeline.

Pipeline propuesto (MLOps) 
# Presentado por: Jonathan Pacheco 



## Definición del problema: 
“Dados los avances tecnológicos, en el campo de la medicina la cantidad de información que existe de los pacientes es muy abundante. Sin embargo, para algunas enfermedades no tan comunes, llamadas huérfanas, los datos que existen escasean. Se requiere construir un modelo que sea capaz de predecir, dados los datos de síntomas de un paciente, si es posible o no que este sufra de alguna enfermedad. Esto se requiere tanto para enfermedades comunes (muchos datos) como para enfermedades huérfanas (pocos datos).



# Clasificación de Estado de Salud Basada en Síntomas Clínicos

Este proyecto se enfoca en la clasificación del estado de salud de los pacientes a partir de información clínica básica. Para ello, se utilizan las siguientes variables:

- Pulso cardíaco  
- Temperatura corporal  
- Presión arterial sistólica  
- Presión arterial diastólica  
- Edad  

Con base en estos datos, el modelo predice la categoría de enfermedad que presenta el paciente, clasificándose en una de las siguientes cinco categorías:

- **No enfermo**  
- **Enfermedad leve**  
- **Enfermedad aguda**  
- **Enfermedad crónica**  
- **Enfermedad terminal**

## Fuente de Datos

El sistema parte de un conjunto de datos históricos que contiene registros completos de las variables mencionadas. Este conjunto se actualiza de forma semanal con la información recolectada de nuevos pacientes y se almacena en un archivo de texto plano (`.txt`).

## Consideraciones para Despliegue en la Nube

En el diagrama asociado al proyecto se sugieren servicios de **Amazon Web Services (AWS)** como opción preferente para el despliegue de la solución en la nube. No obstante, es posible implementar una arquitectura equivalente utilizando servicios similares en otras plataformas cloud como **Google Cloud Platform (GCP)** o **Microsoft Azure**, en función de los requerimientos específicos del entorno o las preferencias del equipo técnico.


## PIPELINE MLOPS PROPUESTO




De acuerdo con la definición del problema, el modelo se entrena exclusivamente con información relacionada con los **síntomas actuales del paciente**, sin hacer uso de datos históricos adicionales (como antecedentes médicos o tratamientos previos). Por este motivo, se utiliza únicamente un archivo de datos que contiene síntomas y su respectiva categoría de enfermedad como referencia para el entrenamiento y evaluación del modelo.






# Fuentes de Datos

Para el desarrollo del modelo de predicción, se contemplan una fuente  de información, bajo los siguientes supuestos:

## 1. Información Histórica

Se dispone de un archivo de texto plano (`.txt`) que contiene registros históricos de pacientes. Este archivo incluye, información del paciente (id), síntomas reportados y la categoría de enfermedad correspondiente. La información se actualiza de manera semanal a través de un proceso batch automatizado.

## 2. Registro Actual de Pacientes

Los médicos utilizan una aplicación para registrar los datos clínicos de los pacientes. Este sistema incluye:

- **Formulario de triage**: Permite ingresar los síntomas actuales del paciente.
- **Pronóstico automático**: Tras el registro de los síntomas, la aplicación genera una predicción sobre la categoría de enfermedad.
- **Formulario de validación**: Permite a los profesionales de la salud registrar la clasificación real de la enfermedad, tiempo despues, la cual sirve como etiqueta verdadera para evaluar y retroalimentar el modelo. (como ya se cuenta con esta información para crear el modelo suponemos que este proceso ya lo realiza el cliente)


# 2. Preprocesamiento de Datos

El preprocesamiento de datos es una etapa fundamental para garantizar la calidad y fiabilidad del modelo de predicción. A continuación, se detallan los pasos principales aplicados durante este proceso:

## Limpieza de Datos

Se realiza una revisión detallada de las variables presentes en el conjunto de datos. En esta etapa:

- Se identifican registros duplicados y se eliminan, conservando únicamente una instancia de cada observación.
- Se validan los formatos de los campos para asegurar la consistencia estructural del dataset.

## Tratamiento de Valores Faltantes

Cuando se detectan registros con información incompleta, se evalúa la viabilidad de aplicar técnicas de imputación. Dependiendo del tipo de variable y el patrón de ausencia, se emplean métodos como:

- Imputación por la media o mediana (para variables numéricas).
- Imputación por la moda o categoría más frecuente (para variables categóricas).

## Corrección de Inconsistencias

Se lleva a cabo una exploración exhaustiva de los datos con el fin de detectar anomalías o valores atípicos. Algunos ejemplos comunes incluyen:

- Presencia de caracteres no válidos en campos numéricos.
- Valores imposibles, como un pulso cardíaco negativo o una temperatura corporal fuera de rango fisiológico.

En estos casos, se analiza el origen de la inconsistencia y se determina la estrategia más adecuada: corrección, imputación o eliminación del registro, siempre priorizando la calidad del conjunto de datos y la robustez del modelo.

Un aspecto crítico a considerar es el **desequilibrio en las clases** de la variable objetivo. Algunas categorías presentan un número significativamente menor de registros, lo cual puede conducir a un sesgo en la predicción, favoreciendo las clases mayoritarias y dificultando la detección de las clases minoritarias. Para mitigar este problema, es fundamental aplicar técnicas de balanceo de clases, tales como:

- **Sobremuestreo** de las clases minoritarias para incrementar su representación en el dataset.
- Uso de **SMOTE (Synthetic Minority Over-sampling Technique)** para generar ejemplos sintéticos de las clases con pocos datos, mejorando así la capacidad del modelo para generalizar.

## Ingeniería de Características

Se realizan transformaciones y mejoras sobre las variables para optimizar el rendimiento del modelo, tales como:

- Codificación de variables categóricas (one-hot encoding, label encoding).
- Normalización o estandarización de variables numéricas.
- Creación de nuevas características relevantes a partir de las existentes.

## Particionamiento del Conjunto de Datos

Finalmente, se divide el conjunto de datos en subconjuntos de entrenamiento y prueba para evaluar adecuadamente el desempeño del modelo y evitar sobreajuste.


## Análisis de Correlaciones e Importancia de Variables

A continuación, se lleva a cabo un análisis exhaustivo de las correlaciones entre variables y la importancia de cada una en el modelo. Esta etapa es crucial para identificar posibles sesgos, por ejemplo, cuando alguna variable ejerce una influencia desproporcionada sobre la predicción.

## Ingeniería de Características

Con base en el análisis previo, se desarrollan nuevas características que puedan mejorar la capacidad predictiva del modelo. Posteriormente, se aplican transformaciones sobre las variables para mejorar su distribución, como la aplicación de la función logarítmica para reducir la asimetría.

Finalmente, se procede a la codificación de variables categóricas mediante técnicas como:

- **One-Hot Encoding**  
- **Label Encoding**

Este proceso genera el conjunto final de características que alimentará al modelo para su entrenamiento.



# Tecnologías Recomendadas para el Preprocesamiento de Datos en la Nube

Para el manejo y preprocesamiento de datos en entornos cloud, recomiendo utilizar **Databricks**, una plataforma colaborativa que facilita el trabajo en equipo dentro de un mismo entorno integrado.

Entre sus principales ventajas destacan:

- Soporte para múltiples lenguajes de programación: **Python**, **SQL**, **R** y **Scala** mediante notebooks interactivos.
- Capacidad de procesamiento paralelo y distribuido basado en **Apache Spark**, lo que optimiza significativamente el tiempo de ejecución en tareas de gran volumen de datos.
- Entorno colaborativo que permite a los equipos trabajar simultáneamente sobre los mismos proyectos y datos.

En caso de que se prefiera ejecutar localmente, es posible sustituir el desarrollo en Databricks por programación directa en Python, conservando la flexibilidad para el procesamiento y análisis de datos.



# 3. Almacenamiento de Datos

Una vez finalizada la fase de preparación y preprocesamiento de datos, la información procesada se almacena en un **bucket de Amazon S3** (Simple Storage Service) de AWS.

La elección de Amazon S3 se fundamenta en sus características clave:

- **Escalabilidad automática**, que permite manejar incrementos en el volumen de datos sin necesidad de intervención manual.
- **Modelo de costos flexible**, donde se paga únicamente por el almacenamiento utilizado y las transferencias de datos realizadas.
- **Alta disponibilidad y durabilidad**, mediante la replicación automática de los datos en múltiples zonas de disponibilidad dentro de una misma región, garantizando la resiliencia y seguridad de la información.

Este enfoque asegura que los datos estén accesibles, seguros y listos para ser consumidos por etapas posteriores del pipeline o por otros servicios en la nube.







# 4. Entrenamiento de Modelos

Una vez que los datos han sido almacenados, se crea un notebook en Databricks para continuar con la fase de entrenamiento y evaluación de modelos.

El proceso inicia con la partición del conjunto de datos en subconjuntos de **entrenamiento** y **prueba**, lo cual permite evaluar el desempeño del modelo de manera objetiva.

Dado que se trata de un problema de clasificación, se implementan y comparan los siguientes algoritmos:

- **Árboles de decisión**  
- **Random Forest**  
- **XGBoost**  
- **Multilayer Perceptron (MLP)**  

A través de esta comparación, se selecciona el modelo con mejor desempeño para su posterior despliegue.



También se puede utilizar **validación cruzada** para evaluar los modelos y optimizar la selección de hiperparámetros. Este proceso se realiza empleando la librería **scikit-learn** en el entorno de Databricks.

Una vez finalizada la optimización, se procede a comparar las métricas de desempeño entre los modelos entrenados. Las métricas consideradas incluyen, entre otras:

- Precisión (Precision)  
- Sensibilidad (Recall)  
- Puntaje F1 (F1 Score)

El modelo con el mejor ajuste y desempeño será seleccionado para su despliegue.

Posteriormente, se registra el modelo utilizando **MLflow**, guardando resultados, métricas, hiperparámetros y artefactos asociados. El modelo puede ser almacenado en formatos como MLflow o **ONNX** para facilitar su interoperabilidad.

Finalmente, toda la solución se empaqueta en un contenedor **Docker**, garantizando la reproducibilidad y portabilidad en cualquier entorno de ejecución.



# Tecnologías Utilizadas en la Fase de Entrenamiento y Despliegue

Durante esta fase se emplean las siguientes tecnologías clave:

- **Databricks**: Plataforma líder para el modelado de datos que permite realizar cálculos y evaluación de métricas con alta eficiencia gracias a su capacidad de procesamiento paralelo distribuido.

- **MLflow**: Herramienta para el registro, rastreo, versionado y comparación de modelos, facilitando además su despliegue ágil en distintos entornos.

- **Docker**: Contenedores que permiten empaquetar el modelo junto con sus dependencias, asegurando portabilidad y reproducibilidad al desplegarse en diferentes ambientes.

- **ONNX (Open Neural Network Exchange)**: Formato universal para almacenar modelos, que facilita la interoperabilidad y el uso del modelo en múltiples plataformas y frameworks.


# 5. Despliegue de Modelo

El proceso de despliegue del modelo inicia con la validación del modelo previamente almacenado en producción, asegurando su correcto funcionamiento y estabilidad. Para ello, se implementan pruebas unitarias automatizadas mediante **GitHub Actions CI/CD**, que garantizan la calidad del modelo y del proceso de despliegue.

En caso de detectarse errores durante la validación, se realizan los ajustes necesarios para asegurar un paso exitoso a producción.

Una vez validado, se descarga el workflow desde Databricks para desplegar una API desarrollada con **Flask** en Python. Esta API estará disponible para que los médicos realicen consultas y obtengan resultados en tiempo real.

### Características de la API:

- **Entrada:** Datos clínicos del paciente.  
- **Salida:** Categorías de clasificación para enfermedades huérfanas.

Finalmente, todos los datos generados a partir de las predicciones se almacenan en un bucket de **Amazon S3**, garantizando su disponibilidad y trazabilidad para análisis futuros.


Una vez validado el modelo, se procede a descargar el workflow desde Databricks para desplegar una API desarrollada con **Flask** en Python. Esta API estará disponible para que los médicos puedan realizar solicitudes, obtener los resultados correspondientes y almacenar los datos procesados.

### Características de la API:

- **Entrada:** Datos clínicos del paciente.  
- **Salida:** Categorías de clasificación para enfermedades huérfanas.

Finalmente, todos los datos generados a partir de las predicciones se almacenan en un bucket de **Amazon S3**, asegurando su correcta conservación y disponibilidad para análisis posteriores.

# 8. Monitoreo de la Disponibilidad del Servicio

Es fundamental monitorear continuamente la disponibilidad del API para garantizar un servicio confiable y eficiente. Los aspectos clave a supervisar incluyen:

- **Tiempo de respuesta (latencia)** del API.  
- **Uso de recursos** en AWS, como CPU, memoria y ancho de banda.  
- **Detección y registro de errores**, idealmente mediante un sistema de gestión de logs que permita un control detallado y facilite la resolución de incidentes.

Implementar estas prácticas asegura la operatividad continua y la calidad del servicio ofrecido.



# 9. Monitoreo de Entrada y Uso del Modelo

Se debe construir un workflow personalizado en Databricks que permita validar la información que ingresa al modelo. Este proceso incluye la detección de:

- Nuevas categorías no previstas.  
- Cambios en la distribución de los datos.  
- Valores fuera de rango.

Para ello, se pueden utilizar histogramas que comparen la distribución actual de los datos con la del conjunto de entrenamiento. Esta información puede integrarse en un dashboard interactivo, el cual además podría generar alertas automáticas ante desviaciones relevantes.

Adicionalmente, se monitorea el rendimiento del modelo midiendo métricas clave como **accuracy**, **recall**, entre otras. Este monitoreo puede programarse para ejecutarse en batch periódicamente, optimizando costos operativos.

Para el registro y comparación de predicciones versus valores reales, se recomienda utilizar **MLflow**. Se asume que la empresa captura la información real de forma batch, almacenándola en el archivo txt histórico inicial, por lo que en el diagrama del sistema se refleja la conexión con el bucket de **Amazon S3** donde reside esta información.



También se evaluará periódicamente la correlación entre las variables independientes (X) y la variable dependiente (Y) durante la operación en producción. En caso de que alguna métrica supere un umbral predefinido, se generarán alertas automáticas.

Los datos de monitoreo se almacenarán en un bucket de **Amazon S3**, donde posteriormente serán evaluados mediante **Amazon CloudWatch** para verificar el cumplimiento de los umbrales establecidos.

Si se detecta una desviación significativa (supera el umbral), se activará una función **AWS Lambda** que disparará el workflow completo en Databricks, iniciando un nuevo ciclo de entrenamiento y actualización del modelo.





**Nota:** Paralelamente al pipeline, es fundamental considerar los siguientes servicios para garantizar un control y seguridad adecuados:

- **IAM (Identity and Access Management):** Controla los permisos de acceso a los recursos de AWS, definiendo quién puede acceder y si el acceso es de solo lectura o de escritura.

- **CloudTrail:** Registra las acciones realizadas en la infraestructura, incluyendo qué usuario ejecutó funciones, los accesos realizados y a qué recursos se accedió.

- **CloudWatch:** Monitorea el comportamiento de los distintos componentes del pipeline, como la latencia del API Gateway, uso de memoria en instancias EC2, y otros indicadores clave para asegurar la salud y rendimiento del sistema.
