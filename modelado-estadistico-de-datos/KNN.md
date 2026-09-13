# $K$ Vecinos más próximos (_$K$-Nearest Neighbours_)

## Clasificación

Es una técnica en la que se estima, fijado un natural $K$:

$$
  P[Y=j | X=x_0]=\frac1K \sum_{i=0}^n I(y_i=j) I(i \in KNN_{x_0})
$$

donde

$$
	KNN_{y}=\{i \text{ los K índices tales que } x_i \text{ son los } K \text{ más cercanos a } y\}
$$

Es decir, para predecir una clase, mira a los $K$ vecinos más cercanos y promedia.

## Regresión

Se toma el valor para una nueva observación $x_0$ como la media de los $K$ vecinos más próximos:

$$
    \hat f(x_0)
    =\frac1{K}\sum_{i=0}^n y_i I(i\in KNN_{x_0})
    =\frac1{K}\sum_{i=0}^n f(x_i) I(i\in KNN_{x_0}) 
$$

Cuando $K=1$, el modelo interpola los datos (mayor sesgo, menor varianza). A mayor $K$, más suave la interpolación. $K=n$ todos los datos son aproximados por la media (mayor varianza, menor sesgo).

La regresión KNN es muy sensible a las variables de input que no son explicativas (variables de ruido) y al aumentar la dimensión en general.
