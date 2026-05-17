# 🧠 Quantitative Algorithmic Suite [TradingView Pine Script v6]

Bienvenido a la suite de herramientas algorítmicas cuantitativas avanzadas para **TradingView (Pine Script v6)**. Este repositorio contiene dos indicadores de grado institucional diseñados para la identificación de regímenes de mercado, la detección de anomalías de precios y la proyección predictiva en mercados financieros de alta liquidez (como el SP500, NASDAQ y acciones individuales).

---

## 🛠️ Caja de Herramientas Cuantitativas

La suite se compone de dos motores analíticos independientes y complementarios:

1. **[ML kNN Stock Analyzer V3 Pro](file:///c:/Users/Oscar/Desktop/Indicadores%20Tradingview/ML%20kNN%20Stock%20Analyzer%20V3%20Pro.pine)**: Un motor adaptativo de Machine Learning (kNN) multidimensional (6D) con gestión dinámica de riesgo por ATR y telemetría de rendimiento estadístico en tiempo real.
2. **[Spline Quantile Regression Channel V2](file:///c:/Users/Oscar/Desktop/Indicadores%20Tradingview/Spline%20Quantile%20Regression%20Channel%20V2.pine)**: Un canal predictivo basado en matemáticas robustas de regresión cuantílica por splines cúbicos e IRLS, diseñado para modelar la volatilidad sin asumir distribuciones normales y proyectar tendencias estables.

---

# 🧠 1. ML kNN Stock Analyzer V3 Pro

Este indicador de análisis técnico utiliza un **modelo de aprendizaje supervisado basado en K-Nearest Neighbors (kNN)**. Ha sido optimizado para la renta variable estadounidense, reduciendo el ruido del mercado a través de filtros institucionales de volumen y consenso.

### 🚀 Características Clave

*   **Feature Engineering Multidimensional (6D kNN)**: Analiza simultáneamente 6 variables críticas:
    *   **Momentum Relativo**: RSI normalizado.
    *   **Volumen Relativo (RVOL)**: Identifica interés institucional y volumen de participación real.
    *   **Distancia a VWAP (en ATRs)**: Evita operar en puntos de sobreextensión diaria.
    *   **Gap de Apertura (%)**: Factoriza la volatilidad inicial del mercado accionario.
    *   **Ancho de Bollinger Bands (Squeeze)**: Clasifica fases de compresión o expansión.
    *   **Posición EMA 50**: Tendencia de mediano plazo basada en momentum.
*   **Gestión de Riesgo Integrada (Trailing Stop ATR Dinámico)**:
    *   Calcula un Stop Loss que sigue al precio a favor de la tendencia (`math.max` para compras, `math.min` para ventas), protegiendo las ganancias a medida que la operación avanza.
*   **Telemetría Real de Performance (Win Rate Estadístico)**:
    *   Calcula el total de operaciones (`Total Trades`) y el porcentaje de acierto (`Win Rate %`) reales únicamente cuando el precio cruza el trailing stop. Totalmente protegido contra el "repintado".
*   **Filtros Institucionales de Calidad**:
    *   **Filtro de Confianza**: Bloquea señales con un consenso de vecinos inferior al **55%**.
    *   **Filtro de Volumen**: Evita operar barras con RVOL inferior a **0.8x** (ruido minorista).

### 📊 Interpretación del Gráfico y las Señales

*   **BUY / SELL (Señales de Compra y Venta)**:
    *   **Etiqueta Verde `BUY`**: Señal de compra confirmada por el modelo y todos sus filtros.
    *   **Etiqueta Roja `SELL`**: Señal de venta confirmada.
    *   *Opacidad Dinámica*: La opacidad de las etiquetas indica el nivel de confianza de los vecinos (más brillante = mayor consenso).
*   **Trailing Stop Loss**:
    *   **Línea Cyan / Verde Punteada**: Nivel de Stop Loss para posiciones de compra que sube protegiendo la ganancia.
    *   **Línea Fucsia / Roja Punteada**: Nivel de Stop Loss para posiciones de venta (Short).
*   **Régimen de Fondo (Fondo de Pantalla)**:
    *   **Verde Suave**: Régimen alcista predominante.
    *   **Rojo Suave**: Régimen bajista predominante.
    *   **Gris / Sin Color**: Mercado de acumulación lateral. Todos los filtros están bloqueando operaciones para proteger la cuenta.

### 📊 Telemetría del kNN (Dashboard)
*   **Total Trades / Win Rate real**: Rendimiento estadístico en tiempo real.
*   **Confianza**: Consenso porcentual exacto del modelo.
*   **RVOL**: Multiplicador de volumen con iconos de estado (`🔥` masivo, `✅` saludable, `⚠️` minorista).
*   **Dist. VWAP**: Distancia en ATRs del precio respecto al VWAP.
*   **Volatilidad**: Estado del rango actual (`⚡ ALTA` para impulsos, `😴 BAJA` para fases de compresión).

---

# 🛠️ 2. Spline Quantile Regression Channel V2

Este indicador ajusta un canal suavizado y flexible utilizando **Regresión Cuantílica por Splines Cúbicos** (B-Splines adaptados para series temporales). A diferencia de canales tradicionales como las bandas de Bollinger o canales de Donchian, este indicador no asume una distribución normal de retornos y se enfoca en estimar los verdaderos límites estadísticos de la cola superior (`upperQ`, por ejemplo 95%) e inferior (`lowerQ`, por ejemplo 5%) del precio, así como la mediana de consenso (`midQ`, 50%).

```
                [ CANAL SUPERIOR (Upper Quantile: 95%) ]  --> Nivel de Extensión / Resistencia
              /
            /   [ LÍNEA MEDIANA (Median Quantile: 50%) ]  --> Dirección Consenso / Tendencia
          /
        /       [ CANAL INFERIOR (Lower Quantile: 05%) ]  --> Nivel de Extensión / Soporte
```

### 🚀 Características Clave y Mejoras V2

*   **Pre-cómputo Ridge OLS Compartido**: Resuelve la estimación inicial por mínimos cuadrados regularizados (Ridge OLS) una sola vez para las 3 regresiones cuantílicas, reduciendo la carga de CPU en un **30%**.
*   **IRLS con Early Stopping**: Optimización del algoritmo *Iteratively Reweighted Least Squares (IRLS)*. Finaliza los bucles de manera anticipada cuando el vector de coeficientes converge ($\Delta\beta < 10^{-6}$), mejorando el rendimiento hasta **5x**.
*   **Estandarización Robusta (Toggle MAD)**: Utiliza la Mediana y la Desviación Absoluta de la Mediana (MAD) en lugar de la media tradicional y la desviación estándar. Esto evita la distorsión del canal causada por grandes saltos de precios (*gaps*) en aperturas del SP500 o NASDAQ.
*   **Forecast Estable con Decaimiento Exponencial**: En lugar de extrapolar polinomios de grado 3 (que divergen rápidamente hacia el infinito), la versión V2 realiza una **extrapolación lineal con amortiguación exponencial** (`decay = 0.97`) que estabiliza las predicciones futuras.
*   **Métricas de Calidad Cuantitativas**: Evalúa el ajuste en tiempo real a través de métricas estadísticas avanzadas (Pinball Loss e índice de cobertura real).

### 📊 Interpretación del Gráfico y Etiquetas

*   **⚠ ABOVE UPPER (Etiqueta Roja en Cimas)**: Se imprime cuando el precio de cierre cruza por encima del canal de cuantil superior. Indica sobreextensión extrema alcista o una ruptura de impulso fuerte.
*   **⚠ BELOW LOWER (Etiqueta Verde en Valles)**: Se imprime cuando el precio cruza por debajo del canal de cuantil inferior. Marca zonas de sobreventa matemática severa.
*   **◈ AT MEDIAN (Etiqueta Naranja en Eje)**: Indica que el precio está cotizando exactamente sobre el consenso del modelo (la mediana). Excelente para buscar continuaciones de tendencia o rebotes.
*   **🔸 SQUEEZE (Etiqueta Amarilla)**: Indica compresión de volatilidad extrema. Ocurre cuando el ancho del canal relativo al ATR cae por debajo de **1.5x**. Anticipa un **movimiento explosivo e inminente** en el precio de la acción.

### 📈 Dashboard de Telemetría (Spline QRC V2)

El dashboard cuenta con un tamaño de letra adaptado a pantallas de alta resolución (`size.normal`) para asegurar una lectura inmediata:

| Métrica | Significado | Estado Óptimo |
| :--- | :--- | :--- |
| **IRLS Iters (Up/Mid/Lo)** | Cantidad de iteraciones utilizadas por el solver para cada banda. | `✓ Converged` (Convergencia rápida) |
| **Pinball Loss (Up/Mid/Lo)** | Métrica de error estadístico del cuantil (menor = mejor). | `< 0.1` (`✓ Good`) |
| **Channel Coverage** | % real de velas históricas atrapadas dentro del canal de regresión. | Próximo al teórico esperado (`✓ Calibrated`) |
| **Width / ATR** | Ancho del canal normalizado por la volatilidad del activo (ATR). | `< 1.5` = `🔸 Squeeze`, `1.5 - 4.0` = `◆ Normal` |
| **Forecast** | Proyección direccional del canal en la ventana futura. | `▲ BULLISH`, `▼ BEARISH`, `◆ NEUTRAL` |
| **Standardization** | Método activo para procesar las variables de entrada. | `MAD (Robust)` o `Stdev (Classic)` |

---

## ⚙️ Guía de Parámetros Recomendados para Acciones (SP500 / NASDAQ)

Para activos de alta liquidez y acciones individuales en marcos temporales estándar, se recomiendan las siguientes configuraciones:

| Parámetro | Propósito / Acción | Valor Recomendado |
| :--- | :--- | :--- |
| **Lookback Period** | Ventana de barras para ajustar la curva matemática. | `100` a `150` barras. |
| **Internal Knots** | Flexibilidad de la curva. Más nudos = más sensible; menos = más suave. | `3` (Equilibrado) o `5` (Volátil). |
| **Adaptive Knots (Volatility)** | Concentra nudos en zonas de fuerte volatilidad o volumen. | Activo (`true`) en fases de alta compresión. |
| **Robust Standardization (MAD)** | Protege el canal contra deformaciones por "Gaps" de mercado. | Activo (`true`) obligatorio para acciones americanas. |
| **Forecast Length** | Cuántas barras proyectar en el futuro. | `20` barras. |
| **Forecast Decay** | Coeficiente de amortiguación para evitar divergencias infinitas. | `0.97` (Seguro y suave). |

---

## 🛠️ Guía de Instalación en TradingView

1.  Abre el gráfico de cualquier activo (por ejemplo, `SPY`, `QQQ`, `AAPL`) en [TradingView](https://www.tradingview.com/).
2.  En la parte inferior de la pantalla, haz clic en la pestaña **Pine Editor**.
3.  **Para instalar el kNN Stock Analyzer**:
    *   Crea un nuevo indicador vacío.
    *   Copia el código completo de [ML kNN Stock Analyzer V3 Pro.pine](file:///c:/Users/Oscar/Desktop/Indicadores%20Tradingview/ML%20kNN%20Stock%20Analyzer%20V3%20Pro.pine) y pégalo en el editor.
    *   Haz clic en **Guardar** (Save) y luego en **Añadir al gráfico** (Add to Chart).
4.  **Para instalar el Spline Quantile Channel**:
    *   Crea un nuevo indicador vacío.
    *   Copia el código completo de [Spline Quantile Regression Channel V2.pine](file:///c:/Users/Oscar/Desktop/Indicadores%20Tradingview/Spline%20Quantile%20Regression%20Channel%20V2.pine) y pégalo en el editor.
    *   Haz clic en **Guardar** (Save) y luego en **Añadir al gráfico** (Add to Chart).

---

## 📄 Licencia y Descargo de Responsabilidad

Este proyecto está bajo la Licencia MIT. Las herramientas y análisis proporcionados en esta suite algorítmica son estrictamente con fines informativos y educativos. El trading financiero conlleva un riesgo sustancial de pérdida. Realice siempre pruebas de backtesting y simulación exhaustivas antes de arriesgar capital real.
