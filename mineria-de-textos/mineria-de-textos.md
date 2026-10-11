# Minería de textos

La minería de textos (_text mining_) es el proceso de analizar colecciones de textos no estructurados, escritos en lenguaje natural, para descubrir información y patrones que no aparecen de forma explícita en ellos. Combina técnicas de procesamiento del lenguaje natural (PLN) y de aprendizaje automático.

Los textos se agrupan en colecciones o [[corpus]], que pueden incluir anotaciones y metadatos.

## Tareas principales

- **Extracción de información** (_Information Extraction_, EI): obtener automáticamente información estructurada a partir de textos. Incluye el reconocimiento de entidades (NER), la extracción de terminología y la de relaciones. Ver [[extraccion-de-informacion]].
- **Recuperación de información** (_Information Retrieval_, IR): buscar documentos, información dentro de ellos o metadatos que los describan, en cualquier repositorio (bases de datos, la web...).
- **Categorización de documentos**: asignar a un documento una o más categorías, definidas previamente, según su contenido (clasificación supervisada).
- **Agrupamiento de documentos** (_Document Clustering_): organizar documentos en grupos sin conocer de antemano su naturaleza ni, a veces, su número (no supervisado).

## Procesamiento del lenguaje natural

El PLN es la rama de la informática que desarrolla sistemas capaces de comunicarse con personas usando el lenguaje humano. En minería de textos sirve para adquirir conocimiento de grandes volúmenes de texto (p. ej. contrastar hipótesis con informes médicos, generar resúmenes, sistemas de diálogo).

### ¿Por qué es difícil?

- **Ambigüedad** en todos los niveles:
  - léxica: _"Estuvo esperando en el banco"_ (¿asiento o entidad financiera?), ver [[desambiguacion-del-sentido]];
  - sintáctica: _"No los aceptaron en el partido por sus prejuicios"_ (¿de quién son los prejuicios?), ver [[analisis-sintactico]];
  - semántica y de discurso.
- **Conocimiento del mundo**: en _"esperé en el banco para sacar dinero"_ sabemos que es la entidad financiera.
- **Negación** y **especulación**: hay que saber si un dato se niega o solo "puede" darse (ver el corpus BioScope en [[corpus#Dominio biomédico]]).
- **Correferencia**: identificar expresiones que se refieren a la misma entidad (_"vieron al Presidente... reconoció al ganador de las elecciones"_).

## Cadena de procesamiento

Procesos de PLN que se usan como base de la minería de textos:

1. Preprocesamiento: [[normalizacion-de-textos]] (expresiones regulares, tokenización, palabras vacías, distancia de edición).
2. [[stemming-y-lematizacion]] (análisis morfológico).
3. [[pos-tagging]] (etiquetado gramatical).
4. [[analisis-sintactico]] (_chunking_, constituyentes, dependencias).
5. [[desambiguacion-del-sentido]] (WSD).
6. [[extraccion-de-informacion]]: [[reconocimiento-de-entidades]], [[extraccion-de-relaciones]], [[expresiones-temporales-y-sucesos]].

Todo sistema se compara con otros sobre [[corpus]] comunes y con las medidas de [[evaluacion-pln]].

## Herramientas

- [NLTK](https://www.nltk.org/) y [spaCy](https://spacy.io/): librerías de Python usadas en la asignatura. Ver [[nltk-spacy]].
- Otras: [Stanford NLP](https://nlp.stanford.edu/software/), [TreeTagger](https://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/), [FreeLing](https://nlp.lsi.upc.edu/freeling/).

## Referencias

- Jurafsky, D. y Martin, J. H. _Speech and Language Processing_ (3.ª ed., borrador). [web.stanford.edu/~jurafsky/slp3](https://web.stanford.edu/~jurafsky/slp3/). Caps. 2 (_Words and Tokens_), 18 (_Sequence Labeling for Parts of Speech and Named Entities_), 19–20 (análisis sintáctico), 21 (_Information Extraction_) y apéndice I (_Word Senses and WordNet_, solo web).
- Bird, S., Klein, E. y Loper, E. (2009). _Natural Language Processing with Python_. O'Reilly. [nltk.org/book](https://www.nltk.org/book/). Caps. 2, 3, 5, 7 y 8.
