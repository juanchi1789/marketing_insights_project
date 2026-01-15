# Modelo de Marketing Mix (MMM) desde cero

Este documento introduce el modelo estadistico de Marketing Mix desde fundamentos matematicos y en notacion LaTeX, con ejemplos sencillos para construir intuicion.

## 0. Motivacion y alcance

El MMM busca cuantificar como distintas palancas (medios, precio, promociones, distribucion) impactan un KPI (ventas, ingresos, leads). Se usa para:

- Medir contribuciones por canal.
- Estimar elasticidades y ROI.
- Simular escenarios de presupuesto.

No es un modelo causal perfecto por si solo: depende de la calidad de los datos, del diseno temporal y de supuestos econometricos.

## 1. Planteamiento del problema

Sea $t = 1, \dots, T$ el indice temporal. Observamos una serie de resultados de negocio (por ejemplo, ventas o ingresos) $y_t \in \mathbb{R}$ y un conjunto de variables explicativas asociadas a acciones de marketing y control. El objetivo es estimar el efecto causal promedio de dichas acciones sobre $y_t$ y construir un modelo para prediccion y descomposicion de contribuciones.

Definimos un vector de covariables $\mathbf{x}_t \in \mathbb{R}^p$ que incluye:

- Variables de marketing (inversion en medios, promociones, precios).
- Variables de control (estacionalidad, tendencia, clima, eventos).

El enfoque MMM clasico es un modelo de regresion lineal con transformaciones que capturan efectos no lineales (adstock, saturacion) en los canales de marketing.

## 1.1 Preparacion de datos

Antes del modelado, es critico:

- Unificar granularidad temporal (semanal o mensual).
- Ajustar por inflacion o cambios de moneda si aplica.
- Tratar outliers y faltantes de manera consistente.
- Alinear fechas de campanas y ventas (lag de reportes).

Una mala alineacion temporal suele ser la principal causa de coeficientes poco interpretable.

## 2. Modelo lineal base

El modelo base es

\[
 y_t = \beta_0 + \sum_{j=1}^p \beta_j x_{t,j} + \varepsilon_t,
\]

con $\varepsilon_t \sim \mathcal{N}(0, \sigma^2)$ i.i.d. o con estructura temporal especifica.

En forma matricial, si $\mathbf{y} \in \mathbb{R}^T$, $\mathbf{X} \in \mathbb{R}^{T \times p}$, entonces

\[
 \mathbf{y} = \beta_0 \mathbf{1} + \mathbf{X}\boldsymbol{\beta} + \boldsymbol{\varepsilon}, \quad \boldsymbol{\varepsilon} \sim \mathcal{N}(\mathbf{0}, \sigma^2 \mathbf{I}).
\]

La estimacion por MCO se obtiene como

\[
 \hat{\boldsymbol{\beta}} = (\mathbf{X}^\top \mathbf{X})^{-1}\mathbf{X}^\top \mathbf{y},
\]

asumiendo rango completo.

## 3. Transformaciones de marketing

### 3.1 Adstock (carryover)

La respuesta a la inversion no suele ser inmediata. Se modela el efecto rezagado con una transformacion adstock para cada canal $j$:

\[
 s_{t,j} = x_{t,j} + \lambda_j s_{t-1,j}, \quad 0 \le \lambda_j < 1,
\]

con $s_{0,j} = 0$. En forma cerrada:

\[
 s_{t,j} = \sum_{k=0}^{t-1} \lambda_j^k x_{t-k,j}.
\]

### 3.2 Saturacion (rendimientos decrecientes)

Se modela con una funcion concava, por ejemplo una transformacion tipo Hill:

\[
 f_j(s_{t,j}) = \frac{s_{t,j}^{\alpha_j}}{s_{t,j}^{\alpha_j} + \theta_j^{\alpha_j}}, \quad \alpha_j, \theta_j > 0.
\]

Esta funcion es creciente y acotada en $(0,1)$, capturando rendimientos decrecientes.

### 3.3 Otras opciones de saturacion

Algunas implementaciones usan:

- Logaritmica: $f(s) = \log(1 + s)$.
- Michaelis-Menten: $f(s) = \frac{s}{s + \theta}$.
- Softplus: $f(s) = \log(1 + e^{s})$ (con escalamiento).

La eleccion depende del tipo de canal y del rango de inversion.

## 4. Modelo MMM con transformaciones

El modelo completo es

\[
 y_t = \beta_0 + \sum_{j=1}^{m} \beta_j f_j(s_{t,j}) + \sum_{k=1}^{q} \gamma_k z_{t,k} + \varepsilon_t,
\]

con:

- $m$ canales de marketing,
- $\mathbf{z}_t \in \mathbb{R}^q$ variables de control,
- $f_j(\cdot)$ transformacion saturante del adstock $s_{t,j}$.

