# Evaluación de tareas de PLN

Para comparar modelos de forma justa hay que usar **los mismos datos** ([[corpus]] comunes, a menudo de campañas de evaluación) y **las mismas medidas**.

## Medidas

Con $tp$ verdaderos positivos, $fp$ falsos positivos y $fn$ falsos negativos respecto a los datos de referencia (_gold standard_):

- **Precisión**: fracción de las predicciones que son correctas.
$$
p = \frac{tp}{tp + fp}
$$
- **Cobertura** o exhaustividad (_recall_): fracción de los datos de referencia que el modelo encuentra.
$$
r = \frac{tp}{tp + fn}
$$
- **Medida F** ($F_1$): media armónica de ambas.
$$
F_1 = \frac{2pr}{p + r}
$$
- **Medida $F_\beta$**: da más peso a la cobertura ($\beta > 1$) o a la precisión ($\beta < 1$) ([van Rijsbergen, 1979](http://www.dcs.gla.ac.uk/Keith/Chapter.7/Ch.7.html)).
$$
F_\beta = \frac{(1+\beta^2)\,p\,r}{\beta^2 p + r}
$$
- **Exactitud** (_accuracy_): aciertos / total. Útil cuando se clasifica cada elemento, como en [[pos-tagging#Evaluación]].

Ejemplo: el modelo propone 13 elementos, la referencia tiene 11 y coinciden 8. Entonces $p = 8/13$, $r = 8/11$ y $F_1 = \frac{2 \cdot 8}{13 + 11} \approx 0{,}67$.

Muchas tareas tienen medidas propias o una unidad de evaluación concreta (p. ej. entidades completas en [[reconocimiento-de-entidades#Evaluación]]).

## Baseline

El rango de valores aceptables cambia mucho entre tareas, y en tareas nuevas no hay resultados previos con los que comparar. El **baseline** es el resultado de un modelo básico que resuelve la tarea; cualquier propuesta debería superarlo.

Ejemplo: en [[desambiguacion-del-sentido]], asignar el sentido más frecuente de la palabra.

## Análisis de errores

Es clave en cualquier sistema de PLN: descubre errores en el programa o en los datos de entrenamiento y orienta nuevos modelos.

La **matriz de confusión** de un clasificador de $N$ clases es una matriz $N \times N$ cuya celda $(x, y)$ cuenta cuántas veces un elemento de la clase real $x$ se clasificó como $y$ (a veces en porcentaje):

| Real \ Predicha | 1 | 2 | 3 | 4 | Total |
| --- | --- | --- | --- | --- | --- |
| **1** | 70 | 10 | 15 | 5 | 100 |
| **2** | 8 | 67 | 20 | 5 | 100 |
| **3** | 0 | 11 | 88 | 1 | 100 |
| **4** | 4 | 10 | 14 | 72 | 100 |

La diagonal son aciertos. Aquí la clase 3 atrae muchos errores (columna 3).

## Datos de entrenamiento y test

- Se evalúa sobre datos **no usados en el desarrollo** (partición entrenamiento / desarrollo / test).
- **Validación cruzada** de $k$ particiones (_k-fold cross-validation_, típicamente $k = 10$): se promedian $k$ evaluaciones con distintas particiones disjuntas de entrenamiento y test.
- Los parámetros del modelo pueden ajustarse para favorecer la precisión o la cobertura.
- Si no hay _gold standard_ (relaciones nuevas), ver [[extraccion-de-relaciones#Evaluación]].

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 4 (clasificación: precisión, cobertura, F y validación cruzada).
- van Rijsbergen, C. J. (1979). _Information Retrieval_ (2.ª ed.), cap. 7 (_Evaluation_), Butterworths. [Versión en línea](http://www.dcs.gla.ac.uk/Keith/Chapter.7/Ch.7.html)
