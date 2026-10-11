# Corpus y campañas de evaluación

Un **corpus** es una compilación de textos en formato electrónico. Es el recurso básico del PLN:

- permite **evaluar** sistemas y **compararlos** en un marco común (mismos datos, mismas medidas, ver [[evaluacion-pln]]);
- permite **entrenar** sistemas supervisados con una parte del corpus;
- es una muestra amplia y natural del lenguaje y una fuente de estadísticas sobre palabras, relaciones y construcciones (etiquetado léxico, desambiguación sintáctica, extracción de gramáticas, traducción automática, extracción de entidades y relaciones).

## Tipos

- **Sin anotar**: texto plano.
- **Anotado**: marcado con información lingüística. Cuando la anotación es de referencia se llama _gold standard_.
- **Monolingüe** o **multilingüe**. Los multilingües pueden ser:
  - **paralelos**: los mismos textos en varias lenguas;
  - **comparables**: número equilibrado de textos por lengua, también equilibrados en tema y tipo.

## Ejemplos

### Generales

- **Brown** ([NLTK data](https://www.nltk.org/nltk_data/)): más de un millón de palabras de 15 temáticas, anotadas con 87 categorías léxicas (`AT` artículo, `NP` nombre propio, `VBD` verbo en pasado...). Formato `palabra/ETIQUETA`:
  `The/AT Fulton/NP County/NP Grand/NP Jury/NP said/VBD Friday/NR ...`
- **SUSANNE** ([grsampson.net](http://www.grsampson.net/Resources.html)): 130.000 palabras, 64 archivos del Brown, con categorías léxicas, lema y análisis sintáctico. Una línea por palabra:

  | ref.        | cat.  | palabra   | lema     | análisis      |
  | ----------- | ----- | --------- | -------- | ------------- |
  | N06:0180.12 | NN1u  | Baldness  | baldness | [S[Ns:s.Ns:s] |
  | N06:0180.15 | VBDZ  | was       | be       | [Vsu.         |
  | N06:0180.18 | VVGt  | attacking | attack   | .Vsu]         |

- **CESS-ESP**: corpus en español con etiquetas POS, disponible en NLTK (ver [[pos-tagging#Español con NLTK]]).
- **Europarl** ([statmt.org/europarl](https://www.statmt.org/europarl/)): actas del Parlamento Europeo en 21 lenguas, alineadas por oración (paralelo). [Koehn, 2005](https://aclanthology.org/2005.mtsummit-papers.11/).
- **CoNLL 2002/2003**: entidades nombradas (ver [[reconocimiento-de-entidades#Corpus]]).
- **TimeBank 1.2**: expresiones temporales y sucesos (ver [[expresiones-temporales-y-sucesos]]).

### Dominio biomédico

- **ADE** ([GitHub](https://github.com/trunghlt/AdverseDrugReaction)): relaciones medicamento–efecto adverso en informes médicos. [Gurulingappa et al., 2012](https://doi.org/10.1016/j.jbi.2012.04.008). Formato en [[extraccion-de-relaciones#Corpus ADE]].
- **BioScope** ([web](https://rgai.inf.u-szeged.hu/node/105)): textos biomédicos anotados con negación, especulación y su alcance. [Vincze et al., 2008](https://doi.org/10.1186/1471-2105-9-S11-S9).
- **DDI** ([web](https://hulat.inf.uc3m.es/DrugDDI/DrugDDI.html)): fármacos e interacciones entre ellos. [Herrero-Zazo et al., 2013](https://doi.org/10.1016/j.jbi.2013.07.011).
- **DIANN** ([GitHub](https://github.com/gildofabregat/DIANN-IBEREVAL-2018/tree/master/DIANN_CORPUS)): discapacidades anotadas. [Fabregat, Martínez-Romo y Araujo, 2018](https://ceur-ws.org/Vol-2150/overview-diann-task.pdf), IberEval@SEPLN.

## Campañas de evaluación

Proponen retos con datos y un marco de evaluación comunes:

- [TREC](https://trec.nist.gov/) (_Text REtrieval Conference_): recuperación de textos a gran escala.
- [CLEF](https://www.clef-initiative.eu/) (_Conference and Labs of the Evaluation Forum_): acceso a la información multilingüe y multimodal.
- [SemEval](https://semeval.github.io/) (_Semantic Evaluation_): análisis semántico.
- IberEval / [IberLEF](https://sites.google.com/view/iberlef-2025/): español y otras lenguas ibéricas.
- Históricas: MUC ([Chinchor, 1998](https://aclanthology.org/M98-1001/)), CoNLL _shared tasks_, ACE.