En notacion compacta:

\[
 y_t = \beta_0 + \sum_{j=1}^{m} \beta_j g_j(x_{1:t,j}; \lambda_j, \alpha_j, \theta_j) + \sum_{k=1}^{q} \gamma_k z_{t,k} + \varepsilon_t,
\]

con $g_j$ la composicion adstock + saturacion.

## 4.1 Tendencia, estacionalidad y calendario

Suele incorporarse una tendencia $T_t$ y componentes de calendario:

\[
 y_t = \beta_0 + \sum_j \beta_j g_j(\cdot) + \sum_k \gamma_k z_{t,k} + \delta T_t + \sum_h \eta_h D_{t,h} + \varepsilon_t,
\]

donde $D_{t,h}$ son dummies de calendario (festivos, eventos, lanzamientos). La estacionalidad puede modelarse con dummies mensuales o funciones trigonometricas:

\[
 \text{Season}_t = \sum_{r=1}^{R} \left(a_r \sin\left(\frac{2\pi r t}{S}\right) + b_r \cos\left(\frac{2\pi r t}{S}\right)\right).
\]

## 5. Inferencia y estimacion

Los parametros del modelo son

\[
 \Theta = \{\beta_0, \boldsymbol{\beta}, \boldsymbol{\gamma}, \boldsymbol{\lambda}, \boldsymbol{\alpha}, \boldsymbol{\theta}, \sigma^2\}.
\]

La estimacion puede hacerse por maximizacion de la log-verosimilitud:

\[
 \ell(\Theta) = -\frac{T}{2}\log(2\pi\sigma^2) - \frac{1}{2\sigma^2}\sum_{t=1}^T (y_t - \hat{y}_t)^2.
\]

Debido a la no linealidad en $\lambda_j$, $\alpha_j$, $\theta_j$, suele usarse optimizacion numerica (por ejemplo, gradiente o metodos cuasi-Newton) o un enfoque Bayesiano con MCMC.

## 5.1 Regularizacion

Cuando hay muchos canales o alta colinealidad, se usa regularizacion:

- Ridge: penaliza $\|\boldsymbol{\beta}\|_2^2$.
- Lasso: penaliza $\|\boldsymbol{\beta}\|_1$.
- Elastic Net: combinacion de ambas.

Esto mejora estabilidad y reduce varianza, a costa de sesgo.

## 6. Interpretacion de coeficientes

El efecto marginal de un canal $j$ sobre $y_t$ es

\[
 \frac{\partial y_t}{\partial x_{t,j}} = \beta_j \cdot f_j'(s_{t,j}) \cdot \frac{\partial s_{t,j}}{\partial x_{t,j}}.
\]

Dado el adstock,

\[
 \frac{\partial s_{t,j}}{\partial x_{t,j}} = 1,
\]

por lo que

\[
 \frac{\partial y_t}{\partial x_{t,j}} = \beta_j f_j'(s_{t,j}).
\]

Con $f_j$ tipo Hill,

\[
 f_j'(s) = \frac{\alpha_j \theta_j^{\alpha_j} s^{\alpha_j - 1}}{(s^{\alpha_j} + \theta_j^{\alpha_j})^2}.
\]

## 6.1 ROI y elasticidades

El ROI incremental aproximado para un periodo se puede definir como:

\[
 \text{ROI}_j = \frac{\Delta y_t}{\Delta x_{t,j}}.
\]

En modelos con logaritmos (log-log), el coeficiente es una elasticidad:

\[
 \log(y_t) = \beta_0 + \beta_j \log(x_{t,j}) + \cdots,
\]

entonces $\beta_j$ mide el cambio porcentual en $y_t$ ante un cambio porcentual en $x_{t,j}$.

## 7. Descomposicion de contribuciones

Una aproximacion comun asigna la contribucion del canal $j$ en $t$ como

\[
 c_{t,j} = \beta_j f_j(s_{t,j}).
\]

Entonces

\[
 y_t \approx \beta_0 + \sum_{j=1}^{m} c_{t,j} + \sum_{k=1}^{q} \gamma_k z_{t,k}.
\]

Esto permite reportar el aporte relativo de cada canal en un periodo.

## 7.1 Baseline y lift

Se define el baseline como el valor esperado sin marketing:

\[
 \hat{y}_t^{\text{base}} = \beta_0 + \sum_k \gamma_k z_{t,k}.
\]

El lift de marketing es:

\[
 \hat{y}_t^{\text{lift}} = \sum_j \beta_j f_j(s_{t,j}).
\]

Esto ayuda a separar crecimiento organico vs. incremental.

## 8. Limitaciones y supuestos

- Linealidad en parametros $\beta_j$ y $\gamma_k$.
- Errores independientes y homocedasticos (si no se modela autocorrelacion).
- Posible colinealidad entre canales.
- Identificacion limitada cuando hay pocas variaciones o alta correlacion.

