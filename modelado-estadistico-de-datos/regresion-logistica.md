# Regresión Logística

El modelo lineal asume que la variable de respuesta es cuantitativa. La regresión logística permite modelar respuestas cualitativas.

## Modelo

Supongamos que tenemos $X_k$ con $k\in\{1..K\}$ distribuidas como variable multinomial:

$$
f(Y|X) = \prod_{k=1}^K p_k^{I(Y=k)}
= \prod_{k=1}^{K-1} p_k^{I(Y=k)}
\left(1-\sum_{j=1}^{K-1} p_j\right)^{1-\sum I(Y=k)}
$$

con $p_k=P[Y=k|X]$ para $k\in\{1..K\}$. Se usa que la probabilidad siempre suma 1 y que la en una multinomial siempre se da que $I(Y=k)=0$ para todo $k$ salvo para un cierto $k'$ que $I(Y=k')=1$.

Se trata de una familia exponencial que se puede expresar como:

$$
f(y|X)
= \exp\sum_{k=1}^K I(y=k)\log p_k
= \exp\left(
    \sum_{k=1}^{K-1} I(y=k)\log p_k +
    \left(1-\sum_{k=1}^{K-1} I(y=k)\right)
    \log\left(1-\sum_{j=1}^{K-1} p_j\right)
    \right)
$$

es decir:

$$
f(y|X)
= \exp\left(
    \sum_{k=1}^{K-1} I(y=k)\log \frac{p_k}{1-\sum_{j=1}^{K-1} p_j} +
    \log\left(1-\sum_{k=1}^{K-1} p_k\right)
    \right)
$$

es decir, la distribución pertenece a una familia exponencial. Siguiendo el modelo lineal generalizado ([[modelo-lineal#Modelo lineal generalizado]]) (no confundir con el modelo lineal general), que siempre es aplicable en el caso de la familia exponencial, tenemos la regresión lineal:

$$
\log \frac{p_k}{1-\sum_{j=1}^{K-1} p_j}
= \beta_{k,0}+\sum_{i=0}^{t}\beta_{k,i} X_i
$$

donde $t$ sustituye a la $p$ del capítulo anterior para evitar confusiones con $p_k$.

Con lo que:

$$
\frac{p_k}{1-\sum_{j=1}^{K-1} p_j}
= \exp\left(\beta_{k,0}+\sum_{i=1}^{t}\beta_{k,i} X_i\right)
$$

$$
p_k=
\left(1-\sum_{j=1}^{K-1} p_j\right)
\exp\left(\beta_{k,0}+\sum_{i=1}^{t}\beta_{k,i} X_i\right)
$$

Sumando sobre $k\in\{1..K-1\}$:

$$
\sum_{k=1}^{K-1} p_k=
\left(1-\sum_{j=1}^{K-1} p_j\right)
\sum_{k=1}^{K-1}\exp\left(\beta_{k,0}+\sum_{i=1}^{t}\beta_{k,i} X_i\right)
$$

Por otro lado, tenemos que:

$$
\left(1-\sum_{j=1}^{K-1} p_j\right)
+\sum_{j=1}^{K-1} p_j= 1
$$

Luego aplicando la ecuación anterior, se obtiene:

$$
\left(1-\sum_{j=1}^{K-1} p_j\right)
+
\left(1-\sum_{j=1}^{K-1} p_j\right)
\sum_{k=1}^{K-1}\exp\left(\beta_{k,0}+\sum_{i=1}^{t}\beta_{k,i} X_i\right)=
\left(1-\sum_{j=1}^{K-1} p_j\right)
\left(1+\sum_{k=1}^{K-1}\exp\left(\beta_{k,0}+\sum_{i=1}^{t}\beta_{k,i} X_i\right)\right)= 1
$$

y finalmente (intercambiando $j$ y $k$):

$$
p_K =1-\sum_{k=1}^{K-1} p_k= \frac{1}{1+\sum_{j=1}^{K-1}\exp\left(\beta_{j,0}+\sum_{i=1}^{t}\beta_{j,i} X_i\right)}
$$

con lo que para $k\in\{1..K-1\}$:

$$
p_k=
\frac{\exp\left(\beta_{k,0}+\sum_{i=1}^{t}\beta_{k,i} X_i\right)}{1+\sum_{j=1}^{K-1}\exp\left(\beta_{j,0}+\sum_{i=1}^{t}\beta_{j,i} X_i\right)}
$$

Definimos $\overrightarrow{x}=\left(1,x_1,\ldots,x_{t}\right)$ y
$\overrightarrow{\beta}_k=(\beta_{k,0},\beta_{k,1},\ldots,\beta_{k,t})$ y tenemos para $k\in\{1..K-1\}$:

$$
p_k=
\frac{\exp\overrightarrow{\beta_k}\cdot\overrightarrow{x}}{1+\sum_{j=1}^{K-1}\exp\overrightarrow{\beta_j}\cdot\overrightarrow{x}}
$$

$$
p_K=
\frac1{1+\sum_{j=1}^{K-1}\exp\overrightarrow{\beta_j}\cdot\overrightarrow{x}}
$$

Finalmente, para expresar las ecuaciones de manera simétrica se puede definir $\overrightarrow{\beta}_K=\overrightarrow{0}$ para expresar $1=\exp\overrightarrow{\beta_K}\cdot\overrightarrow{x}$ y por consiguiente $1+\sum_{k=1}^{K-1}\exp\overrightarrow{\beta_k}\cdot\overrightarrow{x}=\sum_{k=1}^{K}\exp\overrightarrow{\beta_k}\cdot\overrightarrow{x}$:

$$
p_k=
\frac{\exp\overrightarrow{\beta_k}\cdot\overrightarrow{x}}{\sum_{j=1}^{K}\exp\overrightarrow{\beta_j}\cdot\overrightarrow{x}}
$$

con $k\in\{1..K\}$.

También puede ajustarse directamente una expresión como la anterior (y dejar el problema como subdeterminado).

### Estimación de los parámetros

La estimación se hace maximizando la función de verosimilitud:

$$
\ell(\overrightarrow{\beta}_1,\ldots,\overrightarrow{\beta}_{K-1})
= \prod_{k=1}^{K-1} p_k^{I(Y=k)}
\left(1-\sum_{j=1}^{K-1} p_j\right)^{1-\sum I(Y=k)}
$$

donde $p_k$ son los anteriores pero vistos como funciones de los $\overrightarrow{\beta}_k$ y $\overrightarrow{\beta}_K=\overrightarrow{0}$.

Normalmente ha de resolverse con métodos numéricos.

## Interpretación de los coeficientes

Como

$$
\log\frac{p_k}{p_K}=\exp\overrightarrow{\beta_k}\cdot\overrightarrow{x}
$$

Se puede interpretar que un cambio de una unidad de ${x}_i$ corresponde a $\beta_{k,i}$ unidades de cambio en el logaritmo de la razón de oportunidades (_log-odds_) de $Y=k$ respecto a la categoría de referencia (_baseline_) $Y=K$.

[[razon-oportunidades]]
