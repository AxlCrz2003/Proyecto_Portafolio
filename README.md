# Portafolio de Inversión - Teoría de Markowitz

Este fue mi trabajo final de la materia de Análisis de Riesgo y Portafolio de Inversión (Facultad de Economía). La idea era construir un portafolio con varias acciones, optimizarlo con la teoría de Markowitz y ver si le ganaba al mercado (S&P 500).

Primero lo hice completo en Excel (que era lo que pedía la materia) y después, ya por mi cuenta, me dio curiosidad ver qué tan rápido podía replicar lo mismo en Python. Así que aquí están las dos versiones.

## Qué hay en este repo

- `Trabajo final_Portafolio_LopezCruzOscarAxel.pdf` – el ensayo/reporte completo que entregué, con marco teórico, metodología y conclusiones.
- `TrabajoFinal_Portafolio_LopezCruzOscarAxel.xlsx` – el Excel con todo el cálculo: precios, rendimientos, matriz de covarianza, Solver para los pesos óptimos, CAPM, VaR/CVaR, etc.
- `Teoria-Moderna-Mkwz/Portafolio.ipynb` – la versión en Python del mismo ejercicio (o casi, algunos supuestos cambian un poco).

## Los activos

10 acciones: AAPL, IBM, MSFT, UNH, JNJ, Visa, AXP, Boeing, WMT y CAT, comparadas contra el S&P 500 como benchmark. Como tasa libre de riesgo usé CETES.

## Cómo lo hice en Excel

Los precios los saqué de investing.com (histórico de oct-2020 a oct-2025). De ahí:

1. Calculé rendimientos logarítmicos diarios
2. Armé la matriz de varianza-covarianza para ver correlación entre activos
3. Usé Solver para encontrar los pesos que maximizan el índice de Sharpe (el "portafolio óptimo" o tangente)
4. Calculé CAPM, Beta por activo, y las métricas de desempeño: Sharpe, Sortino, Alpha, Tracking Error, Information Ratio
5. Al final, VaR y CVaR paramétrico para medir el riesgo extremo

El modelo terminó dejando fuera casi la mitad de los activos (peso 0%) y concentrando el portafolio en unos pocos: IBM se llevó la mayor ponderación (27.46%), seguido de WMT (22.38%), AXP (19.25%) y CAT (17.01%). La lógica: IBM y WMT bajan el riesgo por su baja correlación con el resto, mientras que AXP y CAT jalan el rendimiento hacia arriba aunque sean más volátiles solas.

Resultado del portafolio óptimo: Beta de 0.83 (o sea, menos volátil que el mercado), Sharpe de 0.95, Sortino de 1.39 y un Alpha de 10.29% contra el benchmark. Nada mal para ser hecho a mano en Excel.

## La versión en Python

Quise ver si podía automatizar todo esto. Usé `yfinance` para bajar los precios directo (en vez de descargar un CSV de investing.com), y `scipy.optimize` para la optimización en lugar de Solver.

Además le agregué cosas que en Excel hubiera sido muy tedioso hacer a mano:

- Simulación de Monte Carlo con 5,000 portafolios aleatorios para visualizar la nube de combinaciones riesgo-retorno
- La frontera eficiente completa (no solo el punto óptimo)
- VaR paramétrico e histórico, para comparar ambos métodos

El periodo de datos es distinto al del Excel (2020-10-01 a 2026-04-01, porque lo corrí más tarde), así que los números no van a coincidir exactamente con el PDF — es normal, es la misma metodología pero con otra ventana de tiempo.

## Herramientas

**Excel:** Solver, tablas dinámicas, gráficas nativas

**Python:** `yfinance`, `numpy`, `pandas`, `scipy.optimize`, `matplotlib`

## Notas

Este proyecto lo hice como parte de la carrera, así que hay cosas que seguramente se pueden optimizar (por ejemplo, ahorita los límites de los pesos son 0-100% por activo, sin restricción de sector). Si le sigo metiendo mano en algún momento, probablemente agregue restricciones de sector o pruebe con más activos.

---
Oscar Axel López Cruz — Economía
