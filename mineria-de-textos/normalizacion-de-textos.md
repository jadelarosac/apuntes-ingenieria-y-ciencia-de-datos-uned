# Normalización de textos

Transformar el texto a una **forma canónica** para uniformizarlo y facilitar los procesos posteriores. Suele incluir:

- normalización de la forma de las palabras (incluyendo [[stemming-y-lematizacion]]);
- segmentación (tokenización) de oraciones;
- segmentación (tokenización) de palabras.

## Expresiones regulares

Lenguaje formal para especificar patrones de texto; herramienta básica en PLN (normalización, tokenización, [[reconocimiento-de-entidades#Reglas]], [[analisis-sintactico#Chunking]]). Distinguen mayúsculas y minúsculas.

Se construyen a partir de otras más simples: un carácter es una regex, y si $e_1$, $e_2$ lo son, también lo son la unión $(e_1|e_2)$, la concatenación $e_1e_2$ y la clausura de Kleene $e_1^*$.

| Operador | Significado | Ejemplo |
| --- | --- | --- |
| `.` | cualquier carácter | `b.t` |
| `^` / `$` | inicio / fin de cadena | `^abc`, `abc$` |
| `[abc]`, `[A-Z0-9]` | un carácter del conjunto / rango | `[Mm]anzana` |
| `\|` | disyunción | `ed\|ing\|s` |
| `*` / `+` / `?` | 0 o más / 1 o más / 0 o 1 | `[a-z]+` |
| `{n}`, `{n,}`, `{,n}`, `{m,n}` | número de repeticiones | `\d{4}` |
| `( )` | ámbito de los operadores | `a(b\|c)+` |

Referencia: Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 2.

## Normalización de palabras

Unificar formas distintas que se refieren a lo mismo: _"U.S.A"_ y _"USA"_, _"Manzana"_ y _"manzana"_, espacios repetidos, erratas. Técnicas habituales: pasar a minúsculas, quitar acentos, colapsar espacios.

## Tokenización

### De palabras

Romper una cadena de caracteres en una secuencia de palabras. No es trivial sin espacios: `myspace.com/pluckerswingbar` puede ser _plucker swing bar_ o _pluckers wing bar_.

```python
import nltk
nltk.download('punkt_tab')
from nltk.tokenize import word_tokenize

word_tokenize("Mr. Smith didn't pay $3.50 for the book.")
# ['Mr.', 'Smith', 'did', "n't", 'pay', '$', '3.50', 'for', 'the', 'book', '.']
```

### De oraciones

Las claves son los signos de puntuación. `?` y `!` son bastante inequívocos, pero el punto es ambiguo (fin de oración o abreviatura: _Mr._, _Inc._). Los métodos deciden primero, con reglas o aprendizaje automático, si el punto pertenece a la palabra; un diccionario de abreviaturas ayuda. Por eso a veces se tokenizan palabras y oraciones a la vez.

El tokenizador `punkt` de NLTK es no supervisado: aprende abreviaturas a partir del propio corpus ([Kiss y Strunk, 2006](https://aclanthology.org/J06-4003/)).

```python
nltk.sent_tokenize("Llegó el Sr. García. Luego se fue.", language="spanish")
```

## Palabras vacías

Palabras sin significado propio (artículos, preposiciones...). Son las más frecuentes y a menudo se eliminan para centrarse en las palabras con contenido.

```python
nltk.download('stopwords')
from nltk.corpus import stopwords
stopwords.words('spanish')   # o 'english'
```

En spaCy, cada token tiene el atributo `token.is_stop` (ver [[nltk-spacy]]).

## Distancia de edición

La **distancia de edición mínima** entre dos cadenas es el coste mínimo de las operaciones de **inserción**, **borrado** y **sustitución** necesarias para transformar una en otra. Mide la similitud entre cadenas: corrección ortográfica (¿_graffe_ está más cerca de _graf_, _graff_ o _giraffe_?), extracción y recuperación de información, traducción automática.

Se calcula con programación dinámica ([Wagner y Fischer, 1974](https://doi.org/10.1145/321796.321811)). Siendo $D(i,j)$ la distancia entre los $i$ primeros caracteres de $x$ y los $j$ primeros de $y$:

$$
D(i,j) = \min
\begin{cases}
D(i-1,j) + c_{del} \\
D(i,j-1) + c_{ins} \\
D(i-1,j-1) + \begin{cases} c_{sub} & x_i \neq y_j \\ 0 & x_i = y_j \end{cases}
\end{cases}
$$

con $D(i,0)=i\,c_{del}$ y $D(0,j)=j\,c_{ins}$.

Ejemplo de SLP3: de _intention_ a _execution_ la distancia es **5** si todas las operaciones cuestan 1, y **8** con la **distancia de Levenshtein** (sustitución de coste 2).

```python
nltk.edit_distance("intention", "execution")                           # 5
nltk.edit_distance("graffe", "giraffe"), nltk.edit_distance("graffe", "graf")  # (1, 2)
```

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 2 (_Words and Tokens_, expresiones regulares y distancia de edición).
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. 3 (_Processing Raw Text_).
- Wagner, R. A. y Fischer, M. J. (1974). The String-to-String Correction Problem. _Journal of the ACM_, 21(1). [doi:10.1145/321796.321811](https://doi.org/10.1145/321796.321811)
- Kiss, T. y Strunk, J. (2006). Unsupervised Multilingual Sentence Boundary Detection. _Computational Linguistics_, 32(4). [ACL Anthology](https://aclanthology.org/J06-4003/)
