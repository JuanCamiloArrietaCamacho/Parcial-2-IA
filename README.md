report_text = """# Reporte – Clasificación de Ingresos

## 1. Decisiones de procesamiento de datos

- **Uso de características**:  
  Se utilizaron todas las variables originales del dataset *Adult* (edad, nivel educativo, ocupación, sexo, horas por semana, etc.). No se crearon variables nuevas derivadas de combinaciones.

- **Tratamiento de valores faltantes**:  
  - Variables numéricas → imputadas con la **mediana**.  
  - Variables categóricas → imputadas con el valor `"Unknown"`.  

- **Transformaciones aplicadas**:  
  - **Numéricas**: estandarización con `StandardScaler` (media 0, varianza 1).  
  - **Categóricas**: codificación con **One-Hot Encoding** (`OneHotEncoder`).  

- **Balanceo de clases**:  
  Se aplicó **SMOTE (Synthetic Minority Oversampling Technique)** para crear ejemplos sintéticos de la clase minoritaria (>50K), corrigiendo el desbalance.

- **Separación de conjuntos**:  
  - `train`: entrenamiento del modelo.  
  - `validación`: comparación de configuraciones (hiperparámetros).  
  - `Prueba final`: evaluación del mejor modelo, sin usarse en fases anteriores.

---

## 2. Hiperparámetros del mejor MLP

El mejor modelo fue **Experimento4**, con la siguiente configuración base:

- Arquitectura: `[1024, 512]` (dos capas ocultas de 1024 y 512 neuronas).  
- Función de activación: ReLU.  
- Dropout: `0.3`.  
- Learning rate: `1e-4`.  
- Weight decay (regularización L2): `1e-3`.  
- Batch size: `1024`.  
- Épocas máximas: `60`.  

### Variantes probadas en la arquitectura
Durante los experimentos se probaron también:
- `[1024, 512, 256]`  
- `[1024, 512, 128]`  
- `[1024, 512, 2]`  

En todos los casos, el modelo con arquitectura `[1024, 512]` se mantuvo como el más robusto y con mejor desempeño en validación y test, superando a los **experimentos 1, 2, 3 y 5**.

### Comparación con y sin regularización
- **Sin regularización** (Experimento1, dropout=0, weight_decay=0):  
  - Training loss bajó rápidamente, pero validation loss se disparó → Esto es un síntoma de *overfitting*.  
  - AUC menor en validación.  

- **Con regularización (Experimento4)**:  
  - Training y validation loss se mantuvieron cercanos y estables.  
  - Mejor **ROC AUC = 0.915** en test.  

👉 **Conclusión**: La mejor combinación fue una red de alta capacidad con regularización adecuada, que evitó el sobreajuste y mejoró la generalización.

---

## 3. Comparación del mejor MLP vs regresión logística

- **Regresión logística (baseline)**:  
  - ROC AUC en test: **0.9075**  
  - Ventajas: simple, interpretable, muy competitivo.

- **Mejor MLP (exp4)**:  
  - ROC AUC en test: **0.9147**  
  - Accuracy: **0.827**  
  - Precision: **0.597**  
  - Recall: **0.829**  
  - F1: **0.694**  

### Interpretación
- Ambos modelos presentan un rendimiento sólido y acertado.  
- El MLP mejora ligeramente el AUC respecto a la regresión logística (0.7 puntos por encima de la regresión logística).  
- El MLP captura mejor relaciones no lineales entre variables.  
- Sin embargo, la regresión logística sigue siendo una opción fuerte si se prioriza la simplicidad e interpretabilidad.

👉 **Conclusión** 
El MLP es superior en desempeño, pero la ganancia debe evaluarse frente al mayor costo computacional ya que este necesitó de más ram y se necesitó correrlo mediante GPU y puede ser más difícil de interpretar.

**Nota**: Para la realización de este trabajo se utilizó la asistencia de herramientas de inteligencia artificial como ChatGPT y Gemini con el objetivo de corregir errores de sintaxis en el código y aclarar conceptos netamente teóricos. Todas las decisiones de modelado, procesamiento de datos y selección de hiperparámetros fueron tomadas de manera autónoma en el desarrollo del proyecto.
"""
