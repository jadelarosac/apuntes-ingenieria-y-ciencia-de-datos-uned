# Análisis del discriminante cuadrático

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
	f_k(x) = \frac1{\sqrt{\det(2\pi\Sigma_k)}}
	\exp\left(-\frac12(x-\mu)^T\Sigma_k^{-1}(x-\mu)\right)
$$

es decir, $\overrightarrow{X}=(X_1..X_p)$  y cada $X_i$ sigue una distribución normal $X_k\sim\mathcal{N}(\mu_k,\Sigma_k)$ entonces el discriminante

$$
	\delta_k(x)=-\frac12 x^T\Sigma^{-1} x +x^T\Sigma^{-1}\mu_k-\frac12\mu_k\Sigma^{-1}\mu_k+\log\pi_k
$$

es máximo si y solo si $p_k$ es máximo. Deduzcámoslo. Vemos que:

$$
	p_k(x)=\frac{\pi_k}{\sum\pi_k f_k(x)}
	\frac1{\sqrt{(2\pi\det(\Sigma_k))^p}}
	\exp\left(-\frac12(x-\mu)^T\Sigma_k^{-1}(x-\mu)\right)
$$

cantidad proporcional a

$$
	\pi_k\exp\left(-\frac12(x-\mu)^T\Sigma_k^{-1}(x-\mu)\right)
$$

Tomando una función creciente como el logaritmo de la expresión anterior no cambian sus máximos o mínimos:

$$
	-\frac12(x-\mu)^T\Sigma^{-1}(x-\mu)+\log\pi_k=
	-\frac12 x^T\Sigma^{-1} x
	+\frac12 x^T\Sigma^{-1}\mu
	+\frac12 \mu^T\Sigma^{-1}x
	-\frac12\mu^T\Sigma^{-1}\mu
	+\log\pi_k
$$

Como $\Sigma_k$ es simétrica y definida positiva (por ser la matriz de covarianzas de una gausiana multidimensional no degenerada) su inversa también es definida estrictamente positiva y $x^T\Sigma_k^{-1} y=y^T\Sigma_k^{-1} x$. Por tanto la expresión anterior es igual a:

$$
	\delta_k(x)=-\frac12 x^T\Sigma^{-1} x
	+x^T\Sigma^{-1}\mu
	-\frac12\mu^T\Sigma^{-1}\mu
	+\log\pi_k 
$$

Se tiene que maximizar $f_k$ y $\delta_k$ es equivalente. Es decir, si para cada clase $k$ definimos $g_x(k)=\delta_k(x)$ y $h_x(k)=p_k(x)$

$$
	\arg\max g_x(k)=\arg\max h_x(k)
$$

## Distancia de Mahalanobis

Se define la Mahalanobis entre dos vectores aleatorios como:

$$
	D_\Sigma(x,y)=\sqrt{}
$$

## Frontera de decisión

## Curvas de nivel




