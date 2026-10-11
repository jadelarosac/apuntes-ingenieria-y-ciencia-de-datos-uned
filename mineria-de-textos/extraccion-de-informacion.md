# Extracción de información

La extracción de información (EI) convierte la información **no estructurada** de los textos en **datos estructurados**: detecta las partes relevantes del texto y las combina según sus relaciones.

Ejemplo (informe de urgencias): _"Paciente varón de 86 años con antecedentes de TBC pulmonar, refiere F no cuantificada desde hace dos semanas, así como tos esporádica productiva y pérdida de peso."_

| Campo | Valor |
| --- | --- |
| Sexo | hombre |
| Edad | 86 |
| Antecedentes | tuberculosis pulmonar |
| Síntomas | fiebre no cuantificada, tos esporádica productiva, pérdida de peso |
| Tiempo | dos semanas |

## Tareas

- **Reconocimiento de entidades** (NER): detectar el segmento de la entidad y clasificarlo (_Tim Wagner_ → PERSONA). Ver [[reconocimiento-de-entidades]].
- **Extracción de relaciones** entre entidades (_Steve Jobs_ co-fundador de _Apple_: PER–ORG). Ver [[extraccion-de-relaciones]].
- **Reconocimiento y normalización de expresiones temporales** y **extracción de sucesos**. Ver [[expresiones-temporales-y-sucesos]].
- **Rellenado de plantillas**, que integra todo lo anterior.

Todas se apoyan en el preprocesamiento de PLN ([[normalizacion-de-textos]], [[pos-tagging]], [[analisis-sintactico#Chunking]]) y se evalúan con precisión, cobertura y F ([[evaluacion-pln]]).

## Rellenado de plantillas

Organizar la información sobre sucesos en estructuras (_scripts_) permite hacer inferencias. Una **plantilla** es un _script_ sencillo con un conjunto fijo de huecos (_slots_):

```
FARE-RAISE-ATTEMPT:
  LEAD AIRLINE:   UNITED AIRLINES
  AMOUNT:         $6
  EFFECTIVE DATE: 2006-10-26
  FOLLOWER:       AMERICAN AIRLINES
```

Se suele modelar con dos sistemas supervisados:

1. **Reconocimiento de plantillas**: decidir si el texto describe un suceso de ese tipo.
2. **Extracción de rellenos de rol** (_role-filler extraction_): clasificar qué rol (`LEAD AIRLINE`, `AMOUNT`...) cumple cada entidad.

Fue la tarea central de las conferencias MUC ([Chinchor, 1998](https://aclanthology.org/M98-1001/)).

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 21 (_Information Extraction: Relations, Events, and Time_).
- Bird, Klein y Loper, [NLTK Book](https://www.nltk.org/book/), cap. [7](https://www.nltk.org/book/ch07.html) (_Extracting Information from Text_).
- Chinchor, N. A. (1998). Overview of MUC-7. _Seventh Message Understanding Conference_. [ACL Anthology](https://aclanthology.org/M98-1001/)
