# Análisis sintáctico

El análisis sintáctico (_parsing_) identifica la estructura sintáctica de una oración. La estructura obtenida depende del tipo de gramática. Sirve de representación intermedia para el análisis semántico, la búsqueda de respuestas o la [[extraccion-de-informacion]].

Su principal dificultad es la **ambigüedad estructural**: una oración puede tener varios árboles válidos con la misma gramática. Ejemplo clásico: _"I shot an elephant in my pajamas"_ (¿quién lleva el pijama?).

## Chunking

Análisis superficial: segmentar y etiquetar secuencias de varias palabras (_chunks_) sin construir el árbol completo. El más habitual es el de **frases nominales** (_NP chunking_):

`[The/DT market/NN] for/IN [system-management/NN software/NN] for/IN [Digital/NNP] ...`

- Los NP-chunks son más pequeños que las frases nominales: **no contienen otros chunks** y no suelen incluir sintagmas preposicionales ni subordinadas.
- Se definen con **expresiones regulares sobre etiquetas POS** (ver [[pos-tagging]]) o con clasificadores entrenados. Para tratarlo como clasificación por token se usa la codificación **IOB** ([Ramshaw y Marcus, 1995](https://aclanthology.org/W95-0107/)), la misma que en [[reconocimiento-de-entidades#Codificación de las clases]].

```python
import nltk
sentence = [("the", "DT"), ("little", "JJ"), ("yellow", "JJ"), ("dog", "NN"),
            ("barked", "VBD"), ("at", "IN"), ("the", "DT"), ("cat", "NN")]
cp = nltk.RegexpParser("NP: {<DT>?<JJ>*<NN>}")
print(cp.parse(sentence))
# (S (NP the/DT little/JJ yellow/JJ dog/NN) barked/VBD at/IN (NP the/DT cat/NN))
```

## Análisis de constituyentes

Descompone la oración en **constituyentes** anidados según una gramática. Se usan **gramáticas independientes del contexto** (_Context-Free Grammar_, CFG), formadas por:

- terminales $\Sigma$ (las palabras);
- no terminales $N$ (`S`, `NP`, `VP`...);
- un símbolo inicial $S \in N$;
- reglas $A \to \alpha$, con $A \in N$ y $\alpha \in (\Sigma \cup N)^*$.

```python
from nltk import CFG, ChartParser
grammar = CFG.fromstring("""
S -> NP VP
PP -> P NP
NP -> Det N | Det N PP | 'I'
VP -> V NP | VP PP
Det -> 'an' | 'my'
N -> 'elephant' | 'pajamas'
V -> 'shot'
P -> 'in'
""")
sent = "I shot an elephant in my pajamas".split()
for tree in ChartParser(grammar).parse(sent):
    print(tree)    # dos árboles: el PP modifica a 'elephant' o a 'shot'
```

### PCFG

En una **CFG probabilística** cada regla tiene una probabilidad (las de un mismo no terminal suman 1). La probabilidad de un árbol es el producto de las de sus reglas, y el analizador busca el **árbol más probable** (algoritmo de Viterbi). Así se descartan análisis válidos pero improbables.

```python
from nltk import PCFG, ViterbiParser
grammar = PCFG.fromstring("""
S -> NP VP [1.0]
PP -> P NP [1.0]
NP -> Det N [0.5] | Det N PP [0.3] | 'I' [0.2]
VP -> V NP [0.6] | VP PP [0.4]
Det -> 'an' [0.5] | 'my' [0.5]
N -> 'elephant' [0.5] | 'pajamas' [0.5]
V -> 'shot' [1.0]
P -> 'in' [1.0]
""")
for tree in ViterbiParser(grammar).parse(sent):
    print(tree)    # un único árbol, el más probable
```

### Técnicas

- **Programación dinámica**: análisis **descendente** (desde el símbolo inicial) o **ascendente** (desde las palabras), p. ej. _chart parsing_ o CKY.
- **Aprendizaje automático** a partir de un corpus anotado con árboles (_treebank_), p. ej. el [Penn Treebank](https://aclanthology.org/J93-2004/).

## Análisis de dependencias

Representa la oración como **relaciones binarias entre palabras** (núcleo → dependiente), que pueden llevar nombre (sujeto, objeto...). Frente al de constituyentes:

- normalmente no hay gramática: se aprende de un _treebank_ de dependencias;
- las palabras están en todos los nodos, no solo en las hojas.

Un análisis es **proyectivo** si los arcos no se cruzan al dibujarlos sobre la oración.

```python
grammar = nltk.DependencyGrammar.fromstring("""
'shot' -> 'I' | 'elephant' | 'in'
'elephant' -> 'an' | 'in'
'in' -> 'pajamas'
'pajamas' -> 'my'
""")
for tree in nltk.ProjectiveDependencyParser(grammar).parse(sent):
    print(tree)
```

En spaCy, la relación de cada token con su núcleo está en `token.dep_` y el núcleo en `token.head` (ver [[nltk-spacy]]).

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), caps. 19 (_Context-Free Grammars and Constituency Parsing_) y 20 (_Dependency Parsing_); apéndice E (_Statistical Constituency Parsing_).
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), caps. [7](https://www.nltk.org/book/ch07.html) (_chunking_) y 8 (_Analyzing Sentence Structure_).
- Ramshaw, L. y Marcus, M. (1995). Text Chunking using Transformation-Based Learning. _Third Workshop on Very Large Corpora_. [ACL Anthology](https://aclanthology.org/W95-0107/)
