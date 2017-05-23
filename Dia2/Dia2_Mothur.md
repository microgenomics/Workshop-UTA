![UNAB_CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

![UCDavisChile](https://github.com/microgenomics/Workshop-UTA/blob/master/images/UCDavisChile.jpg?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon)

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
Luego de acceder a nuestro directorio `workshop_mothur`, copiaremos los archivos necesarios... 	cp –r /Users/jaimealarcon/Desktop/materiales_mothur /Users/jaimealarcon/Desktop/workshop_mothurY le daremos comienzo a `Mothur`...	./mothur

![mothur_inicio](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur_inicio.png?raw=true)

### 1. Pre-procesamiento 

#### Ensamble *paired-end*Debido a que tenemos *reads* *paired-end* pareadas, una forma de mejorar la calidad general de la secuencia es ensamblando estas *reads*. La orden para hacerlo en `Mothur` es:

	make.contigs(file=stability.files, processors=4)
`Mothur` utiliza su propio algoritmo simple para fusionar las reads emparejadas. Se mostrarán muchos mensajes en la pantalla, pero en lo que vale la pena poner atención, es a la lista de archivos de salida (*outputs*) generados por mothur.

![archivos_de_salida]()

`Mothur` siempre dará una lista de los archivos que generó - una característica bastante útil. Hay que tener en cuenta que todas las *reads* están ahora en un sólo archivo FASTA llamado `stability.trim.contigs.fasta`. La información que enlaza *reads* a las muestras esta en `stability.contigs.groups`. Podemos echar un vistazo a los resultados del ensamblaje.

	summary.seqs(fasta=stability.trim.contigs.fasta)

![pantalla1]()Este comando que veremos una y otra vez resume el contenido de un archivo FASTA. Poner atención al total de secuencias ensambladas con éxito. También podemos notar que la mayoría de las *reads* están dentro del tamaño esperado (~250bps), pero algunas *reads* son demasiado largas. Éstas reads están mal ensambladas. Mejoraremos aún más la calidad de la secuencia de entrada emitiendo el siguiente comando.

	screen.seqs(fasta=stability.trim.contigs.fasta, group=stability.contigs.groups, maxambig=0, minlength=100, maxlength=300)
	
Éste comando elimina las secuencias que son demasiado largas o contienen bases ambiguas (N). Una vez más, ejecutaremos el comando `summary.seqs` para comprobar la salida.

	summary.seqs(fasta=stability.trim.contigs.good.fasta)
	
**Notar cuantas secuencias tenemos ahora, que pasaron el filtro.**![pantalla2]()

#### Reducir el número de secuencias redundantesEn sus muestras, habrá secuencias que son idénticas. Podemos emitir los siguientes comandos para combinar secuencias duplicadas en una y mantener un registro del número de copias de cada una de las secuencias duplicadas.	unique.seqs(fasta=stability.trim.contigs.good.fasta)	count.seqs(name=stability.trim.contigs.good.names, group=stability.contigs.good.groups)Esto generará `stability.trim.contigs.good.unique.fasta` que contiene sólo una copia de cada una de las *reads* duplicadas. Podemos ver cuantas secuencias únicas hay. El archivo `stability.trim.contigs.good.names` mantiene un registro de las secuencias que se duplican. El comando `count.seqs` genera un archivo `count_table` - `stability.trim.contigs.good.count_table` que sólo mantiene un registro del número de copias de cada una de las secuencias duplicadas. En general, el archivo `.names` le da la membresía de cada grupo de secuencias.

#### Alineación de secuencias a base de datos de referenciaA continuación, alinearemos las secuencias con la alineación de referencia silva.	mothur > align.seqs(fasta=stability.trim.contigs.good.unique.fasta, reference=silva.nr_v123.v4.align.txt, flip=t)Veremos el resumen de alineación con:

	mothur > summary.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table)

Podemos ver la región en que la mayoría de las secuencias se alinean. Eliminaremos secuencias mal alineadas y eliminaremos las columnas *gap-only* escribiendo:	mothur > fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table, summary=stability.trim.contigs.good.unique.summary, start=8, end=9582, maxhomop=8	mothur > filter.seqs(fasta=stability.trim.contigs.good.unique.good.align, vertical=T, trump=.)Los resultados del alineamiento y filtro son:	Length of filtered alignment: ###	Number of columns removed: ###	Length of the original alignment: ###	Number of sequences used to construct filter: ###Después de alinear y recortar, algunas de las *reads* ahora pueden estar duplicadas, así que ejecutaremos el comando `unique.seqs` de nuevo.	mothur > unique.seqs(fasta=stability.trim.contigs.good.unique.good.filter.fasta, count=stability.trim.contigs.good.good.count_table)Ahora tenemos secuencias alineadas en `stability.trim.contigs.good.unique.good.filter.unique.fasta` y las secuencias duplicadas son rastreadas en `stability.trim.contigs.good.unique.good.filter.count_table`.Utilizaremos el comando `pre.cluster` para eliminar aún más las secuencias raras que son muy similares a las secuencias abundantes (dentro de las diferencias de 2nt). Es probable que estas secuencias raras se deban a errores de secuenciación.	mothur > pre.cluster(fasta=stability.trim.contigs.good.unique.good.filter.unique.fasta, count=stability.trim.contigs.good.unique.good.filter.count_table, diffs=2)#### Eliminar las secuencias quiméricas y no 16SA continuación utilizaremos Mothur para eliminar las secuencias quiméricas.	mothur > chimera.uchime(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.count_table, dereplicate=t)	mothur > remove.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.fasta, accnos=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.accnos)Usando el comando `classify.seqs`, podemos eliminar secuencias que no parecen una secuencia 16S de bacterias. Tenga en cuenta que este paso requiere la base de datos RDP (`trainset9_032012.pds.fasta` y el archivo de taxonomía) como referencia.	mothur > classify.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.count_table, reference=trainset9_032012.pds.fasta, taxonomy=trainset9_032012.pds.tax, cutoff=80)	mothur > remove.lineage(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.taxonomy, taxon=Chloroplast-Mitochondria-unknown-Archaea-Eukaryota)El comando `classify.seqs` es similar al OTU picking de referencia cerrada. Las secuencias son emparejadas con una secuencia de referencia en la base de datos y las secuencias sin similitud pueden eliminarse mediante los comandos `remove.lineage` o `remove.seqs`.
### 2. OTU *Picking*Ya realizado el pre-procesamiento, la siguiente etapa de nuestro análisis es hacer OTUs. `Mothur` toma un enfoque *de novo* OTU *picking*, primero calcula las distancias pair-wise de cada una de las secuencias, luego agrupandolas en OTUs usando agrupación jerárquica. Observen que al especificar un corte de 0.20, sólo se registrarán pares de secuencias que sean del 80% idénticas. Dado que no estaremos interesados ​​en agrupar conjuntos de secuencias disímiles, este corte nos ayudará a ahorrar espacio en disco y memoria. También noten que necesitamos las secuencias alineadas para calcular la matriz de distancia.	mothur > dist.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta, cutoff=0.20)	mothur > cluster(column=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.dist, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table)El paso del `clúster` genera el archivo `stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.an.unique_list.list` que contiene la membresía de cada OTU.A continuación queremos saber cuántas secuencias hay en cada OTU de cada grupo y podemos hacerlo usando el comando `make.shared`. Aquí le decimos a `Mothur` que realmente estamos interesados ​​en el nivel de corte 0.03:	mothur > make.shared(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table, label=0.03)Esto crea un archivo `.shared` que contiene el conteo OTU (contenido similar al de la tabla OTU en QIIME).Si abrimos el archivo `.shared`, podemos ver cuantas OTU hay...

### 3. Asignación de Taxonomía
En `Mothur`, la asignación de taxonomía puede hacerse usando el comando `classify.otu` que toma la lista OTUs y `count_table` (para mantener un registro de los recuentos de reads en cada OTU) y busca los resultados que obtuvimos de `classify.seqs` para asignar OTUs a taxones.	mothur > classify.otu(list=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.list, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table, taxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pds.wang.taxonomy, label=0.03)Puedes ver los resultados de la asignación de taxonomía en `stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy`. Vaya a la otra ventana de terminal y emita el siguiente comando:	less stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy	OTU     Size    Taxonomy
### 4. Construir la Tabla OTUEn `Mothur`, las tablas OTU están representadas por los archivos `.shared`. Podemos convertir archivos `.shared` a tablas OTU compatibles con QIIME en formato BIOM con la siguiente línea de comando:	mothur > make.biom(shared=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.shared, constaxonomy=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy)

### 5. Alineamiento de SecuenciaEn `Mothur`, el alineamiento de secuencia se hace previo como parte del pre-procesamiento. Esto permitie a `Mothur` realizar *de novo* OTU *picking* en los datos de secuencias.

### 6. Análisis FilogenéticoSi están interesado en usar métodos que dependen de un árbol filogenético como el cálculo de la diversidad filogenética o los comandos `unifrac`, tendrás que generar un árbol. En `Mothur`, `clearcut` se utiliza para generar un árbol filogenético de las OTUs.	mothur > dist.seqs(fasta=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta, output=lt, processors=8)	mothur > clearcut(phylip=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.dist)Esto genera un archivo `.tre` = `stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.tre`. Puedes abrir el archivo `.tre` usando el programa `FigTree`.![FigTree](https://github.com/microgenomics/Workshop-UTA/blob/master/images/figtree.png?raw=true)---

![bot](https://github.com/microgenomics/Workshop-UTA/blob/master/images/huinchaunab.jpg?raw=true)
