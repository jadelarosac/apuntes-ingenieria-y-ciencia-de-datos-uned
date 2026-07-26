# Modelo lineal

Si $Y$ se puede aproximar bien por una función lineal, como

$$
    Y \approx\beta_0+\beta_1 X_1 +\ldots+ \beta_{p} X_{p} = \beta_0+\overrightarrow{\beta}\cdot\overrightarrow{X}
$$

Se supone $p<<n$

Si estimamos los parámetros la predicción para el output dado el valor de un input $x$:

$$
    \hat{y} = \hat\beta_0+\overrightarrow{\hat\beta} \cdot\overrightarrow x
$$

donde $\hat\beta_i$ es la aproximación del parámetro $\beta_i$.

El método más habitual es que la aproximación se haga en $L_2$.

## Precisión de la exactitud de los coeficientes

¿Como de preciso es la media muestral como estimador de la media poblacional? En el caso de regresión lineal simple ($p=1$):

$$
    \sqrt{Var(\hat\mu)} = \frac\sigma{\sqrt{n}}
$$

a este valor se le llama error estándar (SE) y puede calcularse para los estimadores del modelo lineal ($p=1$):

$$
    SE(\beta_0)^2 = \frac{\sigma^2}n + \frac{\sigma^2\bar x^2}{\sum (x_i-\bar x) ^2}
$$

$$
    SE(\beta_1)^2 = \frac{\sigma^2}{\sum (x_{i}-\bar x) ^2}
$$


donde $\sigma^2=Var(\varepsilon)$. Se supone que los errores para cada observación tienen varianza común y no están correlados.

$\sigma$, el llamado error residual estándar, puede estimarse con:

$$
    \sqrt\frac{RSS}{n-p-1}=\sqrt\frac1{n-p-1}\sum(y_i-\hat y_i)^2
$$

donde $RSS$ es el error suma se cuadrados.

Los intervalos de confianza al 95% para un parámetro se calculan como ($p=1$, $n$ grande para aproximar $t$ con una normal y esta por el número 2):

$$
    \hat\beta_i\pm 2SE(\hat\beta_i)
$$

por la dualidad entre intervalos de confianza y test de hipótesis, el valor

$$
    t = \frac{\hat\beta_i}{SE(\beta_i)}
$$

sigue la distribución T de Student con $n-2$ grados de libertad ($p=1$). El $p$-valor se calcula como la probabilidad de observar un valor de $t$ igual o mayor al observado cuando $\beta_i=0$. Es decir, cuando el $p$ valor supera un umbral establecido, podemos afirmar que según los valores observados $x_{i,j}$ no se puede afirmar que haya relación entre $X_i$ e $Y$.

## Precisión del modelo

Tenemos el error residual estándar:

$$
    RSE=\sqrt{\frac1{n-p-1}RSS}=\sqrt{\frac1{n-p-1}\sum(y_i-\hat y_i)^2}
$$

Nos dá un error cuantitativo y en las mismas dimensiones que la estimación.

Para comparaciones proporcionales tenemos $R^2$:

$$
    R^2=\frac{TSS-RSS}{TSS}=1-\frac{RSS}{TSS}
$$

donde $TSS = \sum(y_i-\bar y)^2$, la suma total de los cuadrados, mide la varianza total de la respuesta. RSS mide la varianza que el modelo no puede explicar. Su diferencia es la varianza que el modelo explica y se divide entre la varianza total para normalizar.

Hay otro modo de entender $R^2$ en el caso $p=1$ y es como el cuadrado de la correlación:

$$
    Cor(X,Y)=\frac{\sum(x_i-\bar x)(y_i-\bar y)}{\sqrt{\sum (x_i-\bar x)^2}\sqrt{\sum (y_i-\bar y)^2}}
$$

En el caso ($p>1$), tenemos también el $F$-estadístico:

$$
    F=\frac{(TSS-RSS)/p}{RSS/(n-p-1)}
$$

Si el modelo lineal asunciones correctas:

$$
    E[RSS/(n-p-1)]=\sigma^2
$$

y bajo la hipótesis nula de que $\overrightarrow\beta =0$ se tiene

$$
    E[(TSS-RSS)/p =\sigma^2]
$$

Por lo tanto los valores de F cercanos a 1 sugieren que todos los parámetros son cero, mientras que valores mayores, lo contrario. $F$ sigue una distribución $F$ de Snedecor de parámetros $p$ y $n-p-1$, es decir, $F(p, n-p-1)$.

Es necesario estimar todos los parámetros a la vez, no solo uno por uno, porque, si $p$ es grande, por pura suerte es posible (más probable cuanto mayor sea $p$) que alguno de los $p$-valores de algun coeficiente sea menor que el umbral (y daríamos equívocamente el modelo como válido).
