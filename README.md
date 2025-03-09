# Financial analysis pipeline with PostgreSQL and NiFi

## Descripción del proyecto

Este proyecto demuestra un **pipeline de análisis financiero** orientado a la detección de fraudes en transacciones con tarjetas de crédito. El flujo abarca desde la ingesta de datos (con Apache NiFi y PostgreSQL) hasta el análisis exploratorio (EDA) y el modelado de Machine Learning en Python.  
- **Ingesta y almacenamiento**: A través de un contenedor Docker que levanta NiFi, se captura un archivo CSV y se inserta en una base de datos PostgreSQL.  
- **Análisis exploratorio**: Se realiza con Python, generando un dataset limpio.  
- **Modelado ML**: Se entrenan múltiples modelos de clasificación (Logistic Regression, Random Forest y XGBoost) para detectar fraudes.


## Objetivos

1. **Automatizar la ingesta de datos** con NiFi y almacenarlos en una base de datos relacional (PostgreSQL).  
2. **Realizar un EDA** para identificar y limpiar valores duplicados o inconsistentes, y entender la distribución de la variable objetivo (fraude vs no fraude).  
3. **Entrenar modelos de Machine Learning** para la detección de fraudes, comparando distintas técnicas y métricas apropiadas para el desbalance de clases.  
4. **Mostrar un flujo de trabajo unificado** -> Docker + NiFi + PostgreSQL + Python.


## Tecnologías utilizadas



## Flujo de trabajo



## Conclusiones



## Próximas mejoras


