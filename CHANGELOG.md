# Changelog

## [1.1.0] - 2025-05-26
### Fixed

- Modificación en el pipeline:
- Se corrigió las **fuentes de datos** antes dependían de varias fuentes ahora se tiene que solo depende de una.
- Ampliación en el detalle de la **Ingesta y preparación de datos** detallando cada una de los procesos internos y aplicándolos en el documento. 
- Ampliación en el detalle de **Entrenamiento del modelo** detallando en pasos gruesos cada uno de los subprocesos con su respectiva explicación.
- Ampliación en el detalle de **Elección del modelo** detallado de los pasos principales del proceso incluyendo registro, empaquetado y almacenamiento del modelo y sus respectivas tecnologías.
- Modificación en el **despliegue del modelo** agregando la validación del paso a producción y dejando únicamente el despliegue en al api.

### Removed

- **Monitoreo y control del modelo** Se eliminó esta parte para generar unas nuevas funcionalidades la reemplazen y fortalezcan.

### Added

- Se agrego la fase de **Monitoreo en la disponibilidad del servicio** donde se debe monitorear el funcionamiento de la api.
- Se agrego la fase de **Monitoreo de entrada y uso del modelo** donde se validan los datos que entran al modelo, el rendimiento del modelo, y el cambio del desarrollo del modelo.
- Se agrego la fase de **Almacenamiento de monitoreo y control del trigger** cual se construye en S3.
- Se agrega la regla **cumplir con el umbral** la cual valida si el modelo está en en los parámetros deseados o necesita un ajuste, en caso de necesitar un ajuste se envía un función lambda que dispara todo el workflow en databricks para el entrenamiento del modelo.
- Se agrega una función lambda en la parte de Almacenamiento de datos que permite tener esta información para el monitoreo del performance del modelo.
- Agregan comentarios para realizar pruebas unitarias y el desarrollo de  todo el proyecto con github.


## [1.0.0] - 2025-05-20

### Added
- Pipeline inicial del proyecto (ajustado, según recomendaciones del maestro)
- Cuenta con las características de fuentes de datos, ingesta de datos, almacenamiento de datos, entrenamiento del modelo, elección del mejor modelo, despliegue del modelo, predicciones y monitoreo y control.
  

