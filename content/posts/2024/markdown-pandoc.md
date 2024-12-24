+++
title = 'Renderizando markdown con pandoc'
subtitle = 'Aprovechando la simplicidad de markdown y la potencia de $\LaTeX$'
date = 2024-12-24T12:40:00+01:00
draft = false
tags = ['Pandoc', 'Guía']
math = true
+++

LaTeX es un lenguaje de marcas muy versátil con el que se pueden componer muchos tipos de documentos, desde breves artículos de un par de páginas, hasta presentaciones o libros enteros. Permite incluír figuras, generar índices, citas y referencias y es muy utilizado para representar notación matemáticas. Por otro lado, MarkDown, también es un lenguaje de marchas, aunque mucho más sencillo y limitado. Este artículo explora la posibilidad de utilizar los dos en conjunto, tomando ventaja de la simplicidad de MarkDown para formatos básicos y permitiendo usar LaTeX para cosas más avanzadas en un único documento.

## Markdown

Markdown es muy sencillo y comúnmente utilizado, pero dejo aquí una lista sencilla de la sintaxis más común:

```markdown
# Título de nivel 1
## Título de nivel 2
### Título de nivel 3
#### Título de nivel 4
##### Título de nivel 5
**negrita**, *cursiva* y `bloque de código`

- Lista desordenada
- Con varios elementos

1. Lista ordenada
2. Con más de una entrada

```lenguaje
bloque de código```
```

Además, es habitual poder convertir las listas desordenadas en listas de tareas, utilizando `[ ]`, y marcando las tareas completadas con `[x]`, e incluír bloques de matemáticas de LaTeX entre dólares `$x$`, o dos dólares para bloques en líneas separadas. Si quieres ver un ejemplo más completo de la sintaxis de markdown, puedes ver el código fuente de [este artículo en github](https://github.com/delthia/blog.delthia.com/blob/main/content/posts/2024/markdown-pandoc.md), o consultar esta [hoja de referencia](https://www.markdownguide.org/cheat-sheet/).

Con todo esto, están cubiertos la mayor parte de los estilos que se suelen necesitar en documentos sencillos, así que sería muy útil poder convertir un archivo de markdown a un documento pdf.

## Pandoc

[Pandoc](https://pandoc.org) se describe como un *conversor de documentos universal*, una definición muy acertada. Este ejemplo se aprovecha de la manera en la que convierte markdown a pdf, como se explica en la siguiente sección.

Si simplemente queremos convertir un archivo de markdown a pdf, podemos utilizar el siguiente comando:

```fish
pandoc -f markdown <archivo.md> -o <archivo.pdf>
```

Esto creará un documento muy sencillo con el principal problema de que el margen es demasiado grande. Podemos modificar el formato del documento de salida con diferentes opciones de pandoc; estas son las que suelo utilizar yo:

```fish
pandoc -f markdown -V papersize=a4 -V geometry:margin=2cm --highlight-style tango <archivo.md> -o <archivo.pdf>
```

- `papersize=a4` hace que el formato del documento sea el de un A4; por defecto será *US Letter*
- `geometry:margin=2cm` establece los márgenes del documento. 2cm es un buen valor en comparación al original, que es demasiado grande
- `--highlight-style tango` establece el color con el que se resaltará el código en los bloques de código

Los valores de los parámetros `-V` son *variables* en pandoc, que permiten modificar el estilo del documento y sus propiedades. En el [manual de pandoc](https://pandoc.org/MANUAL.html) hay una [lista de todas las variables](https://pandoc.org/MANUAL.html#variables).

Además, es posible utilizar *front-matter* para indicar el estilo y otras características del documento. Esto resulta útil para indicar el título, autores y fecha del documento, por ejemplo:

```markdown
title: 'Título del documento'
author:
    - 'Autor 1'
    - 'Autor 2'
date: '24 diciembre 2024'
```

## LaTeX

Lo que hace que esto sea aún más potente es que pandoc, antes de renderizar el documento, convierte el texto en markdown a LaTeX, lo que significa que podemos incluír cualquier instrucción de LaTeX en el medio del markdown y se interpretará correctamente, las más útiles para mí son:

- `\newpage`: Insertar un salto de página
- `\underline{<texto subrayado}`: Texto subrayado
- `\color{<color>}{<texto>}`: Texto de colores
