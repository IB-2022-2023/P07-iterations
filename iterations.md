# Práctica 07. Iteraciones

# Factor de ponderación: 6

### Objetivos
Los objetivos de esta práctica son que el alumnado:
* 

### Rúbrica de evaluacion de esta práctica
Se señalan a continuación los aspectos más relevantes (la lista no es exhaustiva) que se tendrán en cuenta a la hora de evaluar esta práctica.
Se comprobará que el alumnado:
* Es capaz de escribir programas simples en C++ que resuelvan problemas de
  complejidad similar a los que se han propuesto para esta práctica
* Ha automatizado la compilación de sus programas usando un fichero `Makefile`
  para cada uno de los programas que desarrolle 
* Acredita que todas las prácticas realizadas hasta la fecha se encuentran alojadas en repositorios
  privados de [GitHub](https://github.com/).
* Acredita que es capaz de subir programas a la plataforma 
[Jutge](https://jutge.org/)
para su evaluación
* Ha incluido un comentario prólogo en todos los ficheros (`*.cc`, `*.h`) de sus ejercicios
* Hace que todos los programas de sus prácticas se ajusten a la
[Guía de estilo de Google para C++](https://google.github.io/styleguide/cppguide.html) 
* Acredita que es capaz de editar ficheros remotos en su VM usando vi
* Demuestra que es capaz de ejecutar comandos Linux en su VM

### Documentación de código. Doxygen
XXX Intro

[Doxygen](https://en.wikipedia.org/wiki/Doxygen) es una herramienta de código abierto que permite generar documentación de referencia para proyectos de desarrollo software. 
Una ventaja de Doxygen es que la documentación está escrita en el propio código fuente de los programas, y por lo tanto es relativamente fácil de mantener actualizada. 
Doxygen puede hacer referencias cruzadas entre la documentación y el código, de modo que el lector de un documento puede referirse fácilmente al código fuente.
La herramienta extrae la documentación de los comentarios presentes en los ficheros de código fuente y puede generar la salida en diferentes formatos entre los cuales están HTML, PDF, LaTeX o páginas `man` de Unix.

En esta asignatura no se propone un uso exhaustivo de Doxygen pero sí se promueve que la
documentación de los programas desarrollados se realice en el formato reconocido por Doxygen, que se ha
convertido en un estándar de facto.

Comience por instalar Doxygen en su máquina virtual de la asignatura:
```
$ sudo apt install doxygen
```
Instale también los siguientes paquetes:
```
$ sudo apt install texlive-latex-base
$ sudo apt install texlive-latex-recommended
$ sudo apt install texlive-latex-extra
```
Estos paquetes son necesarios para compilar ficheros en formato [LaTeX](https://es.wikipedia.org/wiki/LaTeX).
Más adelante en este documento se justifica la necesidad de los programas que suministran estos paquetes.

En el [manual de Doxygen](https://www.doxygen.nl/manual/starting.html) se indica cómo comenzar a trabajar con la herramienta.
Si, ubicados en un directorio de trabajo, se invoca `doxygen`:
```
doxygen -g <config-file>
```

la herramienta creará un fichero de configuración.
Si no se le pasa el nombre del fichero como parámetro, creará un fichero con nombre `Doxyfile` preconfigurado para su uso.
En el directorio de trabajo de esta práctica (`src`) se encuentra un fichero `Doxyfile` ya listo para usarse con proyectos de C++.
Se ha incluído asimismo el código fuente de un programa para ilustrar con el mismo el uso de documentación con Doxygen.
Si revisa el fichero `Doxyfile` (es un fichero de texto) verá un conjunto de opciones que el programa permite.
Cada opción va precedida de una explicación de su finalidad y funcionamiento, de modo que puede probar a modificar algunas de ellas si lo desea.
En [esta página](https://www.doxygen.nl/manual/config.html) puede consultarse la finalidad y funcionamiento de cada una de las etiquetas (tags) que se usan en el fichero de configuración de Doxygen.

Para generar la documentación de su aplicación, colóquese en el directorio de su proyecto (`src` en esta práctica) y ejecute:
```
doxygen Doxyfile
```

Con el fichero `Doxyfile` que se suministra, la herramienta creará un subdirectorio `doc` en el directorio raíz de su proyecto en el que alojará toda la documentación generada.
El directorio donde Doxygen genera su salida se especifica con la etiqueta `OUTPUT_DIRECTORY` (línea `61` del fichero `Doxyfile` suministrado).
Con la configuración suministrada se generan 2 subdirectorios dentro de `doc`: `html` y `latex`.
Si abre con un navegador el fichero `doc/html/index.html` accederá a la página principal de la documentación generada para el programa.
Si se coloca en el directorio `doc/latex/` y ejecuta `make` el sistema "compila" el código LaTeX y genera un fichero `refman.pdf` que contiene igualmente la documentación generada.

Tal como se ha indicado, HTML o LaTeX son solo 2 de los formatos que permite generar Doxygen.
Tanto HTML como LaTeX (también [Markdown](https://es.wikipedia.org/wiki/Markdown)) son lo que se conoce como [lenguajes de marcas](https://es.wikipedia.org/wiki/Lenguaje_de_marcado).
HTML es el lenguaje que se utiliza para componer los textos que se muestran en las páginas web.
[Latex](https://en.wikipedia.org/wiki/LaTeX) es un sistema de composición de textos que cuida el formato en especial en el ámbito de la tipografía y que es especialmente adecuado para textos de carácter científico.
No se pretende aquí que profundice en conocer HTML o LaTeX.

La sección [Documenting the code](https://www.doxygen.nl/manual/config.html) del manual de Doxygen indica cómo comentar el código fuente de modo que los comentarios sean procesados por Doxygen para incorporarlos a la documentación generada.

La guía [Documenting C++ Code](https://developer.lsst.io/cpp/api-docs.html) de documentación de código del proyecto LLST es la referencia que se adoptará en la asignatura para documentar el código de los programas que se desarrollen.
Se utilizarán comentarios de tipo JavaDoc para comentarios de bloque:

```
/**
 * ... text ...
 */
```
[JavaDoc](https://en.wikipedia.org/wiki/Javadoc) es otro sistema de documentación ideado para Java y que también es muy popular. 
Doxygen soporta el uso de etiquetas "al estilo Javadoc" en el código.

Los bloques de comentarios multi-línea deben comenzar con 
```
/** 
```
y finalizar con
```
*/
```
Los comentarios de una única línea deben comenzar con `///`.
Por consistencia no use las opciones 
```
/*!
```
o
```
//!
```
permitidas en Doxygen.

Así el bloque de comentarios que debe preceder a cualquier función (o método) tendrá una apariencia similar a esta:
```
/**
 * Sum numbers in a vector.
 *
 * @param values Container whose values are summed.
 * @return sum of `values`, or 0.0 if `values` is empty.
 */
double sum(std::vector<double>& const values) {
  ...
}
```
En el ejemplo anterior `@param` y `@return` son etiquetas de tipo Javadoc.
En [Overview of supported JavaDoc style tags](http://www.time2help.com/doc/online_help/idh_java_doc_tag_support.htm) pueden consultarse este tipo de etiquetas.

El siguiente es un ejemplo (plantilla) de comentario de bloque que debería incluirse al comienzo de todos los ficheros (`*.cc`, `*.h`) de un proyecto de programación en el ámbito de esta asignatura:
```
/**
  * Universidad de La Laguna
  * Escuela Superior de Ingeniería y Tecnología
  * Grado en Ingeniería Informática
  * Informática Básica
  *
  * @author F. de Sande fsande@ull.es
  * @date 01 Dic 2021
  * @brief El programa calcula la suma de todos los términos de valor par de la serie
  *        de Fibonacci que sean menores que un valor dado.
  *        Cada nuevo término de la serie se genera sumando los dos anteriores.
  *        Comenzando con 0 y 1, los primeros 10 términos serán: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
  * @bug No hay bugs conocidos
  * @see https://www.cs.cmu.edu/~410/doc/doxygen.html
  */
```
Todo fichero debiera contener (etiqueta `@brief`) una breve descripción del contenido del fichero.
Si fuera necesario se incluirá a continuación una descripción más detallada.
Obviamente el comentario específico debiera particularizarse para cada caso concreto.

Por otra parte, estudie atentamente todo lo que se indica en el epígrafe [Comments](https://google.github.io/styleguide/cppguide.html#Comments) de la Guía de Estilo de Google y ponga en práctica todo lo que en ella se propone, usando el formato Doxygen para todos los comentarios que introduzca en su código fuente.

### Material de estudio complementario
Estudie del
[tutorial de referencia](https://www.learncpp.com/)
en la asignatura los siguientes apartados:
XXX

### Ejercicios
* Escriba programas que solucionen los siguientes problemas y evalúe su solución utilizando Jutge.
* Al realizar los ejercicios cree dentro de su repositorio de esta práctica un directorio diferente
para cada uno de los ejercicios.
Asigne a cada uno de esos directorios nombres significativos. Por ejemplo `P34279_add-one-second` para el
tercer ejercicio.
* Automatice la compilación del programa correspondiente a cada ejercicio con un fichero `Makefile`
independiente para cada programa y que ha de incluir en el correspondiente directorio.
* Recuerde que Jutge solo evalúa la corrección de su programa desde un punto de vista de su correcto funcionamiento.
Su código ha de cumplir adicionalmente con los requisitos de modularidad, formato y estilo.

1. [P98960](https://jutge.org/problems/P98960_en) Uppercase and lowercase letters
2. [P90615](https://jutge.org/problems/P90615_en) Maximum of three integer numbers
3. [P34279](https://jutge.org/problems/P34279_en) Add one second
4. [P97156](https://jutge.org/problems/P97156_en) Numbers in an interval
5. [P59539](https://jutge.org/problems/P59539_en) Harmonic numbers (1)
