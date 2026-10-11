# NLTK y spaCy

Las dos librerías de Python para PLN que se usan en la asignatura. Ambas funcionan en Linux, Windows y macOS. Los ejemplos de cada tarea están en su nota: [[normalizacion-de-textos]], [[stemming-y-lematizacion]], [[pos-tagging]], [[analisis-sintactico]], [[desambiguacion-del-sentido]], [[reconocimiento-de-entidades]].

## NLTK

[NLTK](https://www.nltk.org/) (_Natural Language Toolkit_) es una caja de herramientas didáctica con corpus, recursos léxicos y algoritmos clásicos. Su documentación de referencia es el [NLTK Book](https://www.nltk.org/book/).

Los recursos se descargan aparte con `nltk.download(...)`:

| Recurso | Uso |
| --- | --- |
| `punkt_tab` | `word_tokenize`, `sent_tokenize` |
| `stopwords` | listas de palabras vacías |
| `averaged_perceptron_tagger_eng` | `pos_tag` (inglés) |
| `maxent_ne_chunker_tab`, `words` | `ne_chunk` (NER) |
| `wordnet` | `WordNetLemmatizer`, `wordnet`, `lesk` |
| `cess_esp`, `conll2002`, `brown` | corpus |

## spaCy

[spaCy](https://spacy.io/) está orientada a producción: aplica una cadena de procesamiento (_pipeline_) con modelos preentrenados por idioma ([modelos en español](https://spacy.io/models/es)). Los modelos hay que descargarlos antes de usarlos:

```bash
python -m spacy download es_core_news_sm   # español
python -m spacy download en_core_web_sm    # inglés
```

### Objetos

- **Contenedores**: `Doc` (texto procesado, secuencia de tokens indexados desde 0), `Span` (fragmento, p. ej. una oración o entidad) y `Token`. Imitan la estructura del texto: un texto tiene oraciones y estas tienen tokens.
- **Componentes del pipeline**: `tok2vec`, `tagger` (en los modelos españoles, `morphologizer`), `parser`, `lemmatizer`, `ner`... Se pueden desactivar para ganar velocidad, aunque entonces se pierden sus anotaciones:

```python
import spacy
nlp = spacy.load("es_core_news_sm", disable=["parser"])   # sin dependencias
print(nlp.pipe_names)   # ['tok2vec', 'morphologizer', 'attribute_ruler', 'lemmatizer', 'ner']
```

El tokenizador también se puede personalizar ([_linguistic features: tokenization_](https://spacy.io/usage/linguistic-features#tokenization)).

### Atributos de Token

Ver la [API de Token](https://spacy.io/api/token).

| Atributo | Contenido |
| --- | --- |
| `text` | texto |
| `lemma_` | lema |
| `pos_` | POS de grano grueso ([Universal](https://universaldependencies.org/u/pos/)) |
| `tag_` | POS de grano fino |
| `dep_` | relación de dependencia sintáctica |
| `head` | núcleo sintáctico |
| `shape_` | forma (`Xxxxx`, `d,d`) |
| `is_alpha`, `is_stop` | ¿alfabético? ¿palabra vacía? |
| `ent_type_`, `ent_iob_` | clase de entidad y código IOB |

```python
nlp = spacy.load("es_core_news_sm")
doc = nlp("Iberdrola ha alcanzado un beneficio neto de 1.644,4 millones de euros en el primer semestre del año")
for t in doc:
    print(t.text, t.lemma_, t.pos_, t.tag_, t.dep_, t.shape_, t.is_alpha, t.is_stop)
```

## Otras herramientas

- [Stanford NLP](https://nlp.stanford.edu/software/) (POS tagging, análisis sintáctico, NER...).
- [TreeTagger](https://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/): POS tagger para muchas lenguas.
- [FreeLing](https://nlp.lsi.upc.edu/freeling/): análisis lingüístico con buen soporte del español.

## Referencias

- Bird, S., Klein, E. y Loper, E. (2009). _Natural Language Processing with Python_. O'Reilly. [nltk.org/book](https://www.nltk.org/book/)
- Documentación de spaCy: [uso](https://spacy.io/usage/), [pipelines](https://spacy.io/usage/processing-pipelines), [API de Token](https://spacy.io/api/token).
