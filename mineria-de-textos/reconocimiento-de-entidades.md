# Reconocimiento de entidades

Una **entidad nombrada** (_named entity_) es un nombre propio (persona, organización, lugar...) y, por extensión, cualquier expresión que identifica un concepto de una clase (enfermedades, medicamentos, proteínas...). El **reconocimiento de entidades** (_Named Entity Recognition_, NER) identifica el segmento de texto de la entidad (**detección**) y le asigna una clase (**clasificación**). Es la primera tarea de la [[extraccion-de-informacion]].

Ejemplo anotado al estilo MUC-7 ([Chinchor, 1998](https://aclanthology.org/M98-1001/)):

```xml
<ENAMEX TYPE="LOCATION">Italy</ENAMEX>'s business world was rocked by the announcement
<TIMEX TYPE="DATE">last Thursday</TIMEX> that Mr. <ENAMEX TYPE="PERSON">Verdi</ENAMEX>
would leave his job as vice-president of
<ENAMEX TYPE="ORGANIZATION">Music Masters of Milan, Inc</ENAMEX>.
```

## Dificultades y pistas

- Las entidades son demasiadas para listarlas todas, cambian a menudo y aparecen en formas variadas o abreviadas.
- Pistas para reconocerlas:
  - **estructura interna**: _Sr. Álvarez_;
  - **contexto**: _"his job as vice-president of Music Masters of Milan"_ indica que es una empresa.

## Métodos

### Diccionarios

Listas de entidades conocidas. Los **gazetteers** son listas de nombres de lugares, con sus variantes e información geográfica y política (p. ej. los de [GATE](https://gate.ac.uk/sale/tao/splitch13.html)).

### Reglas

Patrones, normalmente [[normalizacion-de-textos#Expresiones regulares|expresiones regulares]] escritas a mano. Funcionan bien cuando las entidades siguen un patrón común:

| Rasgo | Ejemplo | Clase |
| --- | --- | --- |
| 4 dígitos | 2019 | año |
| todo mayúsculas | ONU | organización |
| mayúscula + punto | L. | inicial de persona |
| dígitos y letras | X2345 | código de producto |

### Aprendizaje automático

Se trata como **etiquetado de secuencias**:

1. Reunir documentos de entrenamiento representativos.
2. Etiquetar cada token con la clase de su entidad o `O` (_other_).
3. Diseñar rasgos (ver abajo).
4. Entrenar un clasificador secuencial (HMM, CRF...) y aplicarlo a documentos nuevos.

Rasgos típicos: la palabra $w_i$ y sus vecinas, la etiqueta POS de $w_i$ y de sus vecinas ([[pos-tagging]]), prefijos y sufijos, si empieza por mayúscula, la forma de la palabra (`Xxxx`, `dd`), pertenencia a un gazetteer...

### Codificación de las clases

Para que un clasificador por tokens pueda marcar entidades de varias palabras ([Ramshaw y Marcus, 1995](https://aclanthology.org/W95-0107/)):

- **IOB**: `B-` inicio, `I-` dentro, `O` fuera;
- **IO**: solo `I-` y `O` (no separa dos entidades seguidas del mismo tipo);
- otras, como **BILOU** (añade `L-` último y `U-` unidad), que suele funcionar mejor ([Ratinov y Roth, 2009](https://aclanthology.org/W09-1119/)).

| Palabra | IOB | IO |
| --- | --- | --- |
| American | B-ORG | I-ORG |
| Airlines | I-ORG | I-ORG |
| , | O | O |
| a | O | O |
| unit | O | O |
| of | O | O |
| AMR | B-ORG | I-ORG |
| Corp. | I-ORG | I-ORG |
| ... | O | O |
| spokesman | O | O |
| Tim | B-PER | I-PER |
| Wagner | I-PER | I-PER |
| said | O | O |

### Deep learning

- **Embeddings de palabras**: vectores densos de dimensión mucho menor que el vocabulario. Se basan en la **hipótesis distribucional** (palabras en contextos similares tienen significados similares), así que palabras cercanas en significado quedan cerca en el espacio y permiten hacer álgebra: $\vec{rey} - \vec{hombre} + \vec{mujer} \approx \vec{reina}$. Se obtienen por conteo o con redes neuronales: **word2vec** ([Mikolov et al., 2013](https://arxiv.org/abs/1301.3781)), **GloVe** ([Pennington et al., 2014](https://aclanthology.org/D14-1162/)).
- **Bi-LSTM** ([Hochreiter y Schmidhuber, 1997](https://doi.org/10.1162/neco.1997.9.8.1735)): recibe embeddings de palabras y de caracteres y da una probabilidad para cada etiqueta de cada palabra. Con una capa **CRF** encima se obtiene la arquitectura de referencia BiLSTM-CRF ([Huang et al., 2015](https://arxiv.org/abs/1508.01991); [Lample et al., 2016](https://aclanthology.org/N16-1030/)). Antecedente: SENNA ([Collobert et al., 2011](https://jmlr.org/papers/v12/collobert11a.html); código de ejemplo en el [tutorial de UKPLab](https://github.com/UKPLab/deeplearning4nlp-tutorial/tree/master/2016-11_Seminar/Session%201%20-%20SENNA/code%20for%20NER)).
- **Transformers** ([Vaswani et al., 2017](https://arxiv.org/abs/1706.03762)): redes basadas en **autoatención**, que conecta directamente cualquier par de posiciones, así que las dependencias lejanas cuentan igual que las cercanas. Son la base de los LLM actuales. Para NER se ajusta (_fine-tuning_) un modelo como **BERT** ([Devlin et al., 2019](https://aclanthology.org/N19-1423/)). Superan a otras redes si hay suficientes datos y capacidad de cómputo.

## Con NLTK

`nltk.ne_chunk()` es un clasificador preentrenado sobre texto ya etiquetado con POS. Con `binary=True` marca todo como `NE`; si no, da la categoría (`PERSON`, `ORGANIZATION`, `GPE`...).

```python
import nltk
for r in ['punkt_tab', 'averaged_perceptron_tagger_eng', 'maxent_ne_chunker_tab', 'words']:
    nltk.download(r)

doc = "Andrew Ng is the co-founder of Coursera. He was born in the UK in 1976."
tree = nltk.ne_chunk(nltk.pos_tag(nltk.word_tokenize(doc)))
entities = [(" ".join(w for w, _ in t.leaves()), t.label())
            for t in tree if hasattr(t, "label")]
print(entities)   # [('Andrew Ng', 'PERSON'), ('Coursera', 'GPE')]: Coursera mal clasificada y UK sin detectar
```

## Con spaCy

```python
import spacy
nlp = spacy.load("es_core_news_sm")
doc = nlp("Pedro Sánchez visitó la sede de Iberdrola en Bilbao.")
for ent in doc.ents:
    print(ent.text, ent.label_)                        # PER, ORG, LOC, MISC
for token in doc:
    print(token.text, token.ent_iob_ + "-" + token.ent_type_)
```

`token.ent_iob_` vale `B`, `I`, `O` o `""` (sin etiqueta asignada); `token.ent_type_` es la clase ([API de Token](https://spacy.io/api/token)).

## Evaluación

La **unidad de evaluación es la entidad**, no el token. Con $N$ entidades en la referencia, $E$ extraídas y $C$ correctas:

$$
P = \frac{C}{E} \qquad R = \frac{C}{N} \qquad F_1 = \frac{2PR}{P+R}
$$

| Token | Referencia | Predicción |
| --- | --- | --- |
| Jane | B | O |
| Villanueva | I | B |
| of | O | O |
| United | B | B |
| Airlines | I | I |
| Holding | I | O |
| discussed | O | O |
| the | O | O |
| Chicago | B | B |
| route | O | O |

$N = 3$, $E = 3$, $C = 1$ (solo _Chicago_), así que $P = R = F_1 = 1/3$.

Como la segmentación forma parte de la tarea, un error de límites cuenta doble: marcar _American_ en lugar de _American Airlines_ produce un falso positivo y un falso negativo. Además, entrenar por tokens y evaluar por entidades crea un desajuste entre entrenamiento y test. Ver también [[evaluacion-pln]].

## Corpus

- **CoNLL-2002** (español y neerlandés; [Tjong Kim Sang, 2002](https://aclanthology.org/W02-2024/)), en NLTK: `from nltk.corpus import conll2002`.
- **CoNLL-2003** (inglés y alemán; [Tjong Kim Sang y De Meulder, 2003](https://aclanthology.org/W03-0419/)), en [Hugging Face](https://huggingface.co/datasets/eriktks/conll2003).
- Biomédicos: ADE, DDI, DIANN (ver [[corpus#Dominio biomédico]]).

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 18.
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. [7](https://www.nltk.org/book/ch07.html).
- Tjong Kim Sang, E. F. (2002). Introduction to the CoNLL-2002 Shared Task: Language-Independent Named Entity Recognition. [ACL Anthology](https://aclanthology.org/W02-2024/)
- Tjong Kim Sang, E. F. y De Meulder, F. (2003). Introduction to the CoNLL-2003 Shared Task: Language-Independent Named Entity Recognition. [ACL Anthology](https://aclanthology.org/W03-0419/)
- Ratinov, L. y Roth, D. (2009). Design Challenges and Misconceptions in Named Entity Recognition. _CoNLL_. [ACL Anthology](https://aclanthology.org/W09-1119/)
- Mikolov, T. et al. (2013). Efficient Estimation of Word Representations in Vector Space. [arXiv:1301.3781](https://arxiv.org/abs/1301.3781)
- Pennington, J., Socher, R. y Manning, C. (2014). GloVe: Global Vectors for Word Representation. _EMNLP_. [ACL Anthology](https://aclanthology.org/D14-1162/)
- Hochreiter, S. y Schmidhuber, J. (1997). Long Short-Term Memory. _Neural Computation_, 9(8). [doi:10.1162/neco.1997.9.8.1735](https://doi.org/10.1162/neco.1997.9.8.1735)
- Collobert, R. et al. (2011). Natural Language Processing (Almost) from Scratch. _JMLR_, 12. [jmlr.org](https://jmlr.org/papers/v12/collobert11a.html)
- Huang, Z., Xu, W. y Yu, K. (2015). Bidirectional LSTM-CRF Models for Sequence Tagging. [arXiv:1508.01991](https://arxiv.org/abs/1508.01991)
- Lample, G. et al. (2016). Neural Architectures for Named Entity Recognition. _NAACL_. [ACL Anthology](https://aclanthology.org/N16-1030/)
- Vaswani, A. et al. (2017). Attention Is All You Need. _NeurIPS_. [arXiv:1706.03762](https://arxiv.org/abs/1706.03762)
- Devlin, J. et al. (2019). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. _NAACL_. [ACL Anthology](https://aclanthology.org/N19-1423/)
