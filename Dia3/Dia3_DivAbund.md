![UNAB_CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

![UCDavisChile](https://github.com/microgenomics/Workshop-UTA/blob/master/images/UCDavisChile.jpg?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon)

[http://www.unab.cl](http://www.unab.cl) - [http://www.cbib.cl](http://www.cbib.cl) - [www.castrolab.org](http://www.castrolab.org) - [http://www.ucdavischile.org](http://www.ucdavischile.org)

###### Workshop_UTA - Día 3

# Diversidad Alfa y Beta, Métodos de Distancia y Escalamiento Multidimensional, Análisis de Abundancia Diferencial, y Redes de co-ocurrencia.

## Análisis en Phyloseq Esta es una demostración de cómo importar datos de *amplicon microbiome* en R usando `Phyloseq` y ejecutar algunos análisis básicos para entender la diversidad y composición de la comunidad microbiana en sus muestras. Más información acerca de `Phyloseq` [aquí](https://bioconductor.org/packages/release/bioc/html/phyloseq.html).En este tutorial, estamos trabajando con datos de Illumina 16S que ya han sido procesados ​​en una tabla de OTUs y taxonomía del pipeline `Mothur`. `Phyloseq` tiene una variedad de opciones de importación si procesamos los datos de secuencia con una tubería diferente.En este tutorial, aprenderemos cómo importar una tabla OTU y metadatos de muestra en `R` usando el paquete `Phyloseq`. Realizarémos algunos análisis exploratorios básicos, analizando la composición taxonómica de nuestras muestras y visualizando la disimilitud entre nuestras muestras en un espacio de baja dimensión mediante ordenaciones. Por último, estimaremos la diversidad alfa (riqueza y uniformidad) de nuestras muestras.

Abre `Rstudio`... carga las librerías necesarias y define el directorio de trabajo, así:

	# cargar librerías...	library(phyloseq)	library(ggplot2)	library(scales)	library(grid)	library(ape)	library(maps)	library(phytools)	library(reshape2)	# directorio de trabajo...	setwd("~/Desktop/Mothur/graficar")

Importa los archivos de entrada (*inputs*)...

	# Outputs de mothur serán inputs para R...	read.newick("arbolN") -> tree	import_biom("mothur.biom",tree) -> physeq	# Importar metadata	read.table("metadata.tsv",h=T,row.names = 1, sep = ";") -> meta	sample_data(physeq)<-meta	physeq
	
	phyloseq-class experiment-level object	otu_table()   OTU Table:         [ 567 taxa and 12 samples ]	sample_data() Sample Data:       [ 12 samples by 4 sample variables ]	tax_table()   Taxonomy Table:    [ 567 taxa by 6 taxonomic ranks ]	phy_tree()    Phylogenetic Tree: [ 567 tips and 566 internal nodes ]		# Para saber los títulos de las columnas...
	rank_names(physeq)	[1] "Rank1" "Rank2" "Rank3" "Rank4" "Rank5" "Rank6"
	
	# Podemos re-nombrar las columnas, así:	colnames(tax_table(physeq)) <- c("Kingdom", "Phylum", "Class", "Order", "Family", "Genus")	Ahora debemos agregar los OTUs como una columna mas en la tabla, así:

	tax_table(physeq) <- cbind(tax_table(physeq), rownames(tax_table(physeq)))	colnames(tax_table(physeq)) <- c("Kingdom", "Phylum", "Class", "Order", "Family", "Genus", "OTUID")### Diversidad alfa

	alpha_meas = c("Observed", "Chao1")	plot_richness(physeq)	p <- plot_richness(physeq, "Extraction", "Precipitation", measures=alpha_meas)	p + geom_boxplot(data=p$data, aes(x=Extraction, y=value, color=NULL), alpha=.1)![alphadiv](https://github.com/microgenomics/Workshop-UTA/blob/master/images/alphadiv.png?raw=true)

	# Barplots	# Get the most abundant N taxa	TopNOTUs <- names(sort(taxa_sums(physeq), TRUE)[1:20]) 	physeq.top20   <- prune_taxa(TopNOTUs, physeq)	print(physeq.top20)		phyloseq-class experiment-level object	otu_table()   OTU Table:         [ 20 taxa and 12 samples ]	sample_data() Sample Data:       [ 12 samples by 4 sample variables ]	tax_table()   Taxonomy Table:    [ 20 taxa by 7 taxonomic ranks ]	phy_tree()    Phylogenetic Tree: [ 20 tips and 19 internal nodes ]	
	# Get counts from phyloseq object	glom_otu <- otu_table(physeq.top20)		# Get tax from phyloseq object	glom_tax <- tax_table(physeq.top20)[,7]	as.data.frame(cbind(glom_tax, glom_otu)) -> ptablew	ptablew$OTUID -> rownames(ptablew)	ptablew$OTUID <- NULL	lapply(ptablew, as.numeric) -> ptablewnum	as.data.frame(ptablewnum) -> ptablewnum	cbind(glom_tax, ptablewnum) -> tableA	tableA$OTUID -> rownames(tableA)	tableA$OTUID <- NULL	# This is to change raw counts to proportions	abundant <-(t(tableA))	abundant <- sweep(abundant, 1, rowSums(abundant), FUN='/')	abundant <-as.data.frame(t(abundant))	abundant$taxa = rownames(abundant)	rownames(abundant) = NULL		cl <- colors(distinct = TRUE)	set.seed(15887) # to set random generator seed	kolor <- sample(cl, 20)		melt(abundant, id.vars=c("taxa"), measure.vars=c("m01",  "m02",  "m03",  "m04",  "m05",  "m06",  "m07",  "m08",  "m09",  "m10",  "m11",  "m12"), variable.name="sample", 	value.name="proportion") -> long_abundances	
	# And finally plot	ggplot(data=long_abundances,aes(x=sample,y=proportion,fill=taxa,stat="identity")) + 	geom_bar(stat="identity", colour="grey", size = 0.2) +	guides(fill = guide_legend(ncol = 1,bycol=TRUE,title="Microbial taxa")) + 	theme(legend.position = "right") + 	xlab("Samples") + 	ylab("Proportion of Mapped Reads") + 	theme(legend.key.size = unit(4,"mm"),legend.margin=unit(5,"mm")) + 	theme(legend.text = element_text(colour="grey35", size = 10, face = "italic")) + 	theme(legend.title = element_text(colour="grey20", size = 12, face = "bold")) + 	theme(axis.title = element_text(face="bold", colour="#990000", size=25)) + 	theme(axis.text.x  = element_text(size=10, angle = 45, hjust = 1)) +	theme(axis.text.y  = element_text(size=16)) +	scale_fill_manual(name = "taxa", values=kolor)-> q	q![popmapre](https://github.com/microgenomics/Workshop-UTA/blob/master/images/popmapre.png?raw=true)	plot_heatmap(physeq.top20)![heatmapabund](https://github.com/microgenomics/Workshop-UTA/blob/master/images/heatmapabund.png?raw=true)

	plot_bar(physeq.top20, fill="Genus")![genusabundbar](https://github.com/microgenomics/Workshop-UTA/blob/master/images/genusabundbar.png?raw=true)

	plot_tree(physeq.top20, color="Genus", shape="Location", size="abundance")![genusabundloc](https://github.com/microgenomics/Workshop-UTA/blob/master/images/genusabundloc.png?raw=true)

## Análisis en Shiny-PhyloseqPara usar `Shiny-Phyloseq` necesitamos tener un *front-end* (navegador) y un *back-end* (`R`).

### *Front-End*En el *front-end* de `Shiny-Phyloseq`, la aplicación web GUI (*Graphical User Interface*), se ejecutará en cualquier navegador web moderno.**Navegadores soportado**:
+ Firefox+ Chrome+ Safari+ Otros navegadores probablemente estén bien para conectarse a una sesión de `Shiny-Phyloseq`.**Navegador NO soportado**:
+ Versiones anteriores de Microsoft Internet Explorer

### *Back-End*Para ejecutar el *back-end* de `Shiny-Phyloseq`, debes tener la última versión de `R` instalada, así como varios paquetes adicionales.Para actualizar o instalar `R` por primera vez, descarga `R` [aquí](https://cloud.r-project.org), recomendamos que selecciones la penúltima versión disponible.Una vez que haya instalado o actualizado la última versión de `R`, simplemente correr `Shiny-Phyloseq`, automáticamente se instalarán paquetes requeridos en `R`. Recuerda que para ésto es necesario una conexión de internet estable y permisos de instalación.El siguiente código `R` correrá `Shiny-Phyloseq`... en `Rstudio` escribe:	install.packages("shiny")	shiny::runGitHub("shiny-phyloseq","joey711")![datupselect](https://github.com/microgenomics/Workshop-UTA/blob/master/images/datupselect.png?raw=true)

En *Upload Biom-Format File*, selecciona la ubicacion de nuestro archivo `.biom` y explora los menús de `Shiny-Phyloseq` para el análisis de Diversidad Alfa (Alpha Diversity).

## Análisis en MetaCoMET (Metagenomics Core Microbiome Exploration Tool)

Accede a la página de MetaCoMET **[aquí](http://probes.pw.usda.gov/MetaCoMET/index.php)**.

![metacomet](https://github.com/microgenomics/Workshop-UTA/blob/master/images/metacomet.png?raw=true)

Haz clic en `Start`...

![metacomet2](https://github.com/microgenomics/Workshop-UTA/blob/master/images/metacomet2.png?raw=true)

Dirígete a la opción `Choose File...` en *Please choose BIOM file (required)* y selecciona tu archivo `.biom` para comenzar a explorar las opciónes que nos entrega la plataforma...---![bot](https://github.com/microgenomics/Workshop-UTA/blob/master/images/huinchaunab.jpg?raw=true)