# Modelación dinámica de la Tasa de Intervención en Colombia: un enfoque ARIMA-X

Seminario del curso **Series de Tiempo I** (Estadística, Universidad de Antioquia, 2026-2) — Grupo 3.

**Integrantes:** Walys Vera Herrera · Iván Andrés Durango Cogollo · Nicolás Buenaventura Schlesinger

## De qué se trata

Los modelos ARIMA explican una serie solo con su propio pasado. Los modelos **ARIMA-X**
(regresión con errores ARIMA) agregan **variables exógenas**: otras series, dummies de
intervención o atípicos, cuyo efecto se mide con un coeficiente, mientras la parte ARIMA
recoge la inercia que queda en los errores.

Como aplicación modelamos la **Tasa de Intervención del Banco de la República** en función
del **Índice de Producción Industrial (IPI, DANE)**, con datos mensuales de enero de 2014 a
agosto de 2026 (152 observaciones), una dummy para el choque de la pandemia (marzo a
octubre de 2020) y rezagos del IPI identificados por pre-blanqueamiento.

## Estructura

```
Seminario/
├── arimax_ipi.ipynb      análisis completo (kernel de R)
├── arimax_ipi.R          el mismo código como script para RStudio
├── Datos/                CSV de entrada (IPI del DANE y tasa del BanRep)
├── graficos/             figuras que genera el notebook (PDF)
├── resultados/           tablas que genera el notebook (CSV)
├── Documentos/           fuente LaTeX y PDF del entregable escrito
├── Presentacion/         diapositivas en Beamer
├── renv.lock, .Rprofile, renv/   entorno de paquetes (renv)
```

## Resultados principales

| | Valor |
|---|---|
| Orden de integración (ADF y KPSS) | `d = 1` en la tasa y en el IPI |
| Estructura de los errores (AIC) | ARIMA(4,1,1) |
| Ljung-Box sobre los residuos (24 rezagos) | Q = 17.03, p = 0.588: ruido blanco |
| Coeficientes del IPI y de la dummy COVID | no significativos (p > 0.15) |

Pronóstico fuera de muestra de los últimos 12 meses (sep 2025 a ago 2026):

| Modelo | AIC | BIC | RMSE | MAE | MAPE |
|---|---|---|---|---|---|
| ARIMA(4,1,1) | **−52.78** | **−35.44** | **0.994** | **0.772** | **6.92 %** |
| ARIMA-X: IPI + COVID | −49.28 | −26.16 | 1.009 | 0.782 | 7.01 % |
| ARIMA-X: IPI con rezagos 3 y 6 + COVID | −46.89 | −17.99 | 1.028 | 0.794 | 7.10 % |

El ARIMA univariado queda ligeramente por delante: la inercia de la propia tasa domina
el pronóstico de corto plazo, y el alza de 2026 respondió a presiones inflacionarias que
un modelo bivariado con el IPI no recoge.

## Cómo reproducir el análisis

Requisitos: R 4.x. El proyecto usa **renv**, así que las versiones exactas de los
paquetes están en `renv.lock`. Al abrir R en esta carpeta, renv se activa solo; para
instalar todo:

```r
renv::restore()
```

1. Verificar que en `Datos/` estén `ipi_mensual.csv` (DANE) y
   `tasa_politica_monetaria.csv` (SUAMECA, Banco de la República).
2. Abrir `arimax_ipi.ipynb` en VS Code (extensión Jupyter) y seleccionar el kernel de R,
   o abrir `arimax_ipi.R` en RStudio.
3. Ejecutar todo. Las figuras se regeneran en `graficos/` y las tablas en `resultados/`.

## Referencias

- Amézquita Bravo, C. C. (2021). *Evaluación de modelos de series de tiempo para pronosticar la demanda de transporte aéreo a corto y mediano plazo en Colombia* [Tesis de maestría]. Universidad Nacional de Colombia.
- Box, G. E. P. y Tiao, G. C. (1975). Intervention analysis with applications to economic and environmental problems. *Journal of the American Statistical Association*, 70(349), 70–79.
- Chen, C. y Liu, L.-M. (1993). Joint estimation of model parameters and outlier effects in time series. *Journal of the American Statistical Association*, 88(421), 284–297.
- Fan, J. (2009). The Analysis and Application of ARIMAX Model. En *Proceedings of the Conference on Time Series and Econometric Applications* (pp. 156–159).
- Hyndman, R. J. y Athanasopoulos, G. (2021). *Forecasting: Principles and Practice* (3a ed.). OTexts.
- Ljung, G. M. y Box, G. E. P. (1978). On a measure of lack of fit in time series models. *Biometrika*, 65(2), 297–303.
- Ortiz Martín, R. (2022). *Metodología ARIMAX aplicada al análisis de la evolución de datos de suicidios en España* [Trabajo de fin de grado]. Universitat de Barcelona.
- Shumway, R. H. y Stoffer, D. S. (2017). *Time Series Analysis and Its Applications: With R Examples* (4a ed.). Springer.
- Wei, W. W. S. (2006). *Time Series Analysis: Univariate and Multivariate Methods* (2a ed.). Addison-Wesley.
