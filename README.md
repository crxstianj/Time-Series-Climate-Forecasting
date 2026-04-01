# Análisis de Pronósticos en Temperatura y Humedad  


## Descripción del Proyecto

Este proyecto presenta un análisis comparativo de modelos de series temporales aplicados a datos de **temperatura y humedad** registrados durante la ola de calor de 2024 en Salamanca, Guanajuato.

Se implementaron y evaluaron los siguientes enfoques:

- Modelo Ingenuo
- ARIMA
- SARIMA
- Redes Neuronales Feedforward

El objetivo principal fue determinar qué técnica ofrece mayor precisión predictiva bajo un esquema de **validación walk-forward**, considerando la presencia de estacionalidad y posibles relaciones no lineales en los datos climáticos.

---

## Contexto

Las olas de calor en México han aumentado en intensidad y frecuencia debido al cambio climático. Contar con modelos predictivos confiables puede apoyar en:

- Planeación de emergencias  
- Gestión de recursos  
- Toma de decisiones ante eventos climáticos extremos  

---

## Dataset

- Datos recolectados a través de sensores ambientales integrados en dispositivos Arduino.
- Variables: Temperatura (°C) y Humedad relativa (%)
- Frecuencia: Cada 10 minutos
- Duración: 5 días
- Periodo: Junio 2024
- Ubicación: Salamanca, Guanajuato

### Preprocesamiento

- Interpolación lineal de datos faltantes  
- Resampleo a intervalos uniformes de 30 minutos  
- Descomposición en tendencia, estacionalidad y residuo  
- Prueba de estacionariedad (ADF)  

---

## Modelos Implementados

### 1. Modelo Ingenuo

Sirve como benchmark para evaluar mejoras reales de modelos más complejos.

---

### 2. ARIMA (AutoRegressive Integrated Moving Average)

Configuración utilizada:

- p = 2  
- d = 1  
- q = 1  

Se aplicó diferenciación tras prueba ADF.

Resultado: No superó al modelo ingenuo.

---

### 3. SARIMA (Seasonal ARIMA)

Se detectó estacionalidad diaria (24 horas).  
Dado que los datos son cada 30 minutos:

- m = 48  

Parámetros:

No estacionales:
- p = 2  
- d = 1  
- q = 1  

Estacionales:
- P = 1  
- D = 1  
- Q = 1  
- m = 48  

Este modelo capturó adecuadamente la estacionalidad diaria.

---

### 4. Redes Neuronales Feedforward

Arquitectura final:

- 2 capas ocultas:
  - 64 neuronas (ReLU)  
  - 32 neuronas (ReLU)  
- Salida: 1 neurona  
- Función de pérdida: MSE  
- Optimizador: Adam (lr = 0.001)  

Entrenamiento:
- 50% datos iniciales  
- Reentrenamiento continuo en esquema walk-forward  

---

## Validación: Walk-Forward

Metodología aplicada:

1. Entrenamiento con 50% inicial (2.5 días)  
2. Predicción del siguiente punto  
3. Agregar dato real  
4. Reentrenar  
5. Repetir hasta finalizar la serie  

Este enfoque simula un escenario real de predicción en producción.

---

## Métricas de Evaluación

- MAE (Mean Absolute Error)  
- RMSE (Root Mean Squared Error)  

---

## Resultados

### Modelo Ingenuo

| Variable     | MAE   | RMSE  |
|-------------|--------|--------|
| Temperatura | 0.134  | 0.183  |
| Humedad     | 0.953  | 1.314  |

---

### SARIMA

| Variable     | MSE   | RMSE  |
|-------------|--------|--------|
| Temperatura | 0.031  | 0.177  |
| Humedad     | 1.494  | 1.222  |

Mejor modelo para capturar estacionalidad, especialmente en temperatura.

---

### Redes Neuronales Feedforward

| Variable     | MSE   | RMSE  |
|-------------|--------|--------|
| Temperatura | 0.113  | 0.336  |
| Humedad     | 1.953  | 1.398  |

Capturan relaciones no lineales, pero requieren mayor ajuste para superar SARIMA.

---

## Discusión

- ARIMA no logró capturar patrones significativos.  
- SARIMA fue el modelo más robusto para temperatura.  
- La humedad presentó mayor complejidad estructural.  
- Las redes neuronales muestran potencial, pero requieren optimización adicional.  
- El modelo ingenuo es sorprendentemente competitivo para temperatura.  


---

## Tecnologías Utilizadas

- Python  
- NumPy  
- Pandas  
- Statsmodels  
- Scikit-learn  
- Keras 
- Matplotlib  

