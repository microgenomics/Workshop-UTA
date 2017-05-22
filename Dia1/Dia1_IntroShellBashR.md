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

Ahora, escribe el comando `whoami` y presiona la tecla `enter` para enviar el comando a la shell/terminal. La salida del comando es la ID del usuario actual, es decir, nos muestra quién cree que somos:
	/Users/jaimealarcon/Desktop
	# para ir dos directorios antes...
	$ cd ../../
	# Entonces... estando en Desktop podemos ir al directorio Documents así...
	$ cd ../Documents
