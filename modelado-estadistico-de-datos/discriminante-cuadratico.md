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
	\exp\left(-\frac12(x-\mu_k)^T\Sigma_k^{-1}(x-\mu_k)\right)
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
	\pi_k\exp\left(-\frac12(x-\mu_k)^T\Sigma_k^{-1}(x-\mu_k)\right)
$$

Tomando una función creciente como el logaritmo de la expresión anterior no cambian sus máximos o mínimos:

$$
	-\frac12(x-\mu_k)^T\Sigma^{-1}(x-\mu_k)+\log\pi_k=
	-\frac12 x^T\Sigma^{-1} x
	+\frac12 x^T\Sigma^{-1}\mu_k
	+\frac12 \mu_k^T\Sigma^{-1}x
	-\frac12\mu_k^T\Sigma^{-1}\mu_k
	+\log\pi_k
$$

Como $\Sigma_k$ es simétrica y definida positiva (por ser la matriz de covarianzas de una gausiana multidimensional no degenerada) su inversa también es definida estrictamente positiva y $x^T\Sigma_k^{-1} y=y^T\Sigma_k^{-1} x$. Por tanto la expresión anterior es igual a:

$$
	\delta_k(x)=-\frac12 x^T\Sigma^{-1} x
	+x^T\Sigma^{-1}\mu_k
	-\frac12\mu_k^T\Sigma^{-1}\mu_k
	+\log\pi_k 
$$

Se tiene que maximizar $f_k$ y $\delta_k$ es equivalente. Es decir, si para cada clase $k$ definimos $g_x(k)=\delta_k(x)$ y $h_x(k)=p_k(x)$

$$
	\arg\max g_x(k)=\arg\max h_x(k)
$$

## Frontera de decisión

Cuando $K>2$ puede ocurrir que dos clases dadas $k$ y $\ell$ no sean adyacentes. En estos casos, $\forall x\in\mathbb R$ se tiene que $g_x(k)=g_x(\ell)=L\implies\exists t: g_x(t)>L$. Es decir, cualquier punto en el que $x$ no podría distinguir entre la clase $k$ y la $\ell$, $x$ pertenece a una tercera clase $t$.

En el caso contrario, es decir, que exista al menos un $x$ tal que $\arg\max g_x(t)=g_x(k)=g_x(\ell)=\delta_k(x)=\delta_\ell(x)$, tenemos que todos estos puntos pertenecen a la misma hipersuperficie, la cuál viene dada por una ecuación matricial. Si se investigan los coeficientes de cada una de las matrices y vectores que aparecen, se llega a una expresión del tipo:

$$
	\sum_{i=0}^p\sum_{j=0}^p a_{i,j}x_i x_j+\sum_{i=0}^p b_i x_i+C=0
$$

es decir, una cuádrica. Así se justifica el nombre del modelo.


## Curvas de nivel

Fijemos una clase $k$. Tomemos todos los $x\in\mathbb R^p: \mathrm f_k(y)=c\in[0,1]$ donde $\mathrm f$ es la función densidad de probabilidad que definimos antes. Consideremos la aplicación $y\mapsto x=y-\mu_k$. Tenemos entonces que $\mathrm f_k(y)=\mathrm f_k(y-\mu_k+\mu_k)=\mathrm f_k(x+\mu_k)= f_k(x)$ donde $f_k$ es una función de densidad de probabilidad para una distribución $\mathcal N(0,\Sigma_k)$.

$$
	f_k(x) = \frac1{\sqrt{\det(2\pi\Sigma_k)}}
	\exp\left(-\frac12x^T\Sigma_k^{-1}x\right)
$$


### Interpretación de las curvas de nivel

Por la definición de $f_k$, buscamos los $z\in\mathbb R^p: D_k(z,0) = r$, para determinados $r$, donde $D_k$ es la distancia de Mahalanobis asociada a $\Sigma_k$ (esto no es más que lo que hemos hecho en el punto anterior).

[[distancia-Mahalanobis]]

Aplicado al caso del análisis del discriminante cuadrático, se cumple que $\Sigma_k$ es simétrica y definida positiva, con lo cuál podemos considerar la transformación $v\mapsto\Sigma_k^{-1/2}v$.

En este espacio, en el que $U\sim N(0,\mathrm I_p)$, se tiene que

$$
	D_k(X,0)^2=D(U,0)^2=\sum_{i=1}^p U_i^2\sim\mathcal \chi_p
$$

Por tanto, podemos interpretar $r$ como el percentil $\alpha$ de la distribución chi cuadrado con $p$ grados de libertad.

### Forma de las curvas de nivel


Veamos qué forma tiene $D_k(z,0) = r$.

$$
	x^T\Sigma_k^{-1}x=x^TV\Lambda_k^{-1}V^Tx=(V^Tx^T)^T\Lambda_k^{-1}V^Tx=r^2
$$

que es cierta por la diagonalización de $\Sigma_k^{-1}=V\Lambda_k^{-1}V^T$. Si consideramos la aplicación lineal $x\mapsto z=V^Tx^T$ tenemos

$$
	z^T\Lambda_k^{-1}z=r^2
$$

Sea $a_{i,j}$ el componente de la matriz $\Lambda_k^{-1}$. Se tiene que $a_{i,j}=0$ para $i\neq j$ y $a_{i,i}=\lambda_i$:

$$
	\sum_{i=0}^p\sum_{j=0}^p  a_{i,j} x_i x_j=
	\sum_{i=0}^p  \lambda_i x_i^2=r^2\iff
	\sum_{i=0}^p  \frac{\lambda_i}{r^2} x_i^2=1
$$

que es la ecuación de un hiperelipsoide en $\mathbb R^p$.

### Elipsoide de confianza

Entonces

$$
	\{x\in\mathbb R^p: D_k(x,0) \le  \chi_{p,\alpha}\}
$$

es el hiperelipsoide sólido que contiene probabilidad $\alpha$ bajo $f_k$ (o $\mathrm f_k$). Se le llama el hiperelipsoide de nivel de confianza $\alpha$.






