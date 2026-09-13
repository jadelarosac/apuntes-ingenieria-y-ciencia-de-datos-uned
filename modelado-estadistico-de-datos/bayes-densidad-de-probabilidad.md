# Teorema de Bayes para funciones densidad de probabilidad

El teorema de Bayes establece que  para cada $x\in\mathbb R^p$ y cada $N_x$ entorno de dicho $x$:

$$
	P[Y=k|X\in N_x]=\frac{P[Y=k]P[X\in N_x|Y=k]}{P[X\in N_x]}
$$

Por el teorema de la probabilidad total

$$
	P[Y=k|X\in N_x]=\frac{P[Y=k]P[X\in N_x|Y=k]}{\sum P[Y=i]P[X\in N_x|Y=i]}
$$


Definimos $\pi_i = P[Y=i]$ por un lado, y por otro $f_i$ como las funciones densidad de probabilidad de la distribución de $X$ dado $Y$:

$$
	P[X\in N_x|Y=i]=\int_{N_x} f_i(t) \mathrm{d}t
$$

La expresión queda entonces como

$$
	P[Y=k|X\in N_x]=
	\frac{\pi_k \int f_k(t)\mathrm{d}t}{\sum\pi_i \int f_i(t)\mathrm dt}=
	\frac{\pi_k \frac1{|N_x|}\int f_k(t)\mathrm{d}t}
	{\sum\pi_i \frac1{|N_x|}\int f_i(t)\mathrm dt}
$$

El teorema del valor medio para integrales asegura que:

$$
	\lim_{|N_x|\rightarrow0}\frac1{|N_x|}\int_{N_x} f_i(t)\mathrm{d}t = f_i(x)
$$

Por lo tanto podemos definir:

$$
	p_k(x)=\lim_{|N_x|\rightarrow0}P[Y=k|X\in N_x]=\frac{\pi_k f_k(x)}{\sum\pi_i f_i(x)}
$$

donde la suma se hace en las $K$ clases.