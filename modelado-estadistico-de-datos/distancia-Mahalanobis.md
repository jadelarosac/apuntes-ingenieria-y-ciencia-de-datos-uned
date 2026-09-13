# Distancia de Mahalanobis

Dada $\Sigma$ matriz de covarianzas de una distribución de probabilidad, se define la Mahalanobis entre dos vectores aleatorios como:

$$
	D_{\Sigma}(x,y)=\sqrt{(x-y)^T\Sigma^{-1}(x-y)}
$$

En el caso en el que $\Sigma$ sea definida positiva, $\Sigma^{-1}$ es simétrica y definida positiva, con lo que existen $\Lambda$ diagonal y $V$ ortogonal tal que $\Sigma^{-1}=V\Lambda V^T$.

En dicho caso, se define $\Lambda^{1/2}$ como la matriz diagonal cuyos elementos son las raíces cuadradas de los elementos de $\Lambda$. Se tiene, como sugiere la notación, $(\Lambda^{1/2})^T\Lambda^{1/2}=\Lambda$. Se define $\Sigma^{-1/2}=V\Lambda^{1/2}V^T$, que de nuevo cumple 

$$
	(\Sigma^{-1/2})^T\Sigma^{-1/2}=
	V(\Lambda^{1/2})^TV^TV\Lambda^{1/2}V^T=V\Lambda V^T=\Sigma^{-1}
$$

Se puede definir el cambio de coordenadas $v\mapsto\Sigma^{-1/2} v$ (llamada transformación de ruido blanco) de tal modo que si $x,y$ son dos vectores aleatorios y $u,w$ sus imágenes a través de dicha transformación:

$$
	D_\Sigma(x,y)=\sqrt{(x-y)^T\Sigma^{-1}(x-y)}=
	\sqrt{(x-y)^T(\Sigma^{-1/2})^T\Sigma^{-1/2}(x-y)}
$$

$$
	D_\Sigma(x,y)=
	\sqrt{(\Sigma^{-1/2}(x-y))^T\Sigma^{-1/2}(x-y)}=
	\sqrt{((\Sigma^{-1/2}x-\Sigma^{-1/2}y))^T(\Sigma^{-1/2}x-\Sigma^{-1/2}y)}
$$

$$
	D_\Sigma(x,y)=
	\sqrt{(u-w)^T(u-w)}=
	D(u,w)
$$

es decir, la distancia euclídea en el espacio transformado.

Se puede dotar al espacio de la norma del espacio euclídeo transformado:

$$
	||x||_{D_\Sigma}=||\Sigma^{-1/2}x||\implies D_\Sigma(x,y)=||x-y||_{D_\Sigma}
$$

En el caso en el que $\Sigma$ sea la matriz de covarianzas de una distribución normal $\mathcal N(\mu, \Sigma)$, las coordenadas transformadas se distribuyen como $\mathcal N(\mu, \mathrm I_p)$ donde $I_p$ es la matriz identidad.
