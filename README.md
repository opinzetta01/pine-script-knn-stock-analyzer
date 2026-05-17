# 🧠 ML kNN Stock Analyzer V2 [TradingView Pine Script v6]

Este es un indicador de análisis técnico algorítmico avanzado para TradingView desarrollado en **Pine Script v6**. Utiliza un **modelo adaptativo de Machine Learning basado en K-Nearest Neighbors (kNN)** optimizado específicamente para el mercado de **acciones** (Stocks), aplicando ponderación por distancia inversa, métricas institucionales clave y un sistema dinámico de gestión de riesgo.

A diferencia de los clasificadores kNN genéricos diseñados para Forex o Criptomonedas, esta versión 2.0 integra un motor multidimensional con 6 variables simultáneas y filtros avanzados contra la manipulación institucional y el ruido de mercado.

---

## 🚀 Características Clave (v2.0)

1. **Feature Engineering Multidimensional (6D kNN)**: Analiza simultáneamente 6 variables críticas optimizadas para renta variable:
   * **Momentum Relativo**: RSI normalizado.
   * **Volumen Relativo (RVOL)**: Identifica interés institucional y volumen de participación real.
   * **Distancia a VWAP (en ATRs)**: Evita operar en puntos de sobreextensión diaria y favorece la reversión a la media o la ruptura de consenso.
   * **Gap de Apertura (%)**: Factoriza la volatilidad inicial del mercado accionario.
   * **Ancho de Bollinger Bands (Squeeze)**: Clasifica si el mercado está en compresión o expansión.
   * **Posición EMA 50**: Tendencia de mediano plazo basada en momentum.
2. **K Dinámico y Adaptativo**: El número de vecinos analizados se ajusta en tiempo real según la volatilidad actual medida por ATR, buscando reacciones rápidas en tendencias expansivas y promedios estables en mercados laterales.
3. **Votación Ponderada por Distancia Inversa**: Los patrones históricos más similares al actual tienen un mayor peso e influencia en la votación del modelo.
4. **Filtros Institucionales de Calidad**:
   * **EMA 200 (Tendencia Macro)**: Solo permite compras (Longs) arriba de la EMA 200 y ventas (Shorts) abajo.
   * **Filtro de Confianza**: Bloquea cualquier señal con un consenso de vecinos inferior al **55%**.
   * **Filtro de Volumen Institucional**: Evita operar barras con RVOL inferior a **0.8x** (ruido minorista).
   * **Cooldown Temporal**: Bloquea la repetición consecutiva de señales para evitar el *overtrading* destructivo.

---

## 📊 Interpretación del Gráfico y las Señales

El indicador se integra visualmente de manera limpia y ofrece tres capas de interpretación sobre el gráfico:

### 1. Señales de Compra y Venta (BUY / SELL)
*   **Etiqueta Verde `BUY` (Debajo de la vela)**: Señal de Compra confirmada por el modelo y todos sus filtros.
*   **Etiqueta Roja `SELL` (Arriba de la vela)**: Señal de Venta confirmada.
*   **Gradiente de Confianza (Opacidad)**: El color de las etiquetas es translúcido si la confianza es baja (cercana al 55%), y sólido y brillante si la confianza se aproxima al 100%.

### 2. Franjas de Fondo (Regímenes de Mercado)
*   **Fondo Verde Suave**: Régimen Alcista Activo (Tendencia e impulso a favor de compras).
*   **Fondo Rojo Suave**: Régimen Bajista Activo (Tendencia e impulso a favor de ventas).
*   **Fondo Gris / Sin Color**: Mercado Neutral o de acumulación lateral. Todos los filtros están protegiendo la cuenta al bloquear operaciones.

### 3. Medias Móviles de Referencia
*   **Línea Naranja**: EMA 50 (Soporte/Resistencia dinámica de mediano plazo).
*   **Línea Azul**: EMA 200 (Frontera institucional para definir el sesgo macro alcista o bajista).

---

## 🧠 Telemetría del Dashboard (En Tiempo Real)

El indicador incluye una tabla de telemetría personalizable (disponible en tamaños: *Pequeño*, *Normal* y *Grande*) que muestra el estado de las variables procesadas en la vela actual:

*   **Predicción**: Indica si la recomendación del modelo es `🟢 COMPRA`, `🔴 VENTA` o `⚪ NEUTRAL`.
*   **Confianza**: El porcentaje exacto de vecinos que votaron a favor de la dirección.
*   **Régimen**: El sesgo de tendencia macro (`📈 ALCISTA` o `📉 BAJISTA`).
*   **K Dinámico**: El número adaptativo actual de vecinos analizados.
*   **Alcistas/Bajistas**: Desglose exacto de los votos (ej. `18 / 7`).
*   **RVOL**: Multiplicador de volumen. Muestra iconos de estado (`🔥` volumen masivo, `✅` saludable, `⚠️` volumen minorista).
*   **Dist. VWAP**: Distancia en ATRs del precio respecto al VWAP diario.
*   **Volatilidad**: Estado del rango actual (`⚡ ALTA` para impulsos, `😴 BAJA` para fases de compresión pre-ruptura).
*   **Datos Entren.**: Cantidad de barras históricas indexadas en la base de datos local en la memoria RAM del script.

---

## ⚙️ Guía de Parámetros Recomendados

| Parámetro | Day Trading (Velocidad) | Swing Trading (Robustez) |
|-----------|---------------------|----------------------|
| **Velas (Timeframe)** | 5m, 15m, 1h | 1D, 1W |
| **K Base** | 150 | 252 (Un año de datos diarios) |
| **Barras Etiquetado** | 1 - 2 | 3 - 5 |
| **EMA Rápida** | 20 | 50 |
| **EMA Lenta** | 50 | 200 |
| **Cooldown Bars** | 3 vel. | 5 - 10 vel. |
| **RVOL Mínimo** | 1.0x | 0.8x |
| **Umbral Confianza** | 60% | 55% |

---

## 🛠️ Instalación en TradingView

1. Abre el gráfico de cualquier acción en [TradingView](https://www.tradingview.com/).
2. Haz clic en la pestaña **Pine Editor** en la parte inferior de la pantalla.
3. Borra el código existente y pega el contenido completo del archivo `ML kNN Stock Analyzer V2.pine`.
4. Haz clic en **Añadir al gráfico** (Add to chart).
5. Configura los parámetros a tu gusto haciendo clic en el engranaje `⚙️` en el nombre del indicador.

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Úsalo bajo tu propio análisis y gestión de riesgo.
