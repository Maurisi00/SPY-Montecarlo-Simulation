# Modelización de Riesgo Financiero: Simulación de Monte Carlo

## Descripción del Proyecto
Este proyecto consiste en la implementación de un modelo estocástico para simular la evolución futura del precio de activos financieros. El objetivo principal es cuantificar el riesgo de mercado mediante el cálculo del **Value at Risk (VaR)** y el **Conditional VaR (CVaR)**.

El código descarga datos históricos reales, calcula los parámetros estadísticos del activo (retorno esperado y volatilidad) y proyecta miles de escenarios posibles utilizando el Movimiento Browniano Geométrico (GBM). Además, incluye un módulo de *Stress Testing* para evaluar la solvencia de la cartera ante aumentos repentinos de volatilidad.

## Metodología Matemática

El núcleo de la simulación se basa en el **Movimiento Browniano Geométrico**, definido por la ecuación diferencial estocástica:

$$dS_t = \mu S_t dt + \sigma S_t dW_t$$

Para la discretización en la simulación computacional, se utiliza la solución exacta en tiempo discreto:

$$S_{t+1} = S_t \cdot \exp\left( (\mu - \frac{1}{2}\sigma^2)\Delta t + \sigma \sqrt{\Delta t} Z \right)$$

Donde:
- **S**: Precio del activo.
- **μ (mu)**: Deriva (retorno promedio histórico).
- **σ (sigma)**: Volatilidad histórica.
- **Z**: Variable aleatoria estándar normal N(0,1).

Se han utilizado **retornos logarítmicos** en lugar de aritméticos para asegurar la aditividad temporal y la normalidad de la distribución de los rendimientos.

## Características Técnicas

* **Extracción de Datos (ETL):** Conexión a la API de Yahoo Finance (`yfinance`) para la obtención automatizada de series temporales ajustadas.
* **Vectorización con NumPy:** Implementación eficiente de la simulación mediante operaciones matriciales, evitando el uso de bucles `for` para procesar 10,000 escenarios simultáneamente.
* **Métricas de Riesgo:**
    * **VaR (95%):** Máxima pérdida esperada con un 95% de confianza.
    * **CVaR (Expected Shortfall):** Pérdida promedio en el 5% de los peores escenarios (cola izquierda de la distribución).
* **Análisis de Sensibilidad (Stress Testing):** Evaluación del impacto en el VaR ante un escenario de duplicación de la volatilidad ($\sigma \times 2$).

## Stack Tecnológico

* **Python 3.x**
* **NumPy:** Cálculo numérico y generación de procesos estocásticos.
* **Pandas:** Manipulación y limpieza de series temporales financieras.
* **Matplotlib:** Visualización de rutas de precios (spaghetti plots) e histogramas de distribución de pérdidas.
* **yfinance:** Fuente de datos de mercado.

## Estructura del Código

1.  **Configuración:** Definición de parámetros (ticker, horizonte temporal, número de simulaciones).
2.  **Cálculo de Parámetros:** Estimación de drift y volatilidad basada en datos históricos (2018-2023).
3.  **Motor de Simulación:** Generación de la matriz de precios futuros mediante `numpy.cumprod`.
4.  **Análisis:** Cálculo de percentiles para VaR y visualización de resultados.
5.  **Stress Test:** Simulación paralela con parámetros de riesgo alterados.

## Cómo Ejecutar

El proyecto está diseñado para ejecutarse en un entorno de Jupyter Notebook o Google Colab.

1.  Clonar el repositorio.
2.  Instalar las dependencias:
    ```bash
    pip install numpy pandas matplotlib yfinance
    ```
3.  Ejecutar el script principal o abrir el notebook.

## Resultados
El modelo permite visualizar la dispersión de precios a un año vista y ofrece una estimación numérica del capital en riesgo. El módulo de Stress Testing demuestra cómo el VaR no es estático y aumenta significativamente en regímenes de alta volatilidad, evidenciando el riesgo de "colas pesadas" (fat tails) en los mercados financieros.


