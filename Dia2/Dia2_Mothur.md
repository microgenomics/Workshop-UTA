![UNAB_CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

![UCDavisChile](https://github.com/microgenomics/Workshop-UTA/blob/master/images/UCDavisChile.jpg?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon) - Ignacio Ramos

[http://www.unab.cl](http://www.unab.cl) - [http://www.cbib.cl](http://www.cbib.cl) - [www.castrolab.org](http://www.castrolab.org) - [http://www.ucdavischile.org](http://www.ucdavischile.org)

---

###### Workshop_UTA - Día 2

# Introducción a Mothur, SILVA y GreenGenes

## MOTHUR *workflow*
### ¿Qué es Mothur?`Mothur` es un software de libre acceso (*open-source*) expandible y muy utilizado en ecología microbiana, siendo la herramienta para analizar secuencias de 16S rRNA más citada.![mothur](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur_portada.png?raw=true)

### Necesitamos los siguientes archivos de entrada (*inputs*)

#### 28 archivos FASTQ:+ `J0X_S353_L001_R1_001.fastq.gz` y `J0X_S354_L001_R2_001.fastq.gz` - correspondientes a los *reads* *forward* (R1) y *reverse* (R2) de **12 muestras de suelo**.
+ `mockA-041717_S91_L001_R1/2_001.fastq.gz` - **muestras Mock Community**.
+ `waterA-041717_S93_L001_R1/2_001.fastq.gz` - **muestras de agua**.#### Datasets de referencia:+ `silva.nr_v123.v4.align` - [https://www.arb-silva.de](https://www.arb-silva.de) Consiste en **archivos taxonómicos** (`.tax`) y **secuencias de referencia** representativas (`.fasta`).+ `trainset9_032012.pds.tax` y `trainset9_032012.pds.fasta` - [https://rdp.cme.msu.edu](https://rdp.cme.msu.edu) Consisten en **archivos RDP** formateados para `Mothur` con **secuencias de referencia 16S** y **asignaciones taxonómicas**.### MetadataArchivo `stability.txt`: ID de la muestra y lista de *reads* *forward* y *reverse* emparejados.

Lo que verémos a continuación es una adaptación de [Mothur MiSeq SOP](https://mothur.org/wiki/MiSeq_SOP) donde se puede encontrar el tutorial completo.

Primero, crearemos nuestra carpeta de trabajo...

	mkdir /Users/jaimealarcon/Desktop/workshop_mothur	cd /Users/jaimealarcon/Desktop/workshop_mothur
Luego de acceder a nuestro directorio `workshop_mothur`, copiaremos los archivos necesarios. (Asegurate que todos los archivos esten en la carpeta de trabajo, si no el programa no encontrara lo requerido y obtendras un error) 	cp –r /Users/jaimealarcon/Desktop/materiales_mothur/* /Users/jaimealarcon/Desktop/workshop_mothurY le daremos comienzo a `Mothur`...	./mothur

![mothur_inicio](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur_inicio.png?raw=true)

### 1. Pre-procesamiento 

#### Ensamble *paired-end*Debido a que tenemos *reads* *paired-end* pareadas, una forma de mejorar la calidad general de la secuencia es ensamblando estas *reads*. La orden para hacerlo en `Mothur` es:

	make.contigs(file=stability.files, processors=4)
`Mothur` utiliza su propio algoritmo simple para fusionar las reads emparejadas. Se mostrarán muchos mensajes en la pantalla, pero en lo que vale la pena poner atención, es a la lista de archivos de salida (*outputs*) generados por mothur.

La terminal deberia mostrar algo asi:

![mothur1](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur1.png?raw=true)
`Mothur` siempre dará una lista de los archivos que generó - una característica bastante útil. Hay que tener en cuenta que todas las *reads* están ahora en un sólo archivo FASTA llamado `stability.trim.contigs.fasta`. La información que enlaza *reads* a las muestras esta en `stability.contigs.groups`. Podemos echar un vistazo a los resultados del ensamblaje.
Los arhivos que genera mothur son los siguientes:

![mothur2](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur2.png?raw=true)

	summary.seqs(fasta=stability.trim.contigs.fasta)

![mothur3](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur3.png?raw=true)Este comando que veremos una y otra vez resume el contenido de un archivo FASTA. Poner atención al total de secuencias ensambladas con éxito. También podemos notar que la mayoría de las *reads* están dentro del tamaño esperado (~250bps), pero algunas *reads* son demasiado largas. 

	screen.seqs(fasta=stability.trim.contigs.fasta, group=stability.contigs.groups, maxambig=0, minlength=100, maxlength=300)
	
Éste comando elimina las secuencias con mas de 300pb que son demasiado largas, deja las secuencias que tienen al menos 100pb y elimina las que contengan bases ambiguas (N). Una vez más, ejecutaremos el comando `summary.seqs` para comprobar la salida.

	summary.seqs(fasta=stability.trim.contigs.good.fasta)
	
**Notar cuantas secuencias tenemos ahora, que pasaron el filtro.**![mothur4](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur4.png?raw=true)

#### Reducir el número de secuencias redundantesEn sus muestras, habrá secuencias que son idénticas. Podemos emitir los siguientes comandos para combinar secuencias duplicadas en una y mantener un registro del número de copias de cada una de las secuencias duplicadas.	unique.seqs(fasta=stability.trim.contigs.good.fasta)	count.seqs(name=stability.trim.contigs.good.names, group=stability.contigs.good.groups)Esto generará `stability.trim.contigs.good.unique.fasta` que contiene sólo una copia de cada una de las *reads* duplicadas. Podemos ver cuantas secuencias únicas hay. El archivo `stability.trim.contigs.good.names` mantiene un registro de las secuencias que se duplican. El comando `count.seqs` genera un archivo `count_table` - `stability.trim.contigs.good.count_table` que sólo mantiene un registro del número de copias de cada una de las secuencias duplicadas. En general, el archivo `.names` le da la membresía de cada grupo de secuencias.

#### Alineación de secuencias a base de datos de referenciaA continuación, alinearemos las secuencias con la alineación de referencia silva version 123. En este caso la secuenciacion fue realizada contra la region variable 4 del 16S (V4). Para terminos practicos hemos cortado la base de datos de silva que contiene todo el gen a solo la region V4 (silva.nr_v123.v4.align.txt).	mothur > align.seqs(fasta=stability.trim.contigs.good.unique.fasta, reference=silva.nr_v123.v4.align.txt, flip=t)Veremos el resumen de alineación con:

	mothur > summary.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table)
	
![mothur5](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur5.png?raw=true)

Vemos que nuestras secuencias se alinean con la region V4 entre las regiones 1968 y 11550 y es ahi donde haremos el corte.

Podemos ver la región en que la mayoría de las secuencias se alinean. Eliminaremos secuencias mal alineadas y eliminaremos las columnas *gap-only* escribiendo:	mothur > screen.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table, summary=stability.trim.contigs.good.unique.summary, start=1968, end=11550, maxhomop=8)
	
	summary.seqs(fasta=current, count=current)

Luego de ver el resumen de las secuencias que estan alineando con la base de datos veras algo asi.

![mothur6](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur6.png?raw=true)	mothur > filter.seqs(fasta=stability.trim.contigs.good.unique.good.align, vertical=T, trump=.)Los resultados del alineamiento y filtro son:![mothur7](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur7.png?raw=true)Después de alinear y recortar, algunas de las *reads* ahora pueden estar duplicadas, así que ejecutaremos el comando `unique.seqs` de nuevo.	mothur > unique.seqs(fasta=stability.trim.contigs.good.unique.good.filter.fasta, count=stability.trim.contigs.good.good.count_table)Ahora tenemos secuencias alineadas en `stability.trim.contigs.good.unique.good.filter.unique.fasta` y las secuencias duplicadas son rastreadas en `stability.trim.contigs.good.unique.good.filter.count_table`.Utilizaremos el comando `pre.cluster` para eliminar aún más las secuencias raras que son muy similares a las secuencias abundantes (dentro de las diferencias de 2nt). Es probable que estas secuencias raras se deban a errores de secuenciación.	mothur > pre.cluster(fasta=stability.trim.contigs.good.unique.good.filter.unique.fasta, count=stability.trim.contigs.good.unique.good.filter.count_table, diffs=2)#### Eliminar las secuencias quiméricas y no 16SA continuación utilizaremos **UCHIME** un software que incluye **Mothur** para eliminar las secuencias quiméricas.Ejecutamos el comando para eliminar quimera, invocando al packete UCHIME que tiene incorporado MOTHUR asi:	mothur > chimera.uchime(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.count_table, dereplicate=t)Utilizando el comando remove.seq, utilizamos el archivo que arroja UCHIME para eliminar esas secuencias quimericas.

	mothur > remove.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, accnos=stability.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.accnos)![mothur8](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur8.png?raw=true)

Observa que un porcentaje no menor fue eliminado por UCHIME, por lo tanto este paso es muy importante para el analisis, asi evitamos no clasificar secuencias que posiblemente jamas existieron (Falsos positivos)Usando el comando `classify.seqs`, Clasificaremos las secuencias de nuestros datos. Tenga en cuenta que este paso requiere la base de datos RDP (`trainset9_032012.pds.fasta` y el archivo de taxonomía) como referencia.	mothur > classify.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.count_table, reference=trainset9_032012.pds.fasta, taxonomy=trainset9_032012.pds.tax, cutoff=80)
	
Date cuenta que Mothur nos advirte que hay secuencias que no han podido ser clasificadas por lo tanto, deben eliminarse posteriormente.		mothur > remove.lineage(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.taxonomy, taxon=Chloroplast-Mitochondria-unknown-Archaea-Eukaryota)El comando `classify.seqs` es similar al OTU picking de referencia cerrada. Las secuencias son emparejadas con una secuencia de referencia en la base de datos y las secuencias sin similitud pueden eliminarse mediante los comandos `remove.lineage` o `remove.seqs`.
### 2. OTU *Picking*Ya realizado el pre-procesamiento, la siguiente etapa de nuestro análisis es hacer OTUs. `Mothur` toma un enfoque *de novo* OTU *picking*, primero calcula las distancias pair-wise de cada una de las secuencias, luego agrupandolas en OTUs usando agrupación jerárquica. Observen que al especificar un corte de 0.20, sólo se registrarán pares de secuencias que sean del 80% idénticas. Dado que no estaremos interesados ​​en agrupar conjuntos de secuencias disímiles, este corte nos ayudará a ahorrar espacio en disco y memoria. También noten que necesitamos las secuencias alineadas para calcular la matriz de distancia.	mothur > dist.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta, cutoff=0.20)	mothur > cluster(column=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.dist, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table)El paso del `clúster` genera el archivo `stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.an.unique_list.list` que contiene la membresía de cada OTU.A continuación queremos saber cuántas secuencias hay en cada OTU de cada grupo y podemos hacerlo usando el comando `make.shared`. Aquí le decimos a `Mothur` que realmente estamos interesados ​​en el nivel de corte 0.03:	mothur > make.shared(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table, label=0.03)Esto crea un archivo `.shared` que contiene el conteo OTU (contenido similar al de la tabla OTU en QIIME).Si abrimos el archivo `.shared`, podemos ver cuantas OTU hay...

### 3. Asignación de Taxonomía
En `Mothur`, la asignación de taxonomía puede hacerse usando el comando `classify.otu` que toma la lista OTUs y `count_table` (para mantener un registro de los recuentos de reads en cada OTU) y busca los resultados que obtuvimos de `classify.seqs` para asignar OTUs a taxones.	mothur > classify.otu(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.taxonomy, label=0.03)Puedes ver los resultados de la asignación de taxonomía en `stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy`. Vaya a la otra ventana de terminal y emita el siguiente comando:	less stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy	OTU     Size    Taxonomy
### 4. Construir la Tabla OTUEn `Mothur`, las tablas OTU están representadas por los archivos `.shared`. Podemos convertir archivos `.shared` a tablas OTU compatibles con QIIME en formato BIOM con la siguiente línea de comando:	mothur > make.biom(shared=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.shared, constaxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.02.cons.taxonomy)
	
	get.oturep(column=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.dist,list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.list,fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta,name=stability.trim.contigs.good.names,label=0.03)
	
***Aqui VA el archivo LISTO*** 

### 5. Alineamiento de SecuenciaEn `Mothur`, el alineamiento de secuencia se hace previo como parte del pre-procesamiento. Esto permitie a `Mothur` realizar *de novo* OTU *picking* en los datos de secuencias.

### 6. Análisis FilogenéticoSi están interesado en usar métodos que dependen de un árbol filogenético como el cálculo de la diversidad filogenética o los comandos `unifrac`, tendrás que generar un árbol. En `Mothur`, `clearcut` se utiliza para generar un árbol filogenético de las OTUs.	mothur > dist.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta, output=lt, processors=8)	mothur > clearcut(phylip=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.dist)Esto genera un archivo `.tre` = `stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.tre`. Puedes abrir el archivo `.tre` usando el programa `FigTree`.![FigTree](https://github.com/microgenomics/Workshop-UTA/blob/master/images/figtree.png?raw=true)