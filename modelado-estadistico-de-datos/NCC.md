# Clasificador del Centroide más próximo (_Nearest Centroid Classifier_)

Es un método de aprendizaje supervisado. Tiene ciertas similitudes con [[KNN]] .

Para clases $1..K$ se tienen datos $(x_i, y_i)$ con $y_i=k\in\{1..K\}$ e $i\in\{1..n\}$. Se calcula con un conjunto de entrenamiento un estimador para la media de los valores de la clase $k$, el centroide de la clase $k$:

$$
	\mu_k=\frac1{n_k}\sum_{i=1} I(y_i=k)x_i
$$

donde

$$
	n_k =\sum_{i=1}^n I(y_i=k)
$$

Definimos la función

$$
	g_x(k)=D(x,\mu_k)
$$

es la distancia euclídea (aunque también puede extenderse el modelo a otras distancias).

Se asigna a cada nueva observación $x_0$ la clase del centroide más cercano

$$
	\hat{y}_0=\arg\min g_{x_0}(k)
$$