![UNAB_CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

![UCDavisChile](https://github.com/microgenomics/Workshop-UTA/blob/master/images/UCDavisChile.jpg?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon)

[http://www.unab.cl](http://www.unab.cl) - [http://www.cbib.cl](http://www.cbib.cl) - [www.castrolab.org](http://www.castrolab.org) - [http://www.ucdavischile.org](http://www.ucdavischile.org)

---

###### Workshop_UTA - Día 2

# Introducción a Mothur, SILVA y GreenGenes

## MOTHUR *workflow*


### Necesitamos los siguientes archivos de entrada (*inputs*)

#### 28 archivos FASTQ:
 `J0X_S353_L001_R1_001.fastq.gz` y `J0X_S354_L001_R2_001.fastq.gz` - correspondientes a los *reads* *forward* (R1) y *reverse* (R2) de **12 muestras de suelo**.
+ `mockA-041717_S91_L001_R1/2_001.fastq.gz` - **muestras Mock Community**.
+ `waterA-041717_S93_L001_R1/2_001.fastq.gz` - **muestras de agua**.
 `silva.nr_v123.v4.align` - [https://www.arb-silva.de](https://www.arb-silva.de) Consiste en **archivos taxonómicos** (`.tax`) y **secuencias de referencia** representativas (`.fasta`).
 `trainset9_032012.pds.tax` y `trainset9_032012.pds.fasta` - [https://rdp.cme.msu.edu](https://rdp.cme.msu.edu) Consisten en **archivos RDP** formateados para `Mothur` con **secuencias de referencia 16S** y **asignaciones taxonómicas**.

Lo que verémos a continuación es una adaptación de [Mothur MiSeq SOP](https://mothur.org/wiki/MiSeq_SOP) donde se puede encontrar el tutorial completo.

Primero, crearemos nuestra carpeta de trabajo...

	mkdir /Users/jaimealarcon/Desktop/workshop_mothur
Luego de acceder a nuestro directorio `workshop_mothur`, copiaremos los archivos necesarios... 

![mothur_inicio](https://github.com/microgenomics/Workshop-UTA/blob/master/images/mothur_inicio.png?raw=true)

### 1. Pre-procesamiento 

#### Ensamble *paired-end*

	make.contigs(file=stability.files, processors=4)
`Mothur` utiliza su propio algoritmo simple para fusionar las reads emparejadas. Se mostrarán muchos mensajes en la pantalla, pero en lo que vale la pena poner atención, es a la lista de archivos de salida (*outputs*) generados por mothur.

![archivos_de_salida]()

`Mothur` siempre dará una lista de los archivos que generó - una característica bastante útil. Hay que tener en cuenta que todas las *reads* están ahora en un sólo archivo FASTA llamado `stability.trim.contigs.fasta`. La información que enlaza *reads* a las muestras esta en `stability.contigs.groups`. Podemos echar un vistazo a los resultados del ensamblaje.

	summary.seqs(fasta=stability.trim.contigs.fasta)

![pantalla1]()

	screen.seqs(fasta=stability.trim.contigs.fasta, group=stability.contigs.groups, maxambig=0, minlength=100, maxlength=300)
	
Éste comando elimina las secuencias que son demasiado largas o contienen bases ambiguas (N). Una vez más, ejecutaremos el comando `summary.seqs` para comprobar la salida.

	summary.seqs(fasta=stability.trim.contigs.good.fasta)
	
**Notar cuantas secuencias tenemos ahora, que pasaron el filtro.**

#### Reducir el número de secuencias redundantes

#### Alineación de secuencias a base de datos de referencia

	mothur > summary.seqs(fasta=stability.trim.contigs.good.unique.align, count=stability.trim.contigs.good.count_table)

Podemos ver la región en que la mayoría de las secuencias se alinean. Eliminaremos secuencias mal alineadas y eliminaremos las columnas *gap-only* escribiendo:
### 2. OTU *Picking*

### 3. Asignación de Taxonomía

### 4. Construir la Tabla OTU

### 5. Alineamiento de Secuencia

### 6. Análisis Filogenético

![bot](https://github.com/microgenomics/Workshop-UTA/blob/master/images/huinchaunab.jpg?raw=true)