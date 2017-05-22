![UNAB-CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon)

[www.castrolab.org](http://www.castrolab.org)

---

###### Workshop_UTA - Día 1

# Introducción a la terminal, Bash, R/RStudio y Bioconductor

---

## Introducción a la Terminal

Hola! Bienvenidos al primer práctico de uso de la terminal y R, aquí aprenderás los comandos esenciales usados en la **terminal**, también conocida como **consola** o **shell**, que son la base en el manejo de cualquier proyecto que tenga bioinformática en él. Así que... manos a la obra!!

+ **Recordatorio: Nunca escribas variables, nombres de archivo o de carpetas con ESPACIOS o ACENTOS. Los espacios y acentos son enemigos naturales de las terminales.**

### ¿Qué es una Terminal?

|	<img src="https://github.com/microgenomics/Workshop-PUC/blob/master/images/terminal.png?raw=true" alt="alt text" width="150"> | <img src="https://github.com/microgenomics/Workshop-PUC/raw/master/images/terminal2.ico" alt="alt text" width="100">	| <img src="https://github.com/microgenomics/Workshop-PUC/blob/master/images/terminal3.jpg?raw=true" alt="alt text" width="200"> | <img src="https://github.com/microgenomics/Workshop-PUC/blob/master/images/terminal4.jpg?raw=true" alt="alt text" width="200"> |
|:-:|:-:|:-:|:-:|
|Esta es una terminal.| Esta también es una terminal.| Esto es la matrix, se hacen cosas parecidas en bioinformática, así que consideremos esta una terminal también.| Esto es un ramsomware, el virus que atacó a Movistar hace unos días, hecho para windows, esto **no** es una terminal.|

Una terminal es un intérprete de comandos fundamental en los sistemas operativos para la interacción con el usuario, ya que, se escriben comandos y acciones en él. Realizar acciones seguidas en una terminal también es considerado programar, en cierta forma podemos ser *hackers*. En este práctico solo nos concentraremos en las terminales basadas en Unix (Linux o Mac, los siento Windows, esta vez no).

### ¿Qué puedo hacer con la Terminal?

Prácticamente todo lo que hace un computador, con una terminal tu puedes dar ordenes y el computador lo hará, lo difícil es transmitir lo que realmente pensamos a la terminal.

### Primeras ordenes

Cuando se aprende un nuevo lenguaje de programación, lo tradicional es hacer que el computador nos diga "Hola Mundo" en dicho lenguaje de programación. Así que el primer paso abrir una terminal como esta:

![openterminal](https://github.com/microgenomics/Workshop-PUC/blob/master/images/openterminal.png?raw=true)
Ahora usaremos el comando `echo` para que la terminal nos diga lo que queremos, escribiremos: `echo "Hola Mundo"` y presionamos `enter`.

![hellow](https://github.com/microgenomics/Workshop-PUC/blob/master/images/hellow.png?raw=true)
En realidad podemos decirle al computador que nos diga cualquier cosa con el comando `echo`, recuerda que cada vez que ingresas un comando o acción debes presionar `enter`.

Ahora, escribe el comando `whoami` y presiona la tecla `enter` para enviar el comando a la shell/terminal. La salida del comando es la ID del usuario actual, es decir, nos muestra quién cree que somos:	$ whoami	jaimealarconA continuación, vamos a ver dónde estamos, usando el comando `pwd` (que significa "*print working directory*"). Nuestro directorio de trabajo actual es donde la terminel siempre cumplirá la orden, buscará archivos de entrada (*input*) y guardará los archivos de salida (*output*) generados, a menos que especifiquemos explícitamente otra cosa.	$ pwd	/Users/jaimealarconAhora, vamos a aprender el comando que nos permitirá ver el contenido de nuestro propio sistema de archivos. Podemos ver lo que hay en nuestro directorio de inicio ejecutando `ls`, que significa "*listing*":	$ ls	Applications Documents    Library      Music        Public	Desktop      Downloads    Movies       PicturesPara más información sobre cómo usar `ls` podemos escribir `man ls`. Man es el comando "manual" de Unix: imprime una descripción de un comando y sus opciones, y (si tienes suerte) proporciona algunos ejemplos de cómo usarlo.	$ man lsPara cambiar de ubicación dentro de la terminal usamos el comando `cd` seguido del nombre del directorio al cuál nos queremos dirigir. `cd` significa "*change directory*".	$ cd Desktop	# Nos movemos desde nuestro directorio de inicio a nuestro escritorio`cd` no imprime, pero si ejecutamos pwd podemos ver donde estamos ahora...	$ pwd
	/Users/jaimealarcon/DesktopPara ir al directorio anterior:	$ cd ..
	# para ir dos directorios antes...
	$ cd ../../
	# Entonces... estando en Desktop podemos ir al directorio Documents así...
	$ cd ../Documents### Trabajando con archivos y directoriosVamos a crear un nuevo directorio llamado "Workshop_UTA" usando el comando `mkdir Workshop_UTA`, así:	$ mkdir Workshop_UTAComo su nombre lo indica, `mkdir` significa "*make directory*". Dado que el nuevo directorio Workshop_UTA es una ruta relativa (es decir, no tiene una barra inclinada principal), el nuevo directorio se crea en el directorio de trabajo actual:	$ lsVamos a entrar al directorio Workshop_UTA con `cd`, y a continuación, ejecutar un editor de texto llamado `Nano` para crear un archivo llamado "draft.txt", así:	$ cd Workshop_UTA	$ nano draft.txt	# Nano no escribe output en la consola, pero ls ahora muestra el nuevo archivo "draft.txt":	$ ls	draft.txtAhora ejecutemos el comando `rm draft.txt`. Este comando elimina los archivos (`rm` es el término "*remove*"):	$ rm draft.txt	$ ls
		Vamos a volver a crear el archivo "draft.txt" y luego regresar un directorio a `/Users/jaimealarcon/Desktop` utilizando `cd` ..	$ pwd	/Users/jaimealarcon/Desktop/Workshop_UTA	$ nano draft.txt	$ ls	draft.txt	$ cd ..	$ pwd
	/Users/jaimealarcon/DesktopSi tratamos de eliminar todo el directorio Workshop_UTA utilizando `rm`, recibiremos un mensaje de error:	$ rm workshop	rm: cannot remove `workshop': Is a directoryEsto sucede porque `rm` por defecto sólo funciona en archivos, no en directorios. Para remover/eliminar un directorio junto con todo lo que se encuentre en él, usamos `rm -r`, así:	$ rm -r Workshop_UTAVamos a crear ese directorio y archivo una vez más. (Ten en cuenta que esta vez estamos ejecutando `nano` con la ruta `Workshop_UTA/draft.txt`, en lugar de ir al directorio de Workshop_UTA y ejecutar los comandos desde allí).	$ pwd	/Users/jaimealarcon/Desktop	$ mkdir Workshop_UTA	$ nano Workshop_UTA/draft.txt	$ ls Workshop_UTA	draft.txt"draft.txt" no es un nombre particularmente informativo, así que cambiemos el nombre del archivo usando `mv`, que es abreviatura de "*move*".	$ mv Workshop_UTA/draft.txt Workshop_UTA/quotes.txtEl primer parámetro le dice a mv qué estamos moviendo, mientras que el segundo es a dónde ir. En este caso, estamos trasladando Workshop_UTA/draft.txt a Workshop_UTA/quotes.txt, que tiene el mismo efecto que renombrar el archivo. Efectivamente, `ls` nos muestra que Workshop_UTA ahora contiene un archivo llamado quotes.txt:	$ ls Workshop_UTA	quotes.txtUtilizamos `mv` una vez más, pero esta vez usaremos el nombre de un directorio como el segundo parámetro para decirle a `mv` que queremos mantener el nombre de archivo, pero poner el archivo en algún lugar nuevo.	$ mv Workshop_UTA/quotes.txt /Users/jaimealarcon/Desktop/ Por otro lado, el comando `cp` funciona muy parecido a `mv`, excepto que copia un archivo en lugar de moverlo.	$ cp quotes.txt /Users/jaimealarcon/Desktop/workshop	# podemos comprobar que se realizó la orden usando el comando ls Ahora vamos al directorio Mothur/	$ cd /Users/jaimealarcon/Desktop/Mothur	$ ls MothurPodemos ejecutar el comando `wc *.fastq.gz`, `wc` significa "*word count*", cuenta el número de líneas, palabras y caracteres en los archivos. El `*` es una *wildcard* y coincide con cero o más caracteres (como un comodín), entonces la shell convierte `*.fastaq.qz` en una lista de todos los archivos que terminan en ".fastaq.qz".

### Wildcard o Expresiones regulares

Las *wildcard* o **expresiones regulares**, son símbolos que representan patrones, nos ayudan a aumentar nuestro umbral de *match* cuando queremos buscar algo.Si ejecutamos `wc -l` en lugar de `wc`, la salida sólo muestra el número de líneas por archivo:	$ wc -l *.fastq.gz
	43383 207_S191_L001_R1_001.fastq.gz
	46905 207_S191_L001_R2_001.fastq.gz
	47393 211_S194_L001_R1_001.fastq.gz
	48886 211_S194_L001_R2_001.fastq.gz
	43212 214_S197_L001_R1_001.fastq.gz
	45679 214_S197_L001_R2_001.fastq.gz
	31672 218_S201_L001_R1_001.fastq.gz
	32385 218_S201_L001_R2_001.fastq.gz
	35438 222_S205_L001_R1_001.fastq.gz
	36301 222_S205_L001_R2_001.fastq.gz
	411254 totalTambién podemos usar `-w` para obtener sólo el número de palabras, o `-c` para obtener sólo el número de caracteres.¿Cuál de estos archivos es el más corto? Es una pregunta fácil de responder cuando sólo hay 10 archivos, pero ¿qué pasa si hay 6000? Nuestro primer paso hacia una solución es ejecutar el comando `wc`:	$ wc -l *.fastq.gz > lengths.txtEl símbolo `>` indica al shell que redirija la salida de la orden al archivo "lengths.txt" en lugar de imprimirlos en la pantalla. La shell creará el archivo si no existe. Si el archivo existe, se sobrescribirá en silencio, lo que puede conducir a la pérdida de datos, por lo tanto, requiere cierta precaución. `ls lengths.txt` confirma que el archivo existe:	$ ls lengths.txt	lengths.txtAhora podemos enviar el contenido de lengths.txt a la pantalla usando `cat lengths.txt`. El comando `cat` significa "concatenar": imprime el contenido de los archivos uno tras otro. Sólo hay un archivo en este caso, por lo que cat sólo nos muestra lo que éste contiene:	$ cat lengths.txt
	43383 207_S191_L001_R1_001.fastq.gz
	46905 207_S191_L001_R2_001.fastq.gz
	47393 211_S194_L001_R1_001.fastq.gz
	48886 211_S194_L001_R2_001.fastq.gz
	43212 214_S197_L001_R1_001.fastq.gz
	45679 214_S197_L001_R2_001.fastq.gz
	31672 218_S201_L001_R1_001.fastq.gz
	32385 218_S201_L001_R2_001.fastq.gz
	35438 222_S205_L001_R1_001.fastq.gz
	36301 222_S205_L001_R2_001.fastq.gz
	411254 totalAhora vamos a usar el comando `sort` para ordenar su contenido. También usaremos el indicador `-n` para especificar que el orden es numérico en lugar de alfabético. Esto no cambiará el archivo, sino que envía el resultado ordenado a la pantalla:	$ sort -n lengths.txt
	31672 218_S201_L001_R1_001.fastq.gz
	32385 218_S201_L001_R2_001.fastq.gz
	35438 222_S205_L001_R1_001.fastq.gz
	36301 222_S205_L001_R2_001.fastq.gz
	43212 214_S197_L001_R1_001.fastq.gz
	43383 207_S191_L001_R1_001.fastq.gz
	45679 214_S197_L001_R2_001.fastq.gz
	46905 207_S191_L001_R2_001.fastq.gz
	47393 211_S194_L001_R1_001.fastq.gz
	48886 211_S194_L001_R2_001.fastq.gz
	411254 totalPodemos poner la lista ordenada de líneas en otro archivo temporal llamado `sorted-lengths.txt` escribiendo `> sorted-lengths.txt` después del comando, tal como usamos `> lengths.txt` anteriormente. Una vez hecho, podemos ejecutar otro comando llamado `head` para obtener las primeras 10 líneas en el archivo "ordenadas-lengths.txt":	$ sort -n lengths.txt > sorted-lengths.txt	$ head -n 1 sorted-lengths.txtUsando el parámetro `-n` 1 con head le dice que sólo queremos la primera línea del archivo; `-n 20` obtendría los primeros 20, y así sucesivamente. Dado que "ordenado-lengths.txt" contiene las longitudes de nuestros archivos ordenados de menor a mayor, la salida de head debe ser el archivo con menos líneas.	$ head -n 1 ordenado-lengths.txt
	31672 218_S201_L001_R1_001.fastq.gz
	Si crees que esto es confuso, estás en buena compañía: incluso una vez que entiendas lo que hace `wc`, `sort`, y `head`, todos esos archivos intermedios hacen difícil seguir lo que está pasando. Podemos hacerlo más fácil de entender corriendo `sort` y `head` juntos:	$ sort -n lengths.txt | head -n 1	31672 218_S201_L001_R1_001.fastq.gz	# Usa "|" para combinar comandos
La barra vertical `|` entre los dos comandos se denomina *pipe*. Le dice a la shell que queremos usar la salida del comando a la izquierda como entrada al comando de la derecha. El ordenador puede crear un archivo temporal si es necesario, o copiar datos de un programa a otro en la memoria, o cualquier otra cosa; No tenemos que saberlo o preocuparnos por eso...	$ wc -l *.fastq.gz | sort -n
	31672 218_S201_L001_R1_001.fastq.gz
	32385 218_S201_L001_R2_001.fastq.gz
	35438 222_S205_L001_R1_001.fastq.gz
	36301 222_S205_L001_R2_001.fastq.gz
	43212 214_S197_L001_R1_001.fastq.gz
	43383 207_S191_L001_R1_001.fastq.gz
	45679 214_S197_L001_R2_001.fastq.gz
	46905 207_S191_L001_R2_001.fastq.gz
	47393 211_S194_L001_R1_001.fastq.gz
	48886 211_S194_L001_R2_001.fastq.gz
	411254 total		$ wc -l *.fastq.gz | sort -n | head -n 1	31672 218_S201_L001_R1_001.fastq.gz#### Actividad: Cuál es la diferencia entre... ¿?	$ echo "hello" > testfile01.txtY...

	$ echo "bye" >> testfile01.txt**Sugerencia**: Intenta ejecutar cada comando dos veces seguidas y luego examinar los archivos de salida.

![hello_bye](https://github.com/microgenomics/Workshop-UTA/blob/master/images/hello_bye.png?raw=true)

### ¿Conoces el archivo fasta?

Es el formato por excelencia para guardar secuencias de ADN, ARN o Proteínas, tiene el siguiente formato:

	>header o identificador, suele tener información concisa acerca de la secuencia
	GCAAGCGGCTAGCTAGCTACTACCAGCGATCACGAGCATCGATCGATGCT
	GTCGTCGAGTCGTAGCTATATTGCGAGCAGAAATATATATTATATATATA
	GCGCGCGCGCGCGCGCGCGCGGGGGGGGGGGGGGGCCCCCCCCCCCCCCC
	GCAAGCGGCTAGCTAGCTACTACCAGCGATCACGAGCATCGATCGATGCT
	GTCGTCGAGTCGTAGCTATATTGCGAGCAGAAATATATATTATATATATA
	GCGCGCGCGCGCGCGCGCGCGGGGGGGGGGGGGGGCCCCCCCCCCCCCCC
	
creemos nuestro propio fasta!

	primero crearemos el archivo 1.fasta solo con el header del archivo
	echo ">mi_secuencia" > 1.fasta
	ahora agregaremos una secuencia al final del archivo
	echo "GCAAGCGGCTAGCTAGCTACTACCAGCGATCACGAGCATCGATCGATGCT" >> 1.fasta
	y podemos agregar otra secuencia al final.
	echo "GTCGTCGAGTCGTAGCTATATTGCGAGCAGAAATATATATTATATATATA" >> 1.fasta
	y otra
	echo "GCGCGCGCGCGCGCGCGCGCGGGGGGGGGGGGGGGCCCCCCCCCCCCCCC" >> 1.fasta
	
ahora veamos nuestro archivo por la terminal!, usaremos el comando **cat**:

	cat 1.fasta
	>mi_secuencia
	GCAAGCGGCTAGCTAGCTACTACCAGCGATCACGAGCATCGATCGATGCT
	GTCGTCGAGTCGTAGCTATATTGCGAGCAGAAATATATATTATATATATA
	GCGCGCGCGCGCGCGCGCGCGGGGGGGGGGGGGGGCCCCCCCCCCCCCCC
	
	Felicidades!, hemos hecho un archivo fasta
	
	repitamos el proceso pero el fasta deberá llamarse test.fasta
	
	echo ">mi_secuencia_2" > test.fasta
	echo "PKCKPAEECKELAPCKAELKCKLEAPLKECKLAKCLAPEKCLPP" >> test.fasta
	
	Listo!
	
Si rápidamente queremos buscar algún patron en el fasta por ejemplo una pequeña secuencia "CGAT" podemos usar el comando grep:

	grep -n "CGAT" 1.fasta
	2:GCAAGCGGCTAGCTAGCTACTACCAGCGATCACGAGCATCGATCGATGCT
	
	su sintaxis humana sería: [comando + opciones] [patrón] [donde buscar patrón]

Donde:

* grep es el comando para buscar patrones
* -n es un parámetro de grep para indicar en que número de linea encuentra el match
* "CGAT" es el patron para buscar
* 1.fasta es el archivo donde se buscara el patrón
### LOOPS o ciclosSi queremos copiar nuestros archivos `.fastq.gz` para futuros análisis y agregarle el prefijo `backup-`...
	$ cd Mothur 	$ cp *.fastq.gz backup-*.fastq.gz	cp: target `backup-*.fastq.gz' is not a directoryEste problema surge cuando cp recibe más de dos entradas. Cuando esto sucede, espera que la última entrada sea un directorio donde pueda copiar los archivos. Dado que no hay ningún directorio llamado backup- *.fastq.gz en el directorio Mothur obtendremos un error.En su lugar, podemos usar un Loop o ciclo para hacer una misma operación varias veces con diferentes *inputs* y *outputs*. Aquí hay un ejemplo sencillo:	for filename in *.fastq.gz	do
		cp $filename backup-$filename 	done
Para cada iteración, el nombre de cada cosa se asigna secuencialmente a la variable y los comandos dentro del bucle se ejecutan antes de pasar a lo siguiente en la lista. Dentro del bucle, pedimos el valor de la variable poniendo `$` delante de él. `$` le dice al intérprete de la shell que trate la variable como un nombre de variable y que sustituya su valor, en lugar de tratarlo como texto o como un comando externo.### Encontrando "cosas"De la misma manera que muchos de nosotros ahora usamos "Google" como un verbo que significa "para encontrar", los programadores de Unix a menudo usan el comando `grep`. `grep` es una contracción de "*global / regular expression / print*", una secuencia común de operaciones en los primeros editores de texto de Unix.	$ cd /Users/jaimealarcon/Desktop	$ more taxonomia.txt`grep` encuentra e imprime líneas en archivos que coinciden con un patrón. Para nuestros ejemplos, usaremos un archivo que contiene la taxonomía de 567 OTUs, buscando si encontramos el genero *Pelomonas*...	$ grep 'Pelomonas' taxonomia.txt 	{"id":"Otu0006", "metadata":{"taxonomy":["Bacteria", "Proteobacteria", "Betaproteobacteria", "Burkholderiales", "Comamonadaceae", "Pelomonas"], "bootstrap":[100, 100, 100, 100, 100, 99]}},	$ grep 'Pseudomonadaceae' -c taxonomia.txtCon el parámetro `-c` podemos contar las veces que la palabra `'Pseudomonadaceae'` se encuentra en el archivo "taxonomia.txt".

**Puedes encontrar una lista detallada de los comandos básicos mas usados en la terminal [aquí](https://github.com/microgenomics/Workshop-UTA/blob/master/Dia1/GuiaComandosBasicosTerminal.md).**### Variables

En la terminal podemos asignar frases completas (o números), a variables, y no nos referimos a variables matemáticas X e Y (podemos hacerlo, pero no es entretenido). **Recuerda siempre poner tus variables dentro de comillas dobles**...

![despacito](https://github.com/microgenomics/Workshop-PUC/blob/master/images/despacito.png?raw=true)
... y ahora en la misma terminal, podemos decirle al computador que nos muestre los párrafos con el comando echo y darle las variables como parámetro. **Cuando queremos hacer referencia a una variable la escribimos anteponiendo un "$"**: 

	echo $variable1, $variable2, $variable3, $variable4
	Despacito, Quiero respirar tu cuello despacito, Deja que te diga cosas al oído, Para que te acuerdes si no estás conmigo

### Crea tu propio script

Un script es un pequeño archivo con comandos que lee la terminal y las ejecuta en orden desde arriba hacia abajo. Es muy parecido a una receta de cocina. Es más, cocinemos un archivo fastq!

fastq es un archivo de text que tiene un formato en particular similar al fasta (también guarda secuencias), con la diferencia que un fastq también guarda calidades de la secuencia y tiene el siguiente formato:

	@SEQ_ID
	GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTTT
	+
	!''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC65
	
* La primera linea es el header del fastq y siempre empieza con un @
* La segunda linea es la secuencia, igual que un archivo fasta
* la tercera linea es un "+" para (opcionalmente), poner descripciones de la secuencia
* La ultima linea corresponde a las calidades de cada nucleótido, cada símbolo representa un número y por ende una calidad (usado por ejemplo cuando se manda a secuenciar un organismo, las secuencias llegan en este formato). Para ver que número equivale cada símbolo sigue [este enlace](http://ascii.cl)

Aquí les presentamos un comando para editar (o crear), un archivo: **vi**.

	vi miscript.sh
	(.sh es solo una extensión genérica para indicar que adentro hay (o habrá), código shell (sh).

Se nos abrirá una terminal con nada de contenido y en negro (o blanco, dependiendo de tu terminal), si el archivo miscript.bash existe, entonces se abrirá y podremos modificarlo, de lo contrario se creará.

Para **insertar** texto debemos apretar la letra **i**, con ello todo lo que tecleemos estará en el archivo, y como hemos aprendido un par de comandos, haremos nuestra receta de cocina escribiendo:

	echo "@mi_primer_fastq"
	echo "ATGTTGCAACGATTGGTCGTTGCATTATGCCTGCTTGGGT"
	echo "+"
	echo "CCCCCCC65++)(%%%%).1!''*((((>>>>>()()()>"

Para guardar, presionamos **Escape**, para salir del modo **insertar**, y escribimos "**:wq**" y presionamos Enter.
 
* ":" es para indicar que queremos una acción especial de **vi**
* w es para indicar a **vi** que guarde los cambios.
* q es para indicar que saldremos del editor

Todo esto debiera verse así:

 ![](https://github.com/microgenomics/Workshop-PUC/blob/master/images/vimiscript.png?raw=true) 
 Y ahora ejecutamos nuestro script usando el comando **bash**: 
 
 	bash miscript.sh
 	@mi_primer_fastq
	ATGTTGCAACGATTGGTCGTTGCATTATGCCTGCTTGGGT
	+
	CCCCCCC65++)(%%%%).1!''*((((>>>>>()()()>
	
	podemos redireccionar la salida de un script
	bash miscript.sh > miprimer.fastq
	
Cuando se trabaja en bioinformática, es normal que los archivos fasta o fastq sean muy grandes, con muchas secuencias y que ocupen muchos Gigas de espacio. Por eso no es descabellado pensar que podemos reducir su tamaño (y no nos referimos a botar a la basura la mitad de las secuencias), nos referimos a **comprimir** y usaremos el comando **gzip**:

	en nuestro tendríamos que escribir:
	gzip 1.fasta
	gzip 2.fasta
	gzip 3.fasta
	gzip test.fasta
	gzip miprimer.fastq
	
Esto se volvería imposible de hacer si tenemos 1000 fastas, por eso es mejor usar un **ciclo for**. La estructura de un **for** es:

	for variable in $(comando)
	do
		comandos, scripts o recetas de cocina
	done
	
en nuestro caso sería

	for mi_archivo in $(ls *.fast[aq])
	do
		gzip $mi_archivo
	done
	
	ls *.fast[aq]
	1.fasta.gz	2.fasta.gz	3.fasta.gz	test.fasta.gz	miprimer.fastq.gz

En este caso la sintaxis del ciclo **for** la acompañamos de una expresión regular donde apuntamos a "todo (`*`) lo que termine con `.fast` y además el último carácter debe terminar en `a` o `q`". El resultado de esta expresión regular es tomado por **ls** y luego por el ciclo **for**, agregandose a una variable (la misma variable se renueva con otro valor en cada "vuelta").

### Aplicando todo

Primero descarga y descomprime este pequeño set de secuencias [aquí](https://github.com/microgenomics/Workshop-UTA/raw/master/Dia1/problem1.zip). Tenemos el siguiente caso: Han llegado secuencias de personas enfermas con asma  y personas sanas, el ADN fue secuenciado con tecnología Next-Gen y en formato fastq, y nuestro jefe quiere rápidamente unas estadísticas iniciales, como el número de secuencias, cuanto pesan los archivos y mostrar estos resultados para cada muestra en un archivo llamado reporte.txt.

Primero entramos en la carpeta descomprimida

	cd problema1

podemos empezar escribiendo el reporte con un título y guardarlo mientras en el archivo requerido.

	echo "Estadísticas iniciales" > reporte.txt
	
listamos el directorio para saber cuantos archivos hay
	
	ls
	muestra1.fastq	muestra3.fastq	muestra5.fastq	muestra7.fastq
	muestra2.fastq	muestra4.fastq	muestra6.fastq
	
aquí podemos de inmediato responder la pregunta de cuanto pesan los 
archivos tan solo agregando el parámetro antes usado -lh

	ls -lh
	-rw-r--r--  1 castrolab04  staff   3.4K May 17 18:31 muestra1.fastq
	-rw-r--r--  1 castrolab04  staff   4.6K May 17 18:31 muestra2.fastq
	-rw-r--r--  1 castrolab04  staff   8.9K May 17 18:32 muestra3.fastq
	-rw-r--r--  1 castrolab04  staff   4.7K May 17 18:32 muestra4.fastq
	-rw-r--r--  1 castrolab04  staff   6.8K May 17 18:32 muestra5.fastq
	-rw-r--r--  1 castrolab04  staff   6.4K May 17 18:33 muestra6.fastq
	-rw-r--r--  1 castrolab04  staff   5.0K May 17 18:33 muestra7.fastq

y esto podemos guardarlo mientras en nuestro archivo reporte .txt

	echo "Peso de los archivos" >> reporte.txt
	ls -lh *.fastq >> reporte.txt
	
para resolver la petición de saber cuantas secuencias existen, recuerden que existe el comando **wc -l**

	echo "Cantidad de secuencias" >> reporte.txt
	wc -l muestra1.fastq
	40 muestra1.fastq
	
	en este caso son 40 líneas en el fastq, pero recordemos que en realidad un fastq contiene 1 secuencia cada 4 líneas.
	Así que para obtener el número real, debemos dividir el número de wc en 4. 
	La forma mas rápida es con un pequeño truco en "awk"
	
	wc -l muestra1.fastq |awk '{print $1/4, $2}'
	40
	
ahora veamos que significa este nuevo truco (ya un poco más avanzado), llamado awk: todo código awk puesto en la terminal va encerrado entre comillas simples y en bloques de llaves `'{}'`. `print` es una función de awk para imprimir en pantalla la variable que este al lado de la función, en este caso `$1/4`, que a su vez `$1` significa **columna 1**, y como sabemos que la primera columna es el número que entrega **wc -l** lo dividimos por 4 (`/4`), por ultimo `, $2` significa que habrá un espacio e imprimirá la columna 2 (el nombre de la muestra), resultando así en `'{$1/4, $2}'`.

Ahora bien, resultaría un poco tedioso hacer este comando 7 veces (incluso hay proyectos con mas de 700 muestras), por lo que haremos uso del ciclo para calcular el número de secuencias.

	Recuerden que para acceder al valor de una variable se antepone "$"
	for mifastq in $(ls *.fastq)
	do
		wc -l $mifastq |awk '{print $1/4, $2}'
	done
	
	10 muestra1.fastq
	12 muestra2.fastq
	20 muestra3.fastq
	11 muestra4.fastq
	15 muestra5.fastq
	15 muestra6.fastq
	10 muestra7.fastq

y guardamos este resultado en reporte.txt

	for mifastq in $(ls *.fastq)
	do
		wc -l $mifastq |awk '{print $1/4, $2}'
	done >> reporte.txt
	
Y listo! podemos ver el resultado mostrando el reporte con el comando **cat**
	
	cat reporte.txt
	Estadísticas iniciales
	Peso de los archivos
	-rw-r--r--  1 castrolab04  staff   3.5K May 17 18:47 muestra1.fastq
	-rw-r--r--  1 castrolab04  staff   4.6K May 17 18:31 muestra2.fastq
	-rw-r--r--  1 castrolab04  staff   8.9K May 17 18:32 muestra3.fastq
	-rw-r--r--  1 castrolab04  staff   4.7K May 17 18:32 muestra4.fastq
	-rw-r--r--  1 castrolab04  staff   6.8K May 17 18:32 muestra5.fastq
	-rw-r--r--  1 castrolab04  staff   6.4K May 17 18:33 muestra6.fastq
	-rw-r--r--  1 castrolab04  staff   5.0K May 17 18:33 muestra7.fastq
	Cantidad de secuencias
	10 muestra1.fastq
	12 muestra2.fastq
	20 muestra3.fastq
	11 muestra4.fastq
	15 muestra5.fastq
	15 muestra6.fastq
	10 muestra7.fastq
		
Y listo!, ya podemos enviar el reporte a nuestro jefe, por mientras, seguiremos con el tutorial pero ahora desde una perspectiva desde el código de R. No olviden salir de la carpeta para continuar el tutorial
	
	cd ..

## R: ¿Qué es?
R es un lenguaje de programación para cálculos estadísticos y gráficos  basado en el lenguaje S creado en los laboratorios Bell (Lucent Technologies). Muy popular en la ciencia debido a sus gráficos de alta calidad y versatilidad al momento de mostrar lo que se tiene en mente.

### Las 4 zonas

![Rstudio](https://github.com/microgenomics/Workshop-UTA/blob/master/images/Rstudio.png?raw=true)
* **Editor de texto**: en esta zona es donde ocurre la magia, donde pondremos el código e iremos preparando nuestros scripts. Podemos ejecutar nuestro código aprentando el botón `Run`, o ejecutando la linea que queremos ejecutar con ctrl (o command en el caso de mac) + enter
* **Consola**: las acciones que escribimos en el editor de texto se ejecutan en esta ventana, aquí aparecen los resultados excepto los gráficos. También puedes escribir el código directamente aquí.
* **Historial y ambiente**: en esta ventana las variables que creemos serán guardadas y mostradas aquí, ademas de estar disponibles en caso que hagamos mas de un script.
* **Archivos, gráficos, paquetes, ayuda y visualizador (AGPAV)**: esta ventana se divide a su vez en otras 5 pestañas que son de mucha utilidad:
	* **Files**: aquí se muestran los archivos de nuestro `work directory`, generalmente el `HOME`
	* **Plots**: en esta ventana se muestran nuestros gráficos.
	* **Packages**: muestra una lista con los paquetes, instalados además de poder instalar otros.
	* **Help**: es una ventana de ayuda cuando queremos obtener información de alguna librería en especial o una función desconocida.
	* **Viewer**: Es solo un panel de visualización para casos (por ejemplo), de conexiones remotas, no lo tomaremos en cuenta.

### Paquetes

Un paquete en R, es una colección de funciones agrupados y que cumplen con un formato específico. Cada vez que instalamos R, un número finito de funciones viene con ello, suficientes para otorgar estabilidad al entorno.

Si apretamos en la pestaña Packages en la ventana de AGPAV, obtendremos una lista de los paquetes instalados y de aquellos que están "activados"
![](https://github.com/microgenomics/Workshop-PUC/blob/master/images/packages.png?raw=true)
También existe una gran librería donde podemos descargar mas paquetes de acuerdo a nuestras necesidades. Por ejemplo, uno de los paquetes mas usados es **ggplot2**, un paquete usado para graficar todo tipo de datos en distintos tipos de formatos. Para instalarlo presionamos el botón `Install` que esta en la pestaña Packages de la ventana AGPAV.

<img src="https://github.com/microgenomics/Workshop-PUC/blob/master/images/installggplot2.png?raw=true" alt="alt text" width="400">

seleccionamos **ggplot2** y presionamos install, esperamos un poco y listo!. De esta forma se instalan los paquetes de R.

Aprovechemos e instalemos un paquete usaremos mas adelante, se trata del paquete ape, usado para análisis filogenéticos y que contiene funciones de lectura de archivos fasta. Así que otra vez para instalarlo presionamos el botón `Install` que esta en la pestaña Packages de la ventana AGPAV.

<img src="https://github.com/microgenomics/Workshop-PUC/blob/master/images/ape.png?raw=true" alt="alt text" width="400">

En estos casos existe una gran biblioteca de paquetes (librería), donde se alojan para ser descargados, esta librería se llama CRAN. 

Existe otra librería llamada `Bioconductor` (www.bioconductor.org) el cual es una serie de paquetes de R que han sido especialmente desarrollados para bioinformática. Aunque no existe una interfaz gráfica para instalar paquetes como CRAN, `bioconductor` ofrece 2 simples lineas de código para instalar cualquier paquete de esta librería. Por ejemplo, instalemos el paquete `phyloseq`

	source("https://bioconductor.org/biocLite.R")
	biocLite("phyloseq")
	library(phyloseq)
	library(ggplot2)


### Operaciones básicas

R tiene la capacidad de interpretar de forma mas intuitiva que una shell, por ejemplo, escribamos en la consola:
	
	x<-1
	
esto significa que el `x` tomara el valor `<-` de `1`. Esto por si solo no nos devolverá valor alguno. Si escribimos
	
	x
	[1] 1
ahora el sistema devuelve el valor de `1`, esto es por que solo estamos asignando un valor y no estamos pidiendo a R que muestre valor, para eso es la segunda linea. No nos preocupemos por el 1 encerrado en corchetes `[1]` solo es el numero de lineas de nuestro output.

R puede funcionar como una calculadora.
	
	x + 1
	[1] 2
	x - 1
	[1] 0
	x<- x + 1
	x
	[1] 2
	x<-x*x
	[1] 4
	log(10)
	[1] 2.302585

Una de las utilidades básicas mas usadas en R es la lectura de archivos (tablas como tsv o csv). Por ejemplo, descarga y descomprime [este archivo](https://github.com/microgenomics/Workshop-UTA/raw/master/Dia1/taxcount.csv.zip). Es un archivo CSV que contiene información acerca de bacterias en personas con asma y personas sanas y la abundancia en que estas bacterias se encuentran.

antes de leer el archivo con R, primero debes establecer nuestro directorio de trabajo, esto se hace con la función **setwd()**:

	y estableceremos la ruta en nuestra carpeta actual dia1
	/Users/castrolab04/Desktop/Workshop-PUC/dia1 (esta ruta cambia de PC en PC, debes encontrar tu propia ruta)
	si no sabes como obtener la ruta completa, recuerda que existe el comando pwd que puedes usar en una terminal y copiar desde allí la ruta completa.
	setwd("/Users/castrolab04/Desktop/Workshop-PUC/dia1")
	
listo!, ahora que establecimos nuestro working directory, ya podemos leer nuestro archivo taxcount.csv, para eso usaremos la función **read.csv** de R.

	df<-read.csv(file.choose(),header = TRUE)
	
* df es la variable donde guardaremos nuestro CSV
* read.csv() es la función para leer el CSV
* cada función necesita parámetros (inputs) para trabajar, en este caso son file.choose() (el nombre de una función que nos dejará escoger un archivo y seleccionamos taxcount.csv), y header=TRUE que significa que nuestra tabla taxcount.csv tiene encabezados dentro aparte de los datos.

ahora la nuestra tabla esta guardada en la variable `df`, y tiene mas de 1000 filas con información, por eso, para no mostrar todas líneas usaremos la función **head()**

**Nota: escribimos las funciones con () porque se da a entender que reciben datos para funcionar**

	head(df)
	 Kingdom         Phylum          Class             Order             Family           Genus                        Species                           Name SRR1528344 SRR1528346 SRR1528348 SRR1528420 SRR1528426 SRR1528430 SRR1528434 SRR1528456 SRR1528458 SRR1528460 SRR1528462 SRR1528464 SRR1528466 SRR1528468
	1 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium       Corynebacterium accolens       Corynebacterium accolens         12         23          3          9        205        592       2234          3       9973        422          1        154         14       3110
	2 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium    Corynebacterium afermentans    Corynebacterium afermentans          1          0          0          1          7          7         12          7          1          0          0          0          0          2
	3 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium   Corynebacterium ammoniagenes   Corynebacterium ammoniagenes          0          0          0          0          2          0          0          0          0          0          0          0          0          2
	4 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium     Corynebacterium amycolatum     Corynebacterium amycolatum          0          0          0          0          1          0          0          1          6          0          0          0          0          0
	5 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium Corynebacterium argentoratense Corynebacterium argentoratense          0          0          0          0          0          0          0          0          0          0          0          0          0          0
	6 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium    Corynebacterium aurimucosum    Corynebacterium aurimucosum          0          0          0          0          0          0          0         38          1          0          0          0          0          0

También podemos hacerlo manualmente especificando el nombre del archivo:

		df<-read.csv("taxcount.csv",header = TRUE)
		y es exactamente lo mismo
		
Exploremos un poco nuestros datos, para ver la cantidad de filas que tiene nuestro archivo usamos la función **nrow()** y para el numero de columnas **ncol()**

	nrow(df)
	[1] 1414
	ncol(df)
	[1] 21

Para obtener las últimas líneas de nuestros datos, ocupamos la función **tail()**

	tail(df)
	     Kingdom Phylum  Class  Order Family  Genus                    Species SRR1528344 SRR1528346 SRR1528348 SRR1528420 SRR1528426 SRR1528430 SRR1528434 SRR1528456 SRR1528458 SRR1528460 SRR1528462 SRR1528464 SRR1528466 SRR1528468
	1409  unknow unknow unknow unknow unknow unknow            naegleriophila           0          0          1          0          0          0          0          0          0          0          0          0          0          0
	1410  unknow unknow unknow unknow unknow unknow    Paraburkholderia kirkii          0          0          0          0          1          0          1          0          0          0          0          0          0          0
	1411  unknow unknow unknow unknow unknow unknow  Paracaedibacter symbiosus          0          0          0          0          0          0          0          0          0          0          0          0          0          0
	1412  unknow unknow unknow unknow unknow unknow        Solibacter usitatus          0          0          0          0          0          0          0          0          0          0          0          1          0          0
	1413  unknow unknow unknow unknow unknow unknow witches'-broom phytoplasma          0          0          0          0          0          0          0          0          0          0          0          0          0          0
	1414  unknow unknow unknow unknow unknow unknow        yellows phytoplasma          0          1          0          0          0          0          0          0          0          0          0          0          0          0
	
Para obtener el nombre de las columnas de nuestra tabla usamos la función **names()**

	names(df)
	[1] "Kingdom"    "Phylum"     "Class"      "Order"      "Family"     "Genus"      "Species"    "SRR1528344" "SRR1528346" "SRR1528348" "SRR1528420" "SRR1528426" "SRR1528430" "SRR1528434" "SRR1528456" "SRR1528458" "SRR1528460" "SRR1528462" "SRR1528464" "SRR1528466" "SRR1528468"
	
Una tabla a fin de cuentas es una matriz de AxB, y por tanto tiene coordenadas A e B, (o XY), de esta forma podemos acceder a algún dato en específico a nuestra tabla, la sintaxis es [FILA,COLUMNA], si llenamos con solo un valor, entonces se mostraran todos los valores del valor que no pusimos:

	df[2,]
	2 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium    Corynebacterium afermentans    Corynebacterium afermentans          1          0          0          1          7          7         12          7          1          0          0          0          0          2
	
La forma `[2,]` significa acceder a la fila 2 y todas las columnas. si dejamos vacío el lado izquierdo, y seleccionamos la columna 2, entonces mostraremos todas las filas de la columna 2 (no lo escribiremos en honor al espacio del tutorial, pero tu puedes hacerlo en tu RStudio.

Existen trucos bastante útiles cuando se quiere consultar las tablas, por ejemplo, ¿cuáles son las filas que tienen valores mayores a 1?. Intuitivamente uno tendería a escribir `df > 1` y en cierta medida funciona, pero existen columnas en nuestra tabla que no son numéricas,   por eso debemos limitar que columnas son las que serán sometida a dicha comparación, y sabemos que son desde la `8` a la `21` y lo rellenamos en la sección de columnas (lado derecho de la coma)

	rowSums(df[,8:21])>1
  
Esto retornará un montón de valores booleanos (TRUE y FALSE), con las posiciones de las filas que cumplen con nuestra condición. Este resultado podemos usarlo como índice para obtener los valores reales de la tabla y no solo los booleanos.
  
  	index <- rowSums(df[,8:21])>1
  	head(df[index,])
  	    Kingdom         Phylum          Class             Order             Family           Genus                              Species SRR1528344 SRR1528346 SRR1528348 SRR1528420 SRR1528426 SRR1528430 SRR1528434 SRR1528456 SRR1528458 SRR1528460 SRR1528462 SRR1528464 SRR1528466 SRR1528468
	1  Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium             Corynebacterium accolens         12         23          3          9        205        592       2234          3       9973        422          1        154         14       3110
	22 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium Corynebacterium pseudodiphtheriticum          5       1391       5450        561         41        757        463        571        581         70          0         39         51      26161
	23 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae Corynebacterium     Corynebacterium pseudogenitalium          3          7          1         13         51         19          2         12          9         26         68         27          8          7
	39 Bacteria Actinobacteria Actinobacteria Corynebacteriales Corynebacteriaceae       Turicella                   Turicella otitidis          2          1          1          0          1          0          0          0          0          0          0          0          0          0
	41 Bacteria Actinobacteria Actinobacteria Corynebacteriales        Dietziaceae         Dietzia                     Dietzia cinnamea          2          0          0          0          3          0          1          3          0          0          0          0          0          0
	60 Bacteria Actinobacteria Actinobacteria Corynebacteriales   Mycobacteriaceae   Mycobacterium               Mycobacterium gordonae          2          1          0          0          0          0          1          0          0          0          0          0          0          0
	
Encerramos el resultado en la función **head()** solo para mostrarte que ya no son correlativas las filas.

---

![bot](https://github.com/microgenomics/Workshop-UTA/blob/master/images/huinchaunab.jpg?raw=true)
