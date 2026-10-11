# Desambiguación del sentido de las palabras

Muchas palabras tienen varios sentidos según el contexto: _banco_ de sardinas, de dinero o para sentarse. La **desambiguación del sentido** (_Word Sense Disambiguation_, WSD) elige el sentido correcto de una aparición de la palabra entre todos los posibles.

Aplicaciones:

- **traducción automática**: cada sentido puede traducirse distinto;
- **recuperación de información**: desambiguar consultas (_depresión_: enfermedad o situación económica);
- **extracción de información**: _drug_ puede ser medicamento o sustancia ilegal.

## Técnicas

- **Basadas en diccionarios** y bases de conocimiento léxico: comparan definiciones (p. ej. [[#Algoritmo de Lesk]]).
- **Supervisadas**: entrenadas sobre corpus anotados con sentidos.
- **Semisupervisadas**: parten de un pequeño conjunto anotado como semilla y amplían por _bootstrapping_ (idea análoga a la de [[extraccion-de-relaciones#Semisupervisada (bootstrapping)]]).
- **No supervisadas**: inducen sentidos agrupando apariciones por similitud de contexto, bajo la hipótesis de que sentidos similares aparecen en contextos similares.

**Baseline**: asignar siempre el sentido más frecuente (ver [[evaluacion-pln#Baseline]]).

## Relaciones entre sentidos

- **Sinónimos**: palabras distintas con significado igual o muy parecido.
- **Hipónimo**: sentido más específico que otro (subclase): _perro_ de _animal_.
- **Hiperónimo**: sentido más general (superclase): _animal_ de _perro_. Es la relación _is-a_ (ver [[extraccion-de-relaciones#Patrones léxico-sintácticos]]).

## WordNet

[WordNet](https://wordnet.princeton.edu/) ([Miller, 1995](https://doi.org/10.1145/219717.219748)) es una base de datos léxica del inglés con nombres, verbos, adjetivos y adverbios.

- Cada sentido es un **synset** (_synonym set_): el conjunto de palabras que pueden expresarlo. Sus elementos rara vez son sinónimos perfectos, pero bastan para analizar similitudes y diferencias.
- Cada synset tiene una **glosa** (definición y/o ejemplos).
- Los synsets se enlazan por relaciones como la hiperonimia.
- Se usa en WSD, recuperación de información, clasificación de textos, resumen automático y traducción.

```python
import nltk
nltk.download('wordnet')
from nltk.corpus import wordnet as wn
for s in wn.synsets('book')[:3]:
    print(s.name(), s.definition(), s.hypernyms())
```

## Algoritmo de Lesk

Método basado en diccionarios ([Lesk, 1986](https://doi.org/10.1145/318723.318728)): elige el sentido cuya **glosa comparte más palabras con el contexto** de la palabra ambigua.

Ejemplo: _"This **book** has too many pages for him."_

1. _a written work or composition that has been published (printed on **pages** bound together)_: 1 coincidencia.
2. _arrange for and reserve (something for someone else) in advance_: 0 coincidencias.

Se elige el sentido 1.

NLTK lo implementa con las glosas de WordNet ([`nltk.wsd`](https://www.nltk.org/api/nltk.wsd.html)). Al ser un método tan simple, a menudo falla:

```python
from nltk.wsd import lesk
from nltk.tokenize import word_tokenize

lesk(word_tokenize("I love reading books on coding."), 'book')
# Synset('book.n.01'): a written work or composition that has been published...
lesk(word_tokenize("The table was already booked by someone else."), 'book')
# Synset('record.n.05'): a compilation of the known facts... (incorrecto: debería ser un sentido verbal de reservar)
```

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), apéndice I (_Word Senses and WordNet_, solo en la web).
- Lesk, M. (1986). Automatic sense disambiguation using machine readable dictionaries: how to tell a pine cone from an ice cream cone. _SIGDOC '86_. [doi:10.1145/318723.318728](https://doi.org/10.1145/318723.318728)
- Miller, G. A. (1995). WordNet: a lexical database for English. _Communications of the ACM_, 38(11). [doi:10.1145/219717.219748](https://doi.org/10.1145/219717.219748)
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. 2 (_Accessing Text Corpora and Lexical Resources_).
