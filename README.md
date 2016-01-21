# Trabajando con datos desde la línea de comandos

Original de [https://github.com/rgrp](Rufus Pollock) [https://github.com/rgrp/command-line-data-wrangling](Command Line Data Wrangling)

Algunos comandos de Unix que pueden utilizarse para trabajar con datos en la shell.

Se centra en datos guardados en texto plano y CSV, aunque no tiene que utilizar obligatoriamente la *coma* como delimitador exclusivamente.

## Herramientas

* cut = para filtrar columnas
* sed = replace (and much more)
* grep = filter rows
* sort = sort!
* uniq = count duplicate (with sort = crude group by)
* paste = join 2 files (line by line)
* wc = count lines or "words"
* split = split a file into pieces (less useful)

La principal limitación de la mayoría de las utilidades para CSV es que:

- Se orientan a líneas
- Se limitan a un enfoque bastante ingenuo de delimitadores

Si los CSVs están bien formados y:

- No incluyen finalizadores de línea entre campos, algo que se permite en CSV
- No incluyen delimitadores en los valores, lo cual se permite si está entrecomillado

Entonces no hay problemas al usar estas herramientas.

Si eso no es así, podrás usar las herramientas pero tendrás que ser cuidadoso al obtener los resultados.


## Conceptos clave

Tuberías, una de las más grandes ideas del diseño de programación de todos los tiempos

> 1. We should have some ways of coupling programs like garden hose--screw in
> another segment when it becomes when it becomes necessary to massage data in
> another way.  This is the way of IO also. [Doug McIlroy, 1964] ([source](http://doc.cat-v.org/unix/pipes/))

Basic point: the unix shell (and all of the above commands) have great support
for feeding the output of one command into the input of another and doing so in
a streaming "pipe-like" manner.

Example:

    head -n20 file.txt | tail -n5 | cut -c1-10

This take first 20 lines of file.txt pipes that into tail which takes first 5
lines of that pipes that into cut which take characters 1-10 of each line. The
result: characters 1-10 of lines 15-20.


## Examples

### Delete lines at Start or End

If continuous lines at top and bottom of the file use head or tail.

Delete first line:

    tail -n +2 {file}

Delete last line:

    head -n +2 {file}

### Delete Lines Generally

Delete the nth line:

    sed nd {file}

Delete a range of lines (n-m):

    sed n,md {file}

Multiple deletes (first, third, n-m)

    sed 1;3;n,md {file}

### Sort

Sort a CSV file by 2nd, 1st and 3rd columns.

    sort --field-separator=',' --key=2,1,3 {file}

### Group By