## 8.1 Diagnosticos recomendados

- Autocorrelacion de residuos (ACF, Durbin-Watson).
- Colinealidad (VIF).
- Estabilidad temporal (train/test o validacion por bloques).
- Sensibilidad a supuestos de adstock y saturacion.

## 9. Extensiones

- Modelos con errores ARIMA:
\[
 \varepsilon_t = \phi_1 \varepsilon_{t-1} + \cdots + \phi_p \varepsilon_{t-p} + u_t.
\]

- MMM Bayesiano con priors informativos en $\beta_j$, $\lambda_j$, $\alpha_j$.
- Modelos jerarquicos para multiples regiones o productos.

## 9.1 MMM Bayesiano (intuicion)

En un enfoque Bayesiano se fija un prior para cada parametro, por ejemplo:

\[
 \beta_j \sim \mathcal{N}(0, \tau^2), \quad \lambda_j \sim \text{Beta}(a,b).
\]

Se obtiene una distribucion posterior que permite intervalos creibles y propagacion de incertidumbre en los reportes.

## 10. Conclusion

El MMM es un modelo estadistico estructurado que combina regresion con transformaciones que capturan memoria y saturacion de las inversiones en marketing. Su formulacion matematica permite estimar impactos, realizar descomposiciones y construir escenarios de optimizacion.

## 10.1 Flujo practico de trabajo

1. Definir KPI, horizonte temporal y granularidad.
2. Recolectar datos de medios, precio, promos, eventos.
3. Limpiar, alinear y transformar variables.
4. Ajustar modelos base y con transformaciones.
5. Validar, revisar diagnosticos y sensibilidad.
6. Generar escenarios de inversion y reportes.

## 11. Ejemplos sencillos

### 11.1 Regresion lineal sin transformaciones

Supongamos un unico canal de marketing con gasto $x_t$ y ventas $y_t$.

Modelo:

\[
 y_t = \beta_0 + \beta_1 x_t + \varepsilon_t.
\]

Si $\beta_0 = 100$ y $\beta_1 = 2$, entonces un gasto de $x_t = 10$ implica

\[
 \hat{y}_t = 100 + 2 \cdot 10 = 120.
\]

Interpretacion: cada unidad adicional de gasto aumenta ventas en aproximadamente 2 unidades, manteniendo todo lo demas constante.

### 11.2 Adstock con un solo canal

Sea $\lambda = 0.5$ y gastos $(x_1, x_2, x_3) = (10, 0, 0)$. El adstock se calcula:

\[
 s_1 = 10,
\]
\[
 s_2 = x_2 + \lambda s_1 = 0 + 0.5 \cdot 10 = 5,
\]
\[
 s_3 = x_3 + \lambda s_2 = 0 + 0.5 \cdot 5 = 2.5.
\]

Interpretacion: aunque el gasto ocurre solo en $t=1$, su efecto persiste y decae en el tiempo.

### 11.3 Saturacion tipo Hill

Supongamos $\alpha = 1$ y $\theta = 20$. Entonces

\[
 f(s) = \frac{s}{s + 20}.
\]

Para $s=5$:

\[
 f(5) = \frac{5}{25} = 0.2.
\]

Para $s=20$:

\[
 f(20) = \frac{20}{40} = 0.5.
\]

Para $s=80$:

\[
 f(80) = \frac{80}{100} = 0.8.
\]

Interpretacion: los incrementos en $s$ generan aumentos cada vez menores en $f(s)$.

### 11.4 Descomposicion simple con dos canales

Sea un modelo sin transformaciones:

\[
 y_t = 50 + 1.5 x_{t,1} + 0.5 x_{t,2}.
\]

Si en un periodo $x_{t,1}=20$ y $x_{t,2}=40$, entonces

\[
 \hat{y}_t = 50 + 1.5\cdot 20 + 0.5\cdot 40 = 50 + 30 + 20 = 100.
\]

Contribuciones:

\[
 c_{t,1} = 1.5\cdot 20 = 30, \quad c_{t,2} = 0.5\cdot 40 = 20.
\]

Interpretacion: el canal 1 explica 30 unidades y el canal 2 explica 20 unidades en ese periodo.

## 12. Ejemplo simple de escenario

Suponga un canal con modelo:

\[
 y_t = 100 + 3 f(s_t),
\]

con $f(s) = \frac{s}{s+20}$ y $s$ el adstock actual. Si el gasto aumenta y lleva $s$ de 20 a 40:

\[
 f(20) = 0.5, \quad f(40) = \frac{40}{60} \approx 0.667.
\]

El incremento esperado es:

\[
 \Delta y_t = 3(0.667 - 0.5) \approx 0.5.
\]

Esto ilustra que incluso duplicando $s$, el incremento marginal puede ser limitado por saturacion.
