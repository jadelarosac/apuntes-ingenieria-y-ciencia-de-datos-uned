# Extracción de relaciones

Identificar relaciones entre entidades ya reconocidas ([[reconocimiento-de-entidades]]). Formalmente, una relación es un conjunto de tuplas ordenadas sobre los elementos de un dominio.

Ejemplo: _"La UNED es la mayor universidad de España, con más de 250.000 estudiantes..."_ da UNED _es-una_ universidad, UNED _localización_ España, UNED _núm. estudiantes_ > 250.000.

## Tipos de relaciones

- Relaciones de la tarea de extracción de relaciones de **ACE** ([ACE 2005](https://catalog.ldc.upenn.edu/LDC2006T06)):

| Relación              | Tipos   | Ejemplo                           |
| --------------------- | ------- | --------------------------------- |
| Physical-Located      | PER–GPE | _He was in Tennessee_             |
| Part-Whole-Subsidiary | ORG–ORG | _XYZ, the parent company of ABC_  |
| Person-Social-Family  | PER–PER | _Yoko's husband John_             |
| Org-AFF-Founder       | PER–ORG | _Steve Jobs, co-founder of Apple_ |

- Dominio biomédico: [UMLS](https://www.nlm.nih.gov/research/umls/index.html) (_Unified Medical Language System_, de la Biblioteca Nacional de Medicina de EE. UU.) define 134 tipos de entidad y 54 relaciones: _Injury_ **disrupts** _Physiological Function_, _Pharmacologic Substance_ **treats** / **causes** _Pathologic Function_, etc.
- Otras fuentes de relaciones: las **infoboxes** de Wikipedia (Stanford: `state = California`), que pueden pasarse a tripletas [RDF](https://www.w3.org/RDF/) (entidad–relación–entidad), y ontologías como WordNet (relación _is-a_, ver [[desambiguacion-del-sentido#Relaciones entre sentidos]]).

## Corpus ADE

Relaciones medicamento → efecto adverso ([Gurulingappa et al., 2012](https://doi.org/10.1016/j.jbi.2012.04.008); ver [[corpus]]). Columnas: PubMed-ID | oración | efecto adverso | inicio | fin | medicamento | inicio | fin (desplazamientos a nivel de documento):

```
10030778|Intravenous azithromycin-induced ototoxicity.|ototoxicity|43|54|azithromycin|22|34
```

## Técnicas

### Patrones léxico-sintácticos

Ejemplo clásico de [Hearst (1992)](https://aclanthology.org/C92-2082/) para la hiponimia (_is-a_): en _"red algae, such as Gelidium"_ se obtiene Gelidium _is-a_ red algae. Patrones (X hipónimo, Y hiperónimo):

- _Y such as X_, _Y, such as X_
- _X or other Y_, _X and other Y_
- _Y including X_

| Patrones | Ventajas | Inconvenientes |
| --- | --- | --- |
| Hechos a mano | alta precisión, adaptables a un dominio | baja cobertura, mucho esfuerzo |
| Aprendidos | aplicables a nuevos dominios, sin diseño manual | necesitan ejemplos anotados |

### Supervisada

1. Elegir las entidades y relaciones de interés.
2. Conseguir un corpus representativo con entidades y relaciones anotadas, y partirlo en entrenamiento/desarrollo/test (o usar validación cruzada, ver [[evaluacion-pln]]).
3. Entrenar, ajustar y evaluar.

Se plantea como **clasificación** en uno o dos pasos: ¿están relacionadas las dos entidades? y ¿con qué tipo de relación? Rasgos típicos para el par $M_1$ = _American Airlines_ y $M_2$ = _Tim Wagner_ en _"American Airlines, a unit of AMR Corp., immediately matched the move, spokesman Tim Wagner said"_:

- núcleos de las entidades: _Airlines_, _Wagner_;
- bolsa de palabras y n-gramas de las entidades;
- palabras en posiciones concretas: $M_2-1$ = _spokesman_, $M_2+1$ = _said_;
- bolsa de palabras o bigramas entre las entidades;
- las mismas variantes con _stems_ ([[stemming-y-lematizacion]]).

También puede plantearse como **clasificación de secuencias** (anotar el texto con etiquetas de salida y leerlo de izquierda a derecha).

### Semisupervisada (bootstrapping)

Anotar mucho texto a mano es caro. Se parte de unos pocos **patrones o tuplas semilla** muy fiables (DIPRE, Brin 1998; Snowball, [Agichtein y Gravano, 2000](https://doi.org/10.1145/336597.336644)):

```
function BOOTSTRAP(relación R):
    tuplas ← semillas con relación R           # p. ej. <Mark Twain, Elmira>
    repetir:
        oraciones ← oraciones que contienen las entidades de alguna tupla
        patrones  ← generalizar el contexto entre y alrededor de las entidades
                    # "X is buried in Y", "the grave of X is in Y"
        nuevas    ← tuplas encontradas con esos patrones, con confianza alta
        tuplas    ← tuplas + nuevas
    devolver tuplas
```

Riesgo: **deriva semántica** (_semantic drift_). Un patrón erróneo añade tuplas erróneas, que a su vez generan más patrones erróneos. Para evitarlo se asigna una **confianza** a cada patrón, equilibrando su rendimiento sobre las tuplas actuales y su productividad (cuántas coincidencias da en la colección), y se usan umbrales conservadores.

### Supervisión distante

En lugar de pocas semillas, se toma una **base de datos** con muchas tuplas, sin validar ([Mintz et al., 2009](https://aclanthology.org/P09-1113/)):

1. Para cada tupla $(e_1, e_2)$ con relación $R$ (p. ej. _place-of-birth_: <Albert Einstein, Ulm>), buscar todas las oraciones con $e_1$ y $e_2$ en un gran corpus procesado con NER.
2. Extraer rasgos léxicos y sintácticos frecuentes (_PER was born in LOC_, _PER's birthplace in LOC_...). Cada tupla acumula rasgos de muchas oraciones.
3. Entrenar un clasificador supervisado con esas observaciones.

### No supervisada (Open IE)

La **extracción abierta de información** (_Open Information Extraction_) no usa datos de entrenamiento ni una lista de relaciones ([Banko et al., 2007](https://www.ijcai.org/Proceedings/07/Papers/429.pdf)). Extrae como relaciones las secuencias verbales entre sintagmas nominales que cumplen ciertas restricciones léxicas y sintácticas, y descarta las poco frecuentes (p. ej. ReVerb, [Fader et al., 2011](https://aclanthology.org/D11-1142/)).

## Evaluación

- Con corpus anotado: precisión, cobertura y F ([[evaluacion-pln]]).
- En métodos semisupervisados y no supervisados las relaciones son nuevas y **no hay _gold standard_**:
  - la precisión se **estima** revisando a mano una muestra aleatoria, también por niveles (las 1.000, 10.000, 100.000 relaciones de más peso...);
  - la cobertura **no se puede calcular**.

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 21.
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. [7](https://www.nltk.org/book/ch07.html).
- Hearst, M. A. (1992). Automatic Acquisition of Hyponyms from Large Text Corpora. _COLING_. [ACL Anthology](https://aclanthology.org/C92-2082/)
- Brin, S. (1998). Extracting Patterns and Relations from the World Wide Web. _WebDB_.
- Agichtein, E. y Gravano, L. (2000). Snowball: Extracting Relations from Large Plain-Text Collections. _ACM DL_. [doi:10.1145/336597.336644](https://doi.org/10.1145/336597.336644)
- Mintz, M. et al. (2009). Distant supervision for relation extraction without labeled data. _ACL-IJCNLP_. [ACL Anthology](https://aclanthology.org/P09-1113/)
- Banko, M. et al. (2007). Open Information Extraction from the Web. _IJCAI_. [PDF](https://www.ijcai.org/Proceedings/07/Papers/429.pdf)
- Fader, A., Soderland, S. y Etzioni, O. (2011). Identifying Relations for Open Information Extraction. _EMNLP_. [ACL Anthology](https://aclanthology.org/D11-1142/)
