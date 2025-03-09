# Financial analysis pipeline with PostgreSQL and NiFi

<div align="center">
  <img src="https://github.com/user-attachments/assets/b695e7b2-9c09-4ab1-8560-6186d41334bd">
</div>

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
- **Docker**: Despliega contenedor para NiFi.  
- **Apache NiFi**: Orquestación e ingesta de datos (lectura del CSV y carga en PostgreSQL).  
- **PostgreSQL**: Almacena las transacciones de tarjetas de crédito.  
- **Python**:  
  - **EDA**: Limpieza, análisis de distribuciones, detección de outliers/duplicados.  
  - **Machine Learning**: Entrenamiento y comparación de modelos.  
  - Librerías: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `psycopg2`.  


## Flujo de trabajo
1. **Contenedor Docker con NiFi (1.21)**:  
   - NiFi se arranca en un contenedor, mapeando un volumen donde se encuentra el CSV original, se hace uso de un Dockerfile.  
   - Se configura un flujo en NiFi para leer el archivo (`GetFile`) y cargarlo en PostgreSQL (`PutDatabaseRecord`).
   - Comandos para levantar el contenedor (modificar según el caso):
     - docker build -t nifi-with-pgdriver:1.21 .
     - docker run --name nifi -p 9090:8080 -e NIFI_WEB_HTTP_PORT=8080 -v "C:/Users/Ruben/Desktop/Data:/data" -d nifi-with-pgdriver:1.21
   - Se accede a NiFi a través del navegador -> http://localhost:9090/nifi

Prueba de funcionamiento en Docker:
<div align="center">
  <img src="https://github.com/user-attachments/assets/dee8be2d-fb83-4041-b4e1-e53599d76eb2">
</div>

Configuraciones en NiFi:

<div align="center">
  <img src="https://github.com/user-attachments/assets/a878e6f7-5a6a-4994-a603-6265e3ee438e">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/10bcef11-65eb-44bb-9797-d997c0cb2336">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/25284950-105d-4f1d-af92-509cca6f0ece">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/6ec2324f-38e6-4e34-943a-7156295b76dc">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/2c98fe4b-7b4d-4e89-8360-3714b4ab96e5">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/7a7beb12-bd25-4edc-9895-7ce76549b59d">
</div>

2. **Almacenamiento en PostgreSQL**:  
   - NiFi inserta los datos en la tabla `creditcard_transactions` de la base de datos `fraude_db`.  
   - Se verifica la correcta inserción (conteo de filas, etc.).

<div align="center">
  <img src="https://github.com/user-attachments/assets/e66a4b8d-c2bc-4845-b3a2-60fcba1a3426">
</div>


3. **EDA (Exploratory Data Analysis) en Python**:  
   - Con `pandas`, se extraen los datos desde PostgreSQL (vía `psycopg2`).  
   - Se examinan duplicados, valores nulos, distribución de la variable objetivo y de otras variables (`Time`, `Amount`, etc.).  
   - Se genera un **dataset limpio** (`creditcard_clean.csv`).  
4. **Modelado de ML**:  
   - En un notebook dedicado a modelos, se cargan los datos limpios.  
   - Se aplica `train_test_split` con `stratify=y` por el desbalance de la clase fraude.  
   - Se entrenan distintos algoritmos, evaluando con métricas como AUPRC, ROC-AUC y la matriz de confusión.  
   - Opcionalmente, se realiza optimización de hiperparámetros con `GridSearchCV` o `RandomizedSearchCV`.  
5. **Comparación de resultados**:  
   - Se analizan precisión, recall y otras métricas para determinar el mejor modelo.

## Conclusiones
- **Ingesta automatizada**: NiFi facilita la orquestación de flujos de datos, reduciendo pasos manuales.  
- **Dataset desbalanceado**: El fraude ~0.17% exige métricas específicas (AUPRC, recall de la clase minoritaria).  
- **Random Forest / XGBoost** han ofrecido altos valores de recall y precisión, superando a la regresión logística.
- **Infraestructura reproducible**: Con Docker se simplifica la puesta en marcha y se asegura la portabilidad del pipeline.

## Próximas mejoras

1. **Implementar un orquestador** como Airflow, Prefect para programar la ejecución recurrente y el refresco de datos.  
2. **Balance de clases** avanzado con técnicas como SMOTE o undersampling para mejorar la recall de la clase de fraude.  
3. **Streaming**: Adaptar NiFi para procesar transacciones en tiempo real (Kafka/NiFi).  
4. **Monitoreo del modelo**: Integrar una capa MLOps (por ejemplo, MLflow o un dashboard) para vigilar la deriva del modelo en producción.  
5. **Seguridad**: Añadir cifrado de datos sensibles y autenticación/seguridad avanzada en NiFi y PostgreSQL.

