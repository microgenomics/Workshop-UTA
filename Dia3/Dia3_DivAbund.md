![UNAB_CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

![UCDavisChile](https://github.com/microgenomics/Workshop-UTA/blob/master/images/UCDavisChile.jpg?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon)

[http://www.unab.cl](http://www.unab.cl) - [http://www.cbib.cl](http://www.cbib.cl) - [www.castrolab.org](http://www.castrolab.org) - [http://www.ucdavischile.org](http://www.ucdavischile.org)

###### Workshop_UTA - Día 3

# Diversidad Alfa y Beta, Métodos de Distancia y Escalamiento Multidimensional, Análisis de Abundancia Diferencial, y Redes de co-ocurrencia.

## Análisis de Datos en Mothur

Ahora que tenemos nuestro archivo de taxonomía y nuestra tabla de OTU, cambiaremos el nombre de estos archivos para hacer el análisis un poco más legible...

	mothur > system(mv stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.shared stability.an.shared)
	mothur > system(mv stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy stability.an.cons.taxonomy)

...para que cada muestra tenga el mismo número de reads:

	mothur > count.groups(shared=stability.an.shared)
	mothur > sub.sample(shared=stability.an.shared, size=2440)

### Diversidad alfa Podemos ver la diversidad dentro de cada muestra generando la **curva de rarefacción**...	mothur > rarefaction.single(shared=stability.an.shared, calc=sobs, freq=100)Esto generará archivos que terminan en `.rarefaction`, que se pueden graficar en su paquete o programa para generar gráficos favorito (vamos a intentar con Excel). Por desgracia,  rarefacción no es una medida de la riqueza, sino una medida de la **diversidad**. Si se consideran dos comunidades con la misma riqueza, pero igualdad (*eveness*) diferente después de muestrear un gran número de individuos sus curvas de rarefacción serán asíntotas al mismo valor. Dado que tienen diferentes *evennesses*, las formas de las curvas serán diferentes. Podemos generar un resumen de la alfa-diversidad con: 	mothur > summary.single(shared=stability.an.shared, calc=nseqs-coverage-sobs-invsimpson, subsample=2440)

### Diversidad Beta La diversidad beta mide las diferencias en las pertenencias de las comunidades o las estructuras de la comunidad a través de las muestras.

Primero, utilizaremos la función `dist.shared` para calcular las distancias entre pares de muestras. **ThetaYC** es una medida de la estructura de la comunidad y **Jclass** es una medida de pertenencia a la comunidad, es decir, presencia o ausencia de OTU.	mothur > dist.shared(shared=stability.an.shared, calc=thetayc-jclass, subsample=2240)A continuación, convertiremos el cálculo de distancia en un árbol bifurcado.	mothur > tree.shared(phylip=stability.an.thetayc.0.03.lt.ave.dist)Podemos visualizar el árbol en FigTree de nuevo...

![arbol]() También podemos tener en cuenta la información filogenética al medir la diversidad beta. Ésto se hace utilizando `UniFrac`...	mothur > unifrac.unweighted(tree=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.tre, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table, distance=lt, processors=2, random=F, subsample=2240)	mothur > unifrac.weighted(tree=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.tre, count=stability.trim.contigs.good.unique.good.filter.unique.precluster.uchime.pick.pick.count_table, distance=lt, processors=2, random=F, subsample=2240)Estos comandos distanciarán matrices (`stability.phylip.tre1.unweighted.ave.dist` y `stability.phylip.tre1.weighted.ave.dist`) que pueden analizarse usando todos los enfoques de diversidad beta descritos anteriormente para los análisis basados ​​en OTU. Por ejemplo, podríamos usar el comando siguiente para generar un árbol de bifurcación de ejemplo...	mothur > tree.shared(phylip=stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.phylip.tre1.unweighted.phylip.dist)
	# Para salir de Mothur, simplemente escribe...
	mothur > quit

## Análisis en Phyloseq Esta es una demostración de cómo importar datos de *amplicon microbiome* en R usando `Phyloseq` y ejecutar algunos análisis básicos para entender la diversidad y composición de la comunidad microbiana en sus muestras. Más demos de este paquete están disponibles desde los autores [aquí]().En este tutorial, estamos trabajando con datos de Illumina 16S que ya han sido procesados ​​en una tabla de OTUs y taxonomía del pipeline `Mothur`. `Phyloseq` tiene una variedad de opciones de importación si procesamos los datos de secuencia con una tubería diferente.En este tutorial, aprenderemos cómo importar una tabla OTU y metadatos de muestra en `R` con el paquete `Phyloseq`. Realizarémos algunos análisis exploratorios básicos, analizando la composición taxonómica de nuestras muestras y visualizando la disimilitud entre nuestras muestras en un espacio de baja dimensión mediante ordenaciones. Por último, estimaremos la diversidad alfa (riqueza y uniformidad) de nuestras muestras.

