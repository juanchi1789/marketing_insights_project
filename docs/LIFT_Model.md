# Modelo LIFT en marketing

Este documento explica el enfoque de "lift" o incremento para medir el impacto causal de una accion de marketing. El objetivo es estimar el efecto incremental sobre un KPI (ventas, conversiones, ingresos) comparando un grupo expuesto contra un grupo de control.

## 1. Concepto basico

El lift mide la diferencia atribuible a la accion de marketing:

\[
 \text{Lift} = \mathbb{E}[Y \mid \text{expuesto}] - \mathbb{E}[Y \mid \text{control}].
\]

Si la asignacion a grupos es aleatoria, esta diferencia se interpreta como efecto causal promedio.

## 2. Diseno experimental (A/B)

En un experimento controlado:

- Grupo test (T): recibe la campaña.
- Grupo control (C): no recibe la campaña.

El lift simple se calcula como:

\[
 \widehat{\text{Lift}} = \bar{Y}_T - \bar{Y}_C.
\]

Y el lift relativo:

\[
 \widehat{\text{Lift\%}} = \frac{\bar{Y}_T - \bar{Y}_C}{\bar{Y}_C}.
\]

## 3. Lift con tasas de conversion

Si el KPI es una tasa (conversion):

\[
 p_T = \frac{c_T}{n_T}, \quad p_C = \frac{c_C}{n_C},
\]

\[
 \widehat{\text{Lift}} = p_T - p_C, \quad \widehat{\text{Lift\%}} = \frac{p_T - p_C}{p_C}.
\]

Para evaluar significancia se usan pruebas de diferencia de proporciones o intervalos de confianza.

## 4. Lift con periodos pre y post (Diff-in-Diff)

Cuando no se puede aleatorizar, se usa Difference-in-Differences (DiD). Se observan grupos test y control en dos periodos: pre y post.

\[
 \widehat{\text{Lift}}_{DiD} = (\bar{Y}_{T,post} - \bar{Y}_{T,pre}) - (\bar{Y}_{C,post} - \bar{Y}_{C,pre}).
\]

Este enfoque controla por tendencias comunes si el supuesto de "tendencias paralelas" es razonable.

## 5. Ajuste por covariables (regresion)

Se puede estimar el lift con un modelo de regresion:

\[
 Y_i = \beta_0 + \beta_1 D_i + \mathbf{x}_i^\top \boldsymbol{\gamma} + \varepsilon_i,
\]

- $D_i = 1$ si el individuo estuvo expuesto.
- $\beta_1$ estima el lift promedio ajustado por covariables.

En series temporales:

\[
 y_t = \beta_0 + \beta_1 D_t + \sum_k \gamma_k z_{t,k} + \varepsilon_t.
\]

## 6. KPI comunes

- Ventas incrementales.
- Conversion incremental.
- Ingresos incrementales.
- Leads incrementales.
- Incremental ROAS (iROAS).

## 7. Interpretacion e impacto

- Lift positivo: la campana genera incremento real.
- Lift cercano a cero: efecto marginal bajo o inexistente.
- Lift negativo: posible canibalizacion o sesgo de seleccion.

## 8. Buenas practicas

- Definir claramente el KPI y el horizonte de medicion.
- Asegurar asignacion aleatoria cuando sea posible.
- Verificar balance entre grupos (edad, region, historial).
- Medir suficiente tiempo para capturar efectos rezagados.
- Reportar intervalos de confianza, no solo promedio.

## 9. Limitaciones

- Sesgo de seleccion si no hay aleatorizacion.
- Contaminacion entre grupos (spillover).
- Efectos de largo plazo no capturados en ventanas cortas.
- Dependencia del periodo base y estacionalidad.

## 10. Ejemplo numerico

Suponga una campana con:

- Test: $n_T = 10{,}000$, conversiones $c_T = 700$.
- Control: $n_C = 10{,}000$, conversiones $c_C = 600$.

\[
 p_T = 0.07, \quad p_C = 0.06.
\]

\[
 \widehat{\text{Lift}} = 0.07 - 0.06 = 0.01.
\]

\[
 \widehat{\text{Lift\%}} = \frac{0.01}{0.06} \approx 16.7\%.
\]

Interpretacion: la campana aumento la conversion en 1 punto porcentual, equivalente a 16.7% relativo.

## 11. Relacion con MMM

El MMM estima contribuciones promedio por canal en el tiempo. El lift mide el impacto incremental en un contexto controlado. Idealmente, ambos se complementan: el lift valida la incrementalidad y el MMM ayuda a planificar presupuestos.
