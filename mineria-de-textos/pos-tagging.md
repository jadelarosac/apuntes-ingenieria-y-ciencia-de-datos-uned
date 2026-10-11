# POS tagging

El **etiquetado gramatical** (_Part-Of-Speech tagging_) asigna a cada palabra de una oración su categoría léxica (nombre, verbo, adjetivo...) según su definición y su **contexto** (palabras y etiquetas vecinas):

`The/DT grand/JJ jury/NN commented/VBD on/IN a/DT number/NN of/IN other/JJ topics/NNS ./.`

Es la base de otras tareas: [[reconocimiento-de-entidades]], [[desambiguacion-del-sentido]], [[analisis-sintactico]].

## Conjuntos de etiquetas

- **Penn Treebank** (45 etiquetas, inglés): `NN`, `NNS`, `VBD`, `JJ`, `DT`, `IN`... ([Marcus et al., 1993](https://aclanthology.org/J93-2004/)). Es el que usa `nltk.pos_tag`.
- **Brown** (87 etiquetas), ver [[corpus#Generales]].
- **Universal POS tags** ([UD](https://universaldependencies.org/u/pos/)): conjunto reducido y multilingüe (`NOUN`, `VERB`, `ADJ`...). Es el que usa `token.pos_` en spaCy.

## Técnicas

- **Basadas en reglas**.
- **Modelos estadísticos**: probabilidades de secuencias de palabras y etiquetas estimadas a partir de un corpus anotado (p. ej. HMM, ver [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 18).
- **Aprendizaje automático**: clasificadores secuenciales (perceptrón, CRF, redes neuronales).

**Baseline** típico: asignar a cada palabra su etiqueta más frecuente (ver [[evaluacion-pln#Baseline]]).

## Con NLTK

### Inglés

Trae un etiquetador preentrenado (perceptrón promediado):

```python
import nltk
nltk.download('averaged_perceptron_tagger_eng')
nltk.pos_tag(nltk.word_tokenize("And now for something completely different"))
# [('And', 'CC'), ('now', 'RB'), ('for', 'IN'), ('something', 'NN'), ('completely', 'RB'), ('different', 'JJ')]
```

### Español con NLTK

No hay modelo preentrenado: hay que entrenar con un corpus anotado como **CESS-ESP** (etiquetas EAGLES, p. ej. `ncfs000` = nombre común femenino singular). Opciones:

- **unigramas**: la etiqueta depende solo de la palabra;
- **bigramas**: depende de la palabra y de la etiqueta anterior. Con `backoff` se recurre a un modelo más simple cuando el contexto no se ha visto.

```python
nltk.download('cess_esp')
from nltk.corpus import cess_esp
from nltk import UnigramTagger, BigramTagger

sents = cess_esp.tagged_sents()
uni = UnigramTagger(sents)
bi = BigramTagger(sents, backoff=uni)
bi.tag("Esto es una pequeña prueba del tagger en español .".split())
# [('Esto', 'pd0ns000'), ('es', 'vsip3s0'), ..., ('tagger', None), ...]   palabra no vista → None
```

## Con spaCy

`token.pos_` da la etiqueta de grano grueso (Universal) y `token.tag_` la de grano fino:

```python
import spacy
nlp = spacy.load("es_core_news_sm")
for token in nlp("Iberdrola ha alcanzado un beneficio neto de 1.644,4 millones de euros"):
    print(token.text, token.pos_, token.tag_)
```

## Evaluación

- **Exactitud** (_accuracy_): etiquetas correctas / palabras etiquetadas.
- **Precisión, cobertura y F1 por etiqueta** (ver [[evaluacion-pln#Medidas]]).

| Palabra | Referencia | Predicción |
| --- | --- | --- |
| EU | NNP | NNP |
| rejects | VBZ | VBZ |
| German | JJ | JJ |
| call | **NN** | **VB** |
| to | TO | TO |
| boycott | **VB** | **NN** |
| British | JJ | JJ |
| lamb | NN | NN |
| . | . | . |

Exactitud $= 7/9 \approx 0{,}78$. Para `NN`: la referencia tiene 2 (_call_, _lamb_), el sistema predice 2 (_boycott_, _lamb_) y acierta 1, así que $P = R = F_1 = 0{,}5$.

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 18.
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. 5 (_Categorizing and Tagging Words_).
- Marcus, M. P., Santorini, B. y Marcinkiewicz, M. A. (1993). Building a Large Annotated Corpus of English: The Penn Treebank. _Computational Linguistics_, 19(2). [ACL Anthology](https://aclanthology.org/J93-2004/)
- [Universal POS tags](https://universaldependencies.org/u/pos/).
