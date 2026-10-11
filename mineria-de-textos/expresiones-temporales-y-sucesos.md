# Expresiones temporales y sucesos

Tareas de [[extraccion-de-informacion]] que sitúan los hechos de un texto en el tiempo. Son especialmente útiles en búsqueda de respuestas y generación de resúmenes.

## Expresiones temporales

Hay que **reconocerlas** y **normalizarlas** (_"3 de la tarde"_, _"mañana"_, _"desde ayer"_, _"dos veces al mes"_). Tipos:

- **absolutas**: fecha u hora concreta (_April 24, 1916_; _10:15 AM_; _the 3rd quarter of 2006_);
- **relativas**: referidas a otro momento (_yesterday_, _next semester_, _last quarter_);
- **duraciones**: periodos de tiempo (_four hours_, _six days_, _the last three quarters_).

Su núcleo es un **disparador léxico**:

- nombres (_morning_, _winter_);
- nombres propios (_January_, _Monday_, _Easter_);
- adjetivos (_recent_, _annual_);
- adverbios (_daily_, _monthly_).

### Anotación TimeML

[TimeML](https://timeml.github.io/site/) marca las expresiones con la etiqueta `<TIMEX3>` y sus atributos. **TimeBank 1.2** ([LDC2006T08](https://catalog.ldc.upenn.edu/LDC2006T08)) es un corpus anotado con este esquema:

`A fare increase initiated <TIMEX3>last week</TIMEX3> by UAL Corp's United Airlines was matched by competitors over <TIMEX3>the weekend</TIMEX3>...`

### Reconocimiento

- **Reglas** sobre los disparadores.
- **Etiquetado de secuencias** con IOB, igual que en [[reconocimiento-de-entidades#Codificación de las clases]]:

  ```
  A fare increase initiated last week by UAL Corp's ...
  O O    O        O         B    I    O  O   O
  ```

  Rasgos: el token y su ventana de tokens, la forma (mayúsculas, longitud), las etiquetas POS y de _chunk_ del token y de la ventana, y la presencia en la lista de disparadores.

## Normalización temporal

Hacer corresponder la expresión con un **punto en el tiempo** (día del calendario y/o hora) o una **duración**. El valor se guarda en el atributo `value` en formato [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html).

Las fechas completas (año, mes y día) son raras. La mayoría se interpretan respecto al **ancla temporal** (_temporal anchor_) del documento, normalmente su fecha de creación.

| Unidad | Patrón | Ejemplo |
| --- | --- | --- |
| Fecha completa | `YYYY-MM-DD` | `1991-09-28` |
| Semana | `YYYY-Wnn` | `2007-W27` |
| Fin de semana | `PnWE` | `P1WE` |
| Hora (24 h) | `HH:MM:SS` | `11:13:45` |
| Fecha y hora | `YYYY-MM-DDTHH:MM:SS` | `1991-09-28T11:00:00` |
| Trimestre | `YYYY-Qn` | `1999-Q3` |

Las **duraciones** siguen el patrón `Pnx` ($n$ = longitud, $x$ = unidad): `P3Y` son tres años y `P2D` dos días.

### Ejemplo

```xml
<TIMEX3 id="t1" type="DATE" value="2007-07-02" functionInDocument="CREATION_TIME">July 2, 2007</TIMEX3>
A fare increase initiated <TIMEX3 id="t2" type="DATE" value="2007-W26" anchorTimeID="t1">last week</TIMEX3>
by United Airlines was matched by competitors over
<TIMEX3 id="t3" type="DURATION" value="P1WE" anchorTimeID="t1">the weekend</TIMEX3>, marking the second
successful fare increase in <TIMEX3 id="t4" type="DURATION" value="P2W" anchorTimeID="t1">two weeks</TIMEX3>.
```

- **Ancla**: la fecha del documento, `2007-07-02`.
- **last week** → `2007-W26`. Las semanas ISO van de la 01 a la 53, empiezan en lunes y la primera del año es la que contiene el primer jueves. El 2 de julio de 2007 cae en la semana 27, así que la anterior es la 26.
- **the weekend** → `P1WE`. Los fines de semana se tratan como duraciones. Como cae entero dentro de una semana ISO, `anchorTimeID` lo sitúa en una semana concreta.
- **two weeks** → `P2W` (duración).

## Sucesos

Un **suceso** (_event_) es algo que ocurre en un punto del tiempo y del espacio. Extraerlo es obtener su nombre, tipo, agente, tiempo y lugar:

_"[EVENT Citing] high fuel prices, United Airlines [EVENT said] Friday it has [EVENT increased] fares by \$6..."_

- Tipos: acciones, estados, de notificación (_dice_, _explica_), de percepción...
- Se suele abordar con **aprendizaje supervisado** (etiquetado IO), con rasgos como prefijos y sufijos, etiqueta POS ([[pos-tagging]]) o tipo de verbo.

### Ordenación temporal

Colocar sucesos y expresiones temporales en una línea de tiempo. Una versión más sencilla es la **ordenación parcial** con las relaciones de intervalos de [Allen (1983)](https://doi.org/10.1145/182.358434): _before_, _after_, _meets_, _overlaps_, _starts_, _finishes_, _during_, _equal_. TimeBank anota la mayor parte de esta información.

Los sucesos y sus participantes alimentan el [[extraccion-de-informacion#Rellenado de plantillas|rellenado de plantillas]].

## Referencias

- Jurafsky y Martin, [SLP3](https://web.stanford.edu/~jurafsky/slp3/), cap. 21.
- Pustejovsky, J. et al. (2003). TimeML: Robust Specification of Event and Temporal Expressions in Text. _IWCS-5_. [timeml.github.io](https://timeml.github.io/site/)
- Pustejovsky, J. et al. (2006). _TimeBank 1.2_. Linguistic Data Consortium, [LDC2006T08](https://catalog.ldc.upenn.edu/LDC2006T08).
- Allen, J. F. (1983). Maintaining knowledge about temporal intervals. _Communications of the ACM_, 26(11). [doi:10.1145/182.358434](https://doi.org/10.1145/182.358434)
- [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html): formato de fechas y horas.
