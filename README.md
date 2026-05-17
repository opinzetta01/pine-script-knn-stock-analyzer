# 🧠 ML kNN Stock Analyzer V3 Pro [TradingView Pine Script v6]

Este es un indicador de análisis técnico algorítmico avanzado para TradingView desarrollado en **Pine Script v6**. Utiliza un **modelo adaptativo de Machine Learning basado en K-Nearest Neighbors (kNN)** optimizado específicamente para el mercado de **acciones** (Stocks), aplicando ponderación por distancia inversa, métricas institucionales clave y un avanzado **sistema dinámico de gestión de riesgo con telemetría de rendimiento real**.

Esta versión **V3 Pro** es el resultado de fusionar la inteligencia analítica multidimensional del V2 (6 dimensiones) con un sistema de gestión de capital institucional (Trailing Stop basado en ATR) y evaluación estadística en tiempo real.

---

## 🚀 Características Clave (v3.0 Pro)

1. **Feature Engineering Multidimensional (6D kNN)**: Analiza simultáneamente 6 variables críticas optimizadas para renta variable:
   * **Momentum Relativo**: RSI normalizado.
   * **Volumen Relativo (RVOL)**: Identifica interés institucional y volumen de participación real.
   * **Distancia a VWAP (en ATRs)**: Evita operar en puntos de sobreextensión diaria y favorece la reversión a la media o la ruptura de consenso.
   * **Gap de Apertura (%)**: Factoriza la volatilidad inicial del mercado accionario.
   * **Ancho de Bollinger Bands (Squeeze)**: Clasifica si el mercado está en compresión o expansión.
   * **Posición EMA 50**: Tendencia de mediano plazo basada en momentum.

2. **Gestión de Riesgo Integrada (Trailing Stop ATR Dinámico)**:
   * Implementa un sistema de Stop Loss dinámico que sigue al precio basado en la volatilidad actual (múltiplo de ATR).
   * El stop "persigue" el precio a favor de la tendencia (`math.max` para posiciones largas, `math.min` para cortas), protegiendo las ganancias a medida que la operación avanza.
   * Permite definir y visualizar en el gráfico cuándo una operación se cierra definitivamente por tocar el Trailing Stop.

3. **Telemetría Real de Performance (Win Rate Estadístico)**:
   * El sistema rastrea métricas de operaciones cerradas reales (no simuladas).
   * Calcula de manera precisa el total de operaciones (`Total Trades`) y el porcentaje de acierto (`Win Rate %`) únicamente cuando el precio cruza el trailing stop.
   * Totalmente protegido contra "repintado": las evaluaciones se hacen en base a cierres de vela y estados históricos pasados sin mirar al futuro.

4. **Filtros Institucionales de Calidad**:
   * **Filtro de Confianza**: Bloquea cualquier señal con un consenso de vecinos inferior al **55%**.
   * **Filtro de Volumen Institucional**: Evita operar barras con RVOL inferior a **0.8x** (ruido minorista).

---

## 📊 Interpretación del Gráfico y las Señales

El indicador se integra visualmente de manera limpia y ofrece múltiples capas de interpretación:

### 1. Señales de Compra y Venta (BUY / SELL)
*   **Etiqueta Verde `BUY` (Debajo de la vela)**: Señal de Compra confirmada por el modelo y todos sus filtros.
*   **Etiqueta Roja `SELL` (Arriba de la vela)**: Señal de Venta confirmada.
*   **Gradiente de Confianza (Opacidad)**: El color de las etiquetas es translúcido si la confianza es baja, y sólido/brillante si la confianza es alta.

### 2. Trailing Stop Loss Dinámico
*   **Línea Fucsia / Roja Punteada**: Se dibuja durante una posición activa de venta (Short), marcando el nivel límite del Stop Loss.
*   **Línea Cyan / Verde Punteada**: Se dibuja durante una posición activa de compra (Long), mostrando cómo el Stop Loss sube acompañando el precio.
*   El cruce del precio con esta línea indica el cierre (salida) de la posición.

### 3. Franjas de Fondo (Regímenes de Mercado)
*   **Fondo Verde Suave**: Régimen Alcista Activo.
*   **Fondo Rojo Suave**: Régimen Bajista Activo.
*   **Fondo Gris / Sin Color**: Mercado Neutral o de acumulación lateral. Todos los filtros están protegiendo la cuenta al bloquear operaciones.

---

## 🧠 Dashboard de Telemetría (En Tiempo Real)

El indicador incluye una tabla de telemetría expandida y personalizable que muestra el estado de las variables y el rendimiento histórico de la estrategia:

*   **Total Trades**: Cantidad total de operaciones completadas desde el inicio del historial del gráfico.
*   **Win Rate real**: Porcentaje estadístico de precisión en las operaciones pasadas.
*   **Predicción**: `🟢 COMPRA`, `🔴 VENTA` o `⚪ NEUTRAL`.
*   **Confianza**: El porcentaje exacto de vecinos que votaron a favor de la dirección.
*   **Régimen**: El sesgo de tendencia macro (`📈 ALCISTA` o `📉 BAJISTA`).
*   **K Dinámico**: Número adaptativo actual de vecinos analizados.
*   **RVOL**: Multiplicador de volumen con iconos de estado (`🔥` masivo, `✅` saludable, `⚠️` minorista).
*   **Dist. VWAP**: Distancia en ATRs del precio respecto al VWAP.
*   **Volatilidad**: Estado del rango actual (`⚡ ALTA` para impulsos, `😴 BAJA` para fases de compresión).
*   **Datos Entren.**: Cantidad de barras históricas indexadas en el motor del modelo.

---

## ⚙️ Guía de Parámetros Recomendados

| Parámetro | Day Trading (Velocidad) | Swing Trading (Robustez) |
|-----------|---------------------|----------------------|
| **Velas (Timeframe)** | 5m, 15m, 1h | 1D, 1W |
| **K Base** | 150 | 252 |
| **Barras Etiquetado** | 1 - 2 | 3 - 5 |
| **EMA Rápida** | 20 | 50 |
| **EMA Lenta** | 50 | 200 |
| **Multiplicador ATR** | 2.5 - 3.0 | 3.5 - 4.0 |
| **RVOL Mínimo** | 1.0x | 0.8x |
| **Umbral Confianza** | 60% | 55% |

*Nota: Asegúrate de ajustar el **Multiplicador ATR** de la gestión de riesgo según la volatilidad específica del activo. Un valor de 2.5 suele ser estándar, pero acciones más volátiles pueden requerir valores mayores (3.0 o más).*

---

## 🛠️ Instalación en TradingView

1. Abre el gráfico de cualquier acción en [TradingView](https://www.tradingview.com/).
2. Haz clic en la pestaña **Pine Editor** en la parte inferior de la pantalla.
3. Borra el código existente y pega el contenido completo del archivo `ML kNN Stock Analyzer V3 Pro.pine`.
4. Haz clic en **Añadir al gráfico** (Add to chart).
5. Configura los parámetros a tu gusto haciendo clic en el engranaje `⚙️` en el nombre del indicador.

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Úsalo bajo tu propio análisis y gestión de riesgo.
