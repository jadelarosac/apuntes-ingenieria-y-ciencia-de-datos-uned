# Lenguaje de programación R

Parece que en la asignatura se usa de forma básica.

Puntos importantes que recordar:

- Es un lenguaje bastante _vector_-oriented. Es decir, va a ser más eficiente hacer cálculos vectorizados que iterar sobre los datos. Usarlo como una hoja de excel abstracta (burrada, pero transmite la idea).
- Es sencillo. Todo es un vector. Tipos de datos básicos, vectores de longitud 1. Todo se basa en esto esencialmente. Otras estructuras de datos (aunque implementadas con vectores):
  - listas: vector heterogéneo atributos con nombre
  - arrays: vector con atributo dimensión (vector con número de filas y columnas). Si la longitud de la dimensión es 2, son matrices.
  - Objetos: listas con ciertos atributos y restricciones. Ejemplo:
    - _data-frames_: listas cuyos vectores de datos tienen todos la misma longitud.
- Si alguna vez es necesario (_spoiler_, no lo va a ser, en el curso no se da R en profundidad) todo esto de las estructuras de datos cobra vital importancia para escribir librerías de C/C++/Fortran/Rust/¿Julia?. No obstante, tenerlo en mente ayuda a resolver problemas de forma idiomática/eficiente.
