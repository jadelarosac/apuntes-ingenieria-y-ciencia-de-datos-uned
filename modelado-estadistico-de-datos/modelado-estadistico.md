# Modelado Estadistico de datos

## Modelo

Consideramos $X$ como inputs, predictores, variables independientes, caracerísticas (_features_) o variables. $Y$ serían entonces outputs, respuestas (_response_) o variables dependientes.

Los modelos que pretendemos estudiar son de la forma:

$$
    Y = f(X) + \varepsilon
$$

donde se asume que el efecto de la variable aleatoria $\varepsilon$ puede de algún modo descartarse (porque el efecto sea pequeño, porque tenga una distribución conocida que permita controlarlo o por cualquier motivo).

### Predicción

Imaginemos que queremos predecir el comportamiento del sistema.

La cuestión es que conocer $f$ nos permitiría, con el máximo grado de precisión que nos deje $/varepsilon$ determinar $Y$. Por eso hemos de hayar una $\widehat{f}$ estimación de $f$ lo más adecuada posible, que nos permita calcular una estimación de las variables dependientes $\widehat{Y}=\widehat{f}(X)$.

Lo mejor que podemos esperar es que el error reducible (al estimar $f$) sea muy próximo a cero, pero siempre tendremos el error irreducible asociado a $\varepsilon$ de tal modo que:

$$
    E[\widehat{Y}-Y]^2 = E[\widehat{f}(X)-f(X)]^2 + Var(\varepsilon)
$$

### Inferencia

Puede que no querramos predecir nada sino entender cosas cómo:

- ¿Cómo se relacionan los diferentes predictores entre sí?
- ¿y con la respuesta?
- ¿Se relacionan todos en una medida relevante?
- ¿De qué tipo es la relación con cada uno de ellos? ¿Es lineal?

## Formas de estimar $f$

### Métodos paramétricos

Proponemos una familia de funciones $\widehat{f}$ con unos parámetros libres que hay que determinar para obtener la mejor aproximación posible.

### Métodos no paramétricos

Se busca una estimación de $f$ sin exigir una forma concreta. Se necesita una gran cantidad de datos. Ejemplo, un _thin plate spline_ o algunos modelos bayesianos.

### Exactitud vs Interpretabilidad

Los modelos más exactos suelen tener más parámetros, reglas etc que vuelven más complicado establecer relaciones entre cambios en las variables de input y su efecto en el output.

Los modelos más interpretables, que suelen tener pocos parámetros, reglas sencillas, etc., es difícil que capturen la complejidad de un sistema real.

Elige tus batallas.

## Aprendizaje

### Supervisado

Tenemos una serie de datos observados $(X_i,Y_i)$ y los usamos para predecir con la mayor exactitud posible observaciones futuras o para entender las relaciones entre las variables dependientes con las dependientes.

### No supervisado

Tenemos observaciones $X_i$ pero no hay observaciones de la variable dependiente. El objetivo es usando la distribución de los propios $X_i$ intentar extraer información de ella para estimar las respuestas a nuevos inputs con los inputs más probables dada dicha distribución.

### Mixto

Algunos problemas pueden caer en ambas categorías.

## Exactitud de los modelos.

Se usa mínimos cuadrados.

### Sets de entrenamiento y test

Para encontrar la $\widehat{f}$ adecuada, se suele usar mínimos cuadrados en un subconjunto de los datos (set de entrenamiento) con la esperanza (razonable aunque no garantizada) de obtener buenos resultados en mínimos cuadrados en el subconjunto de datos de test (pueden ser datos futuros en el caso de la predicción).

### Equilibrio (_trade-off_) entre sesgo (_bias_) y varianza

Siempre se tiene que:

$$
    E[y_0-\widehat{f}(x_0)]^2=Var(\widehat{f}(x_0))+Bias(\widehat{f}(x_0)) +Var(\varepsilon)
$$

donde la varianza del error irreducible la conocemos, la varianza de $\widehat{f}$ proviene de las posibles cambios en la elección de la función **si se cambian los conjuntos de entrenamiento** y el sesgo de $\widehat{f}$ es el error de aproximar con $\widehat{f}$ una función $f$ que en el mundo real puede ser terriblemente compleja. $x_0$ es una observación del conjunto de test (puede ser una observación futura).

Es decir, tenemos 3 fuentes de error:

- **Varianza del error irreducible**: Por la aletoriedad intrínseca del fenómeno modelado.
- **Sesgo del método de aprendizaje estadístico**: Por la simplificación de la realidad por una elección más o menos arbitraria de una $\widehat{f}$ (o de una familia de funciones etc, la idea es que el error viene de que $\widehat{f}$ no es $f$).
- **Varianza del método de aprendizaje estadístico**: Por la elección del conjunto de entrenamiento (que es también un proceso aleatorio).

## Tipos de problema

### Problemas de regresión

Como los vistos hasta este punto.

### Problemas de clasificación

Análogo a lo anterior. En lugar de usar mínimos cuadrados, usamos

$$
    \frac1n \sum I[y_i\neq \widehat{y}_i]
$$

### Clasificador de Bayes

En teoría el mejor clasificador posible es el de Bayes, el que maximiza:

$$
 P[Y=j | X=x_0]
$$

Es decir, si hay dos variables el que le asigna a cada $x_0$ el valor de $j$ que haga que la probabilidad sea mayor a $\frac12$. O en el caso general, el que le asigna a cada $x_0$ el valor de $j$ que haga que la probabilidad sea mayor a todas las demás.

El problema es que no conocemos la distribución de probabilidad.

#### $K$ Vecinos más próximos (_$K$-Nearest Neighbours_)

Es una técnica en la que se estima, fijado un natural $K$:

$$
  P[Y=j | X=x_0]=\frac1K \sum I(y_i=j) I(i \in KNN)
$$

Es decir, para predecir una clase, mira a los $K$ vecinos más cercanos (el conjunto $KNN$) y promedia.
