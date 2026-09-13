### Razón de oportunidades

Dados eventos $A_{i}$ ($i\in\{1..n\}$) una partición de eventos exhaustiva y mutuamente excluyentes (es decir, $\sum P[A_i]=1$) y en la que $\prod P[A_i]\neq 0$, para cualquier $M>0$
se define:

$$
    O(A_i)=M\, P[A_i]
$$
(es decir, se define salvo un factor de proporcionalidad común a todos los eventos). En particular las expresiones:

$$
\frac{P[A_i]}{P[A_n]}:\ldots:\frac{P[A_{n-1}]}{P[A_n]}:1
$$

$$
P[A_i]:\ldots:P[A_{n-1}]:P[A_n]
$$

$$
2P[A_i]:\ldots:2P[A_{n-1}]:2P[A_n]
$$

$$
M\, P[A_i]:\ldots:M\, P[A_{n-1}]:M\, P[A_n]
$$

son todas equivalentes.

Se define entonces la razón de oportunidades:

$$
    O(A_1):\cdots:O(A_n)
$$

como la clase a la que pertenecen todas las expresiones anteriores.

Algunas consideraciones:

- La razón de oportunidades está definida salvo una constante. Es decir, para cualquier $M>0$ se tiene que $M\, O(A_1):\cdots :M\, O(A_n)=O(A_1):\cdots:O(A_n)$, por definición.
- En la literatura existen varias traducciones para _odds ratio_: razón de probabilidades, razón relativa, razón de oportunidades, razón de posibilidades, razón de momios, razón de productos cruzados, razón de desigualdades, razón de disparidad, razón de exceso, oportunidad relativa, disparidad, desigualdad relativa, relación impar...
- Cuando los eventos son dos, las condiciones anteriores equivalen a que ambos eventos son complementarios, es decir, $A$ y $B$ con $P[B]=1-P[A]$. En ese caso en la literatura angloparlante se denomina habitualmente a $O(A):O(B)$ como las _odds_ del evento $A$. En estos apuntes se le denominará la razón de oportunidades de $A$, sin hacer referencia a su complementario, pero sin darle una expresión particular.
- Como ejemplo, tirar un dado y que salga un múltiplo de 3 (es decir, que salga 3 o 6) tiene una probabilidad de $2/6$ o equivalentemente $1/3$. Ahora, la razón de oportunidades de que salga un múltiplo de 3 frente a que no sería $a:b=(1/3):(2/3)$. Pero también se puede elegir $a=1$ y $b=2$ con lo que se obtiene $1:2$. Léase una oportunidad 1 a 2 (que salga un múltiplo de 3 -3 o 6- frente a 1, 2, 4 o 5, hay el doble de oportunidades). Pero también se puede expresar como $2:4$, que coincide con la razón de los casos favorables entre desfavorables.
- Es decir, si según la ley de Lagrange la probabilidad de A se interpreta como casos favorables entre casos posibles de que un evento ocurra, la razón de oportunidades de A se interpreta como los casos favorables entre los casos desfavorables.
- Para un ejemplo en el caso de más de dos eventos, supongamos que estamos probando un software para calificar alumnos y necesitamos generar datos para ponerlo a prueba. Para ello generamos, de forma uniforme, notas enteras entre 0 y 10. Se propone el ejemplo de sacar una nota en un examen menor que 5 (0, 1, 2, 3, 4, probabilidad 5/11), mayor que 8 (9 o 10, probabilidad 2/11) frente a que esté entre ambos (5, 6, 7, 8, probabilidad 4/11). En ese caso, $a:b:c$ que calculándolas esta vez como razones de probabilidades (simplemente por ver otro método) equivale a $(5/11)/(4/11):(2/11)/(4/11):1=5/4:2/4:1$, que resumidamente se puede expresar como $5:2:4$.
- En este caso se puede interpretar también que cada $O(A_j)$ representa los casos favorables a que suceda $A_j$ frente a los casos desfavorables $O(A_i)$ para $i\neq j$. En el ejemplo anterior, la interpretación de $5:2:4$ es que sacar menos de un 5 tiene 5 casos favorables frente a los $2+4=6$ casos desfavorables. Igualmente, sacar más de un 8 tiene 2 casos favorables frente a los $5+4=9$ casos desfavorables.
- Si se escoge un $A_j$ y se toma como representante de
  $O(A_1):\cdots:O(A_j):\cdots:O(A_n)$ a la expresión:
  $$
  \frac{P[A_1]}{P[A_j]}:\cdots:1:\cdots:
  \frac{P[A_n]}{P[A_j]}
  $$
  se denomina la relación de oportunidades respecto a la referencia (_baseline_) $A_j$. En muchas ocasiones se suprime el 1:
  $$
  \frac{P[A_1]}{P[A_j]}:\cdots:
  \frac{P[A_n]}{P[A_j]}
  $$
  muchas veces se toma $j=n$.
- En el caso anterior si se toma logaritmos:
  $$
  \log\frac{P[A_1]}{P[A_j]}:\cdots:
  \log\frac{P[A_n]}{P[A_j]}
  $$
  se le denomina logaritmo de la razón de oportunidades, y es lo que se usa para interpretar los coeficientes de la regresión logística.
- Por último, puede verse la expresión $O(A_1):\cdots:O(A_n)$ como las coordenadas homogéneas de un punto en el espacio proyectivo real $PR^{n-1}$. Por las condiciones impuestas, ninguno de estos puntos pertenece al hiperplano en el infinito. De hecho es un subconjunto un poco extraño porque ninguna o todas sus coordenadas tienen que ser estrictamente positivas (es decir, si vemos el espacio proyectivo como puntos antipodales en una hiperesfera y esta como subconjunto de un espacio euclídeo de una dimensión superior, uno de los puntos siempre vive en el primer $2^n$-ante - cuadrante, octante...- del espacio ambiente). No tiene más interés que justificar la notación con los :, que es la misma que se usa para coordenadas homogéneas.
