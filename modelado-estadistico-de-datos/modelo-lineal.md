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

En el caso general se tiene:

$$
    SE(\beta_i)^2 = \frac{\sigma^2}{\sum (x_{i}-\bar x) ^2}VIF(\beta_i)
$$

donde el VIF se explicará en la sección de multicolinearidad.

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

## Clasificación

Se puede hacer clasificación binaria sobre una variable $X_i$ haciendo que tome valores en $\{0,1\}$.

Se puede interpretar como dos rectas de regresión diferentes (aunque el ajuste se hace simultáneamente). Un problema es que entonces ambas rectas son paralelas.

Para clasificación $n$-aria, es mejor añadir $n$ variables que una variable que tome más de dos valores.

## Extensiones del modelo lineal

#### Añadir parámetros de interacción

Veamos el caso para $p=2$, generaliza de forma sencilla. Sea:

$$
    Y = \beta_0+\tilde\beta_1 X_1+\beta_2 X_2
$$

el modelo original. Imaginemos que los valores de las observaciones sugieren que los valores observados en $X_1$ dependen fuertemente de los valores que tome $X_2$. En ese caso podemos suponer que $\tilde\beta_1$ es una función de $X_2$. Supongámosla lineal. En ese caso:

$$
    Y = \beta_0+(\beta_1+\beta_3 X_2) X_1+\beta_2 X_2
    = \beta_0+\beta_1 X_1+\beta_2 X_2+\beta_3 X_1 X_2
$$

al término $X_1X_2$ se le denomina interacción o sinergia entre $X_1$ y $X_2$.

Como caso particular, en el caso de variables de clasificación binaria, los parámetros de interacción permiten tener rectas con distinta pendiente para cada variable.

La significancia de un parámetro de interacción implica la de las variables de efectos principales, independientemente de su $p$-valor.

### Relaciones no lineales

Se puede hacer un análisis del tipo

$$
    Y = \beta_0+\beta_1f_1(X_1)+\cdots+\beta_pf_p(X_p)
$$

donde $f_i$ son funciones potencia natural positiva (regresión polinómica) u de otro tipo ($f_i(x)=1/x$ para algún $i$, y la identidad para el resto). Sigue siendo un modelo lineal. ¡Cuidado con el overfitting!

## Problemas del modelo lineal

### Falta de linearidad en los datos

Se puede detectar con gráficas (plots) residuales, $e_i=y_i-\hat y_i$ contra $x_i$. Si los datos muestran un patrón claro, podemos estar enfrentándonos a este problema.

Una forma de paliarlo es hacer el ajuste con una transformación no lineal de la variable ($X^2$, $\sqrt X$, $\log X$...).

### Correlación entre errores

Los $e_i$ deben estar icorrelados. Si no, estamos subestimando los $p$-valores (peligrosísimo) o equivalentemente, obteniendo intervalos de confianza mucho más estrechos de los que deberíamos.

Una pista puede ser que el gráfico residual tiene más pinta de cuerda que de serrucho.

Es un caso muy común en el contexto de las series temporales, aunque no solo en este caso. Por ejemplo, un modelo de predicción de altura según el peso puede fallar si un 20% de los individuos vienen de la misma familia.

### Varianza no constante (Heteroscedasticidad)

Se supone que $Var[e_i] = \sigma^2$, es decir, que haya homocedasticidad. Pero hay muchos casos en los que no (v.g. el error puede aumentar con el valor de la respuesta).

Una medida para mitigar esto es usar una función cóncava de la variable de respuesta ($\sqrt{Y}$, $\log Y$...).

#### Caso en el que la varianza sea conocida

Si sabemos que la varianza de la observación $y_i$ es $1/w_i$ menor que la de la distribución global, podemos hacer regresión lineal con pesos $w_i$. En ese caso minimizamos

$$
    \sum w_i(y_i-\hat y_i)^2
$$

Un caso típico es que se tiene una variable $Z$ que se quiere predecir con un modelo lineal, pero solo se conocen las observaciones $y_i=\frac1n\sum_j z_{i,j}$ donde $j\in\{1\ldots n_i\}$. Entonces $\sigma_i=\sigma/n_i$, con lo que se elige $w_i=n_i$.

#### Valores atípicos (outliers)

Si tienen baja influencia (low leverage) no suelen ser muy problemáticos en la estimación de los parámetros. Sin embargo, sí afectan al RSE y al $R^2$.

Se pueden detectar con gráficos residuales studentizados, que se consiguen al dividir cada $e_i$ por su error estimado estandar.

Si encontramos un outlier, podemos simplemente excluirlo del análisis. Sin embargo, a veces son un indicio de un mal modelo (como que faltan predictores).

#### Valores con alta influencia (high leverage)

A diferencia de los outliers, que tienen un valor anómalo de $y_i$, estos tienen un valor anómalo de $X$ (no necesariamente de algún $x_i$, puede ser la combinación).

El problema es que tales valores tienen un efecto muy grande en los valores de la regresión, haciendo el modelo muy sensible a errores en estos valores.

Para determinar estos valores se usa el estadístico de palanca (leverage statistic), que para la regresión lineal simple se calcula:

$$
    h_j = \frac1n+\frac{(x_j-\bar x)^2}{\sum(x_j-\bar x)^2}
$$

Toma valores entre $1/n$ y 1, siendo la media $(p+1)/n$. Cuanto más alejado de este valor y más cerca de 1, mayor su influencia.

Se puede hacer un gráfico de los residuos estudentizados contra $h_j$ para detectar valores que sean a la vez valores atípicos y con gran influencia (¡la peor combinación!).

### Colinearidad

Cuando dos o más variables están estrechamente relacionadas entre sí. Se puede ver con las curvas de nivel, fijado un valor de error cuadrado, que el mínimo es casi "una curva de mínimos" (no es posible por la convexidad de los cuadrados, pero casi, existe una región donde hay pequeñas variaciones de error, y en otras direcciones cambios muy bruscos, como un valle). Esto crea inestabilidad en el modelo.

El error estándar asociado a los coeficientes de la regresión crece, reduciendo su potencia y reduciendo la probabilidad de detectar un coeficiente mayor que cero.

La forma de detectarla cuando solo se da entre dos variables es mirando la matriz de correlación de los predictores. Elementos grandes (en valor absoluto) indican este fenómeno.

#### Multicolinearidad

Se calcula el VIF, el factor de inflacción de varianza. Se calcula:

$$
    VIF(\hat\beta_j)=\frac1{1-R^2_{X_j|X_{\bar j}}}
$$

donde $R^2_{X_j|X_{\bar j}}$ es el $R^2$ resultado del ajuste de $X_j$ sobre el resto de predictores. Cuando es grande, cercano a uno, indica colinearidad y el VIF será grande (se suelen tomar valores de 5 o 10 como umbral a partir del cual se determina que hay colinealidad).

Se puede eliminar una de las variables involucradas en la colinearidad o combinarlas en un único predictor.
