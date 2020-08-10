<!--
    Tip 72: Ajustar el case sensitive para búsquedas individuales  
-->

```:set ignorecase:``` <!-- No distingue entre mayúsculas y minúsculas -->
/humanidad              <!--  Buscar la palabra (¡Todas coinciden!) -->
/\Chumanidad            <!-- \C obliga a buscar con mayúsculas y minúsculas --> 
/humanidad\C            <!-- Se puede usar también al final de la búsqueda  -->

Humanidad
humanidad
HumaNIdad
HuMaNiDaD

# :set noignorecase: Ahora distingue entre mayúsculas y minúsculas (default de vim) 

# /familia   - Busca la palabra (solo una coincide)
# /\cfamilia - Desactiva el case sensitive en la búsqueda (¡todos coinciden!)
# /familia\c - Se puede usar al final de la búsqueda

familia
Familia
FAmIlIa
fAMILIa

# :set smartcase: Si el patrón de búsqueda es todo en minúscula, la búsqueda 
# no será case sensitive, pero apenas detecte una mayúscula, se volverá case
# sensitive
saltamontes
Saltamontes
SALTAMontes

#--------- Tip 73: Usar el patrón de búsqueda \v para búsquedas con expresiones regulares
body   { color: #3c3c3c; }
a      { color: #0000EE; }
strong { color: #000; }

# Comando: /#\([0-9a-fA-F]\{6}\|[0-9a-fA-F]\{3}\)

# Expresión regultar: #\([0-9a-fA-F]\{6}\|[0-9a-fA-F]\{3}\)
# Match con un #, seguido de tres o seis caracteres hexagecimales.
# Eso incluye todos los números además de letras de la A a la F, mayúsculas 
# minúsculas. Tiene 5 backslashes! 


#([0-9a-fA-F]{6}|[0-9a-fA-F]{3}) <--- Expresión regular normal

#Comando: /\v#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})

# \v: very magic search: Hace que VIM lea las expresiones regulares como otros
# lenguajes. TRata todos los caracteres como especiales, menos underscore, letras
# y números.

# \x <---- reemplaza [0-9a-fA-F]
# Comando final: \v#(\x{6}|\x{3})

#------------ Tip 74: \V para búsquedas literales


The N key searches backward...
...the \v pattern switch ( very magic search)...

# Buscar "a.k.a": / ¿Funciona?
# /a\.k\.a\. o /\V

# \V verynomagic: sólo los backslash tienen un significado especial
# No recomendado para expresiones regulares

# ------------- Tip 75 : Usar paréntesis para capturar subcoincidencias


I love Paris in the
the springtime.

# /\v<(\w+)\_s+\1>  <---- Expresión regular para buscar par de palabras duplicadas

<> = límites de la palabra
w = [A-Za-z0-9_]
+ = Encuentra el elemento precedente 1 o mas veces
() = Todo lo que coincida con lo que está en paréntesis, se guarda en memoria
\1 = El texto capturado en paréntesis se llama con esto (si hay más paréntesis,
se usa \2, \3, etc.)
\_s= coincide con espacios en blanco o saltos de linea


# -------------- Tip 76: Poner límites a las palabras

the problem with these new recruits is that
they don't keep their boots clean.

# /the
# /\v<the>

# -------------- Tip 77: Poner límites a una coincidencia

Pattern != match

# Es posible cortar la coincidencia, haciendola un subset de todo el patrón de
# búsqueda

# /zs
# /ze
# /Practical \zsVim
Practical Vim
Practical not Vim
Not vim Practical Vim

# /\v"[^"]+"  
Comienza y termina con comillas y coincide con uno o más caracteres que NO son
comillas

Match "quoted words"---not quote marks.

# /\v"\zs[^"]+\ze"

\zs después de las commilas y \ze antes de las comillas las excluyen


#--------------- Tip 78: Evitar caracteres "especiales"

# \V evita esto para la mayoría, pero no todos :(


Search items: [http://vimdoc.net/search?q=/\\][s]
...
[s]: http://vimdoc.net/search?q=/\\

#/\Vhttp://vimdoc.net/search?q=/\\ ¿funciona?

#/\Vhttp:\/\/vimdoc.net\/search?q=\/\\  --> Se omiten los slash con backslash
 
# ?http://vimdoc.net/search?q=/\\ -> ¿mucho mejor?

# ?http://vimdoc.net/search\?q=/\\ -> backslash omite '?'

# /\Vhttp:\/\/vimdoc.net\/search?q=\/\\\\ - > ¡Por fin!

=escape(@", getcmdtype().'\')
=escape(@", '?\')
=escape(@", '/\')
