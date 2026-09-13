# Análisis del discriminante lineal

El objetivo es la clasificación de un evento en una de $K>0$ clases diferentes.

## Preludio

Tal y como se establece en [[bayes-densidad-de-probabilidad]], el teorema de Bayes establece que:

$$
	p_k(x)=\frac{\pi_k f_k(x)}{\sum\pi_i f_i(x)}
$$

Estimar $\pi_k$ suele ser sencillo, se toma la fracción de las observaciones que pertenecen a la clase $k$ respecto del total.

## Modelo

Si

$$
	f_k(x) = \frac1{\sqrt{\det(2\pi\Sigma)}}
	\exp\left(-\frac12(x-\mu_k)^T\Sigma^{-1}(x-\mu_k)\right)
$$

es decir, $\overrightarrow{X}=(X_1..X_p)$  y cada $X_i$ sigue una distribución normal (pero todos con la misma matriz de varianzas) $X_k\sim\mathcal{N}(\mu_k,\Sigma)$ entonces el discriminante

$$
	\delta_k(x)=x^T\Sigma^{-1}\mu_k-\frac12\mu_k\Sigma^{-1}\mu_k+\log\pi_k
$$

es máximo si y solo si $p_k$ es máximo. Deduzcámoslo. Sabemos por lo visto en [[discriminante-cuadratico]] que es equivalente a minimizar la cantidad:

$$
	-\frac12 x^T\Sigma^{-1} x
	+x^T\Sigma^{-1}\mu
	-\frac12\mu^T\Sigma^{-1}\mu
	+\log\pi_k = \delta_k(x)-\frac12 x^T\Sigma^{-1} x
$$

ya que el término cuadrático $\frac12 x^T\Sigma^{-1} x$ es constante respecto a $k$ (es decir, es independiente de la clase porque todos los $X_i$ comparten matriz de covarianzas), se tiene que maximizar $f_k$ y $\delta_k$ es equivalente. Es decir, si para cada clase $k$ definimos $g_x(k)=\delta_k(x)$ y $h_x(k)=p_k(x)$

$$
	\arg\max g_x(k)=\arg\max h_x(k)
$$

## Estimación de los parámetros

## Frontera de decisión

Cuando $K>2$ puede ocurrir que dos clases dadas $k$ y $\ell$ no sean adyacentes. En estos casos, $\forall x\in\mathbb R$ se tiene que $g_x(k)=g_x(\ell)=L\implies\exists t: g_x(t)>L$. Es decir, cualquier punto en el que $x$ no podría distinguir entre la clase $k$ y la $\ell$, $x$ pertenece a una tercera clase $t$.

En el caso contrario, es decir, que exista al menos un $x$ tal que $\arg\max g_x(t)=g_x(k)=g_x(\ell)=\delta_k(x)=\delta_\ell(x)$, tenemos que todos estos puntos pertenecen al mismo hiperplano, el cuál viene dado por:

$$
	x^T\left(\Sigma^{-1}(\mu_k-\mu_\ell)\right)
	=\frac12\left(
		\mu_k^T\Sigma^{-1}\mu_k -
		\mu_\ell^T\Sigma^{-1}\mu_\ell
	\right)
	+\log\frac{\pi_k}{\pi_\ell}
$$

que corresponde a un hiperplano representado con su ecuación implícita. Es decir  $x^Tb =\sum x_ib_i=c$ con $b\in\mathbb R^p$ y $c\in \mathbb R$ fijos. El nombre de discriminante lineal viene entonces justificado por esta ecuación lineal en los $x_i$.

## Curvas de nivel

Las curvas de nivel vuelven a ser hiperelipsoides, como en el caso de [[discriminante-cuadratico#Curvas de nivel]], solo que todos los ellos tienen la misma excentricidad y los ejes alineados. Lo único que cambian son los centros de cada uno de ellos, $\mu_k$. 

## Reducción de la dimensión y representación

Siguiendo el análisis de [[distancia-Mahalanobis]], podemos hacer la siguiente observación:

$$
	\delta_k(x) = -\frac12D_\Sigma(x,\mu_k)^2+\log\pi_k
$$

entonces

$$
	\arg\max g_x(k) = 
	\arg\max\left(
		-\frac12D_\Sigma(x,\mu_k)^2+\log\pi_k
	\right) = 
	\arg\min\left(
		D_\Sigma(x,\mu_k)^2-2\log\pi_k
	\right)
$$

donde al multiplicar por un número negativo como $-2$ cambiamos la búsqueda de un máximo por un mínimo.

En el caso en el que los $\pi_i$ sean todos iguales (es decir todas las clases tengan las mismas observaciones) el modelo es equivalente $D_\Sigma(x,\mu_k)^2$, lo que corresponde al [[NCC]] (clasificador del centroide más cercano).
