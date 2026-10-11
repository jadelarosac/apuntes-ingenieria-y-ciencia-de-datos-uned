# Stemming y lematización

Ambos reducen las variantes de una palabra a una forma común, como parte de la [[normalizacion-de-textos]].

## Análisis morfológico

La morfología estudia la estructura de las palabras. Un **morfema** es la unidad mínima con significado. El análisis morfológico segmenta una palabra en morfemas:

- **raíz** (_stem_): aporta el significado principal;
- **afijos** (prefijos y sufijos): añaden número, género, tiempo verbal, etc.

Ejemplos: _cats_ → _cat + s_; _carried_ → _carry + ed_; _perros_ → _perr + o + s_; _acrecentar_ → _a + crec + entar_.

## Diferencias

- **Lematización**: devuelve el **lema** o forma de diccionario (_leíamos_ → _leer_). Usa un vocabulario y análisis morfológico, y normalmente solo quita la flexión.
- **Stemming**: recorta heurísticamente el final de la palabra. Es una aproximación más tosca a la lematización y suele unir también palabras derivadas. Ante _saw_, un stemmer puede devolver _s_, mientras que un lematizador devuelve _see_ o _saw_ según sea verbo o nombre ([Manning, Raghavan y Schütze, 2008](https://nlp.stanford.edu/IR-book/html/htmledition/stemming-and-lemmatization-1.html)).

El stemming es muy común porque agrupa todas las palabras con la misma raíz (útil, por ejemplo, en recuperación de información).

## Stemming

NLTK incluye stemmers ya hechos, preferibles a los construidos a mano con expresiones regulares porque manejan casos irregulares:

- **Porter** ([Porter, 1980](https://doi.org/10.1108/eb046814)): el más usado; solo inglés.
- **Lancaster** ([Paice, 1990](https://doi.org/10.1145/101306.101310)): más agresivo.
- **Snowball** ([snowballstem.org](https://snowballstem.org/)): versión mejorada de Porter para muchas lenguas (español, inglés, francés, portugués, italiano, alemán, neerlandés, sueco, noruego, danés, ruso, finés...).

```python
import nltk
from nltk.stem import PorterStemmer, SnowballStemmer

tokens = nltk.word_tokenize("Supreme executive power derives from a mandate from the masses")
[PorterStemmer().stem(t) for t in tokens]
# ['suprem', 'execut', 'power', 'deriv', 'from', 'a', 'mandat', 'from', 'the', 'mass']

stemmer = SnowballStemmer("spanish")
[stemmer.stem(t) for t in nltk.word_tokenize("Una excelente cámara para empezar en la fotografía")]
# ['una', 'excelent', 'cam', 'par', 'empez', 'en', 'la', 'fotograf']
```

## Lematización

NLTK lematiza en inglés usando [[desambiguacion-del-sentido#WordNet|WordNet]]. Por defecto trata cada palabra como nombre; con `pos` se indica otra categoría:

```python
nltk.download('wordnet')
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()
lemmatizer.lemmatize("women"), lemmatizer.lemmatize("lying", pos="v")   # ('woman', 'lie')
```

spaCy lematiza con el modelo cargado y expone el lema en `token.lemma_` (ver [[nltk-spacy]]):

```python
import spacy
nlp = spacy.load("es_core_news_sm")
[(t.text, t.lemma_) for t in nlp("Las niñas habían leído los libros")]
# [('Las', 'el'), ('niñas', 'niña'), ('habían', 'haber'), ('leído', 'leer'), ('los', 'el'), ('libros', 'libro')]
```

## Referencias

- Manning, C. D., Raghavan, P. y Schütze, H. (2008). _Introduction to Information Retrieval_, Cambridge University Press. [Stemming and lemmatization](https://nlp.stanford.edu/IR-book/html/htmledition/stemming-and-lemmatization-1.html)
- Porter, M. F. (1980). An algorithm for suffix stripping. _Program_, 14(3). [doi:10.1108/eb046814](https://doi.org/10.1108/eb046814)
- Paice, C. D. (1990). Another stemmer. _ACM SIGIR Forum_, 24(3). [doi:10.1145/101306.101310](https://doi.org/10.1145/101306.101310)
- [Snowball](https://snowballstem.org/) y [NLTK: stem](https://www.nltk.org/howto/stem.html).
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. 3.
