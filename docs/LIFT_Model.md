# Modelo LIFT en marketing

Este documento explica el enfoque de "lift" o incremento para medir el impacto causal de una accion de marketing. El objetivo es estimar el efecto incremental sobre un KPI (ventas, conversiones, ingresos) comparando un grupo expuesto contra un grupo de control. La idea es simple: si dos grupos son comparables, cualquier diferencia en el resultado se puede atribuir a la campana.

## 0. Motivacion y usos

El lift se usa para responder preguntas practicas como:

- La campana realmente aumento ventas o solo movio ventas que hubieran ocurrido igual?
- Cuanto incremento genero y en que porcentaje?
- Vale la pena escalar el presupuesto?

Es especialmente util cuando se necesita una medida causal directa, sin depender de supuestos fuertes de modelos.

## 1. Concepto basico

El lift mide la diferencia atribuible a la accion de marketing:

\[
 \text{Lift} = \mathbb{E}[Y \mid \text{expuesto}] - \mathbb{E}[Y \mid \text{control}].
\]

Si la asignacion a grupos es aleatoria, esta diferencia se interpreta como efecto causal promedio. En estadistica, esto es el Average Treatment Effect (ATE).

## 2. Diseno experimental (A/B)

En un experimento controlado:

- Grupo test (T): recibe la campana.
- Grupo control (C): no recibe la campana.

El lift simple se calcula como diferencia de promedios:

\[
 \widehat{\text{Lift}} = \bar{Y}_T - \bar{Y}_C.
\]

Y el lift relativo:

\[
 \widehat{\text{Lift\%}} = \frac{\bar{Y}_T - \bar{Y}_C}{\bar{Y}_C}.
\]

En la practica, tambien se reporta el incremento absoluto y el relativo para dar contexto. El absoluto muestra el cambio en unidades del KPI; el relativo facilita la comparacion entre periodos o segmentos.

## 3. Lift con tasas de conversion

Si el KPI es una tasa (conversion):

\[
 p_T = \frac{c_T}{n_T}, \quad p_C = \frac{c_C}{n_C},
\]

\[
 \widehat{\text{Lift}} = p_T - p_C, \quad \widehat{\text{Lift\%}} = \frac{p_T - p_C}{p_C}.
\]

Para evaluar significancia se usan pruebas de diferencia de proporciones o intervalos de confianza. Esto ayuda a decidir si el lift observado es compatible con ruido aleatorio o si es un efecto real.

## 3.1 ROI e incremental ROAS (iROAS)

Si el KPI es monetario (ingresos), se puede calcular ROI incremental:

\[
 \text{ROI} = \frac{\text{Incremento en ingresos}}{\text{Inversion}}.
\]

En medios digitales es comun reportar iROAS:

\[
 \text{iROAS} = \frac{\Delta \text{Ingresos}}{\Delta \text{Gasto}}.
\]

Un iROAS de 1.5 significa que por cada 1 invertido se generan 1.5 adicionales.

## 4. Lift con periodos pre y post (Diff-in-Diff)

Cuando no se puede aleatorizar, se usa Difference-in-Differences (DiD). Se observan grupos test y control en dos periodos: pre y post.

\[
 \widehat{\text{Lift}}_{DiD} = (\bar{Y}_{T,post} - \bar{Y}_{T,pre}) - (\bar{Y}_{C,post} - \bar{Y}_{C,pre}).
\]

Este enfoque controla por tendencias comunes si el supuesto de "tendencias paralelas" es razonable. Es decir, sin campana, ambos grupos habrian evolucionado de forma similar.

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

Esta formulacion es util cuando hay pequeñas diferencias entre grupos y se quiere ajustar por edad, region, comportamiento pasado u otras variables relevantes.

## 6. KPI comunes

- Ventas incrementales.
- Conversion incremental.
- Ingresos incrementales.
- Leads incrementales.
- Incremental ROAS (iROAS).

## 7. Interpretacion e impacto

- Lift positivo: la campana genera incremento real.
- Lift cercano a cero: efecto marginal bajo o inexistente.
- Lift negativo: posible canibalizacion, cambio en mix o sesgo de seleccion.

## 8. Buenas practicas

- Definir claramente el KPI y el horizonte de medicion.
- Asegurar asignacion aleatoria cuando sea posible.
- Verificar balance entre grupos (edad, region, historial).
- Medir suficiente tiempo para capturar efectos rezagados.
- Reportar intervalos de confianza, no solo promedio.
- Registrar criterios de exclusion para evitar sesgos posteriores.

## 9. Limitaciones

- Sesgo de seleccion si no hay aleatorizacion.
- Contaminacion entre grupos (spillover).
- Efectos de largo plazo no capturados en ventanas cortas.
- Dependencia del periodo base y estacionalidad.
- Resultados poco estables con tamanos de muestra pequenos.

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

Interpretacion: la campana aumento la conversion en 1 punto porcentual, equivalente a 16.7% relativo. Si el ingreso promedio por conversion es 20, el incremento esperado de ingresos es aproximadamente $10{,}000 * 0.01 * 20 = 2{,}000$.

## 11. Relacion con MMM

El MMM estima contribuciones promedio por canal en el tiempo. El lift mide el impacto incremental en un contexto controlado. Idealmente, ambos se complementan: el lift valida la incrementalidad y el MMM ayuda a planificar presupuestos.
