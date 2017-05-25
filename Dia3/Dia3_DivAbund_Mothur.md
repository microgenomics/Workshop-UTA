![UNAB_CBIB](https://github.com/microgenomics/Workshop-UTA/blob/master/images/logocbibhorizontal.png?raw=true)

![UCDavisChile](https://github.com/microgenomics/Workshop-UTA/blob/master/images/UCDavisChile.jpg?raw=true)

## Microbial Genomics Lab

[Eduardo Castro-Nallar](https://github.com/ecastron) - [Jaime Alarcon](https://github.com/jaimealarcon)

[http://www.unab.cl](http://www.unab.cl) - [http://www.cbib.cl](http://www.cbib.cl) - [www.castrolab.org](http://www.castrolab.org) - [http://www.ucdavischile.org](http://www.ucdavischile.org)

###### Workshop_UTA - Día 3

# Diversidad Alfa y Beta, Métodos de Distancia y Escalamiento Multidimensional, Análisis de Abundancia Diferencial, y Redes de co-ocurrencia - Mothur

## Análisis de Datos en Mothur

Ahora que tenemos nuestro archivo de taxonomía y nuestra tabla de OTU, cambiaremos el nombre de estos archivos para hacer el análisis un poco más legible...

	mothur > system(mv stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.shared stability.an.shared)
	mothur > system(mv stability.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.0.03.cons.taxonomy stability.an.cons.taxonomy)

...para que cada muestra tenga el mismo número de reads:

	mothur > count.groups(shared=stability.an.shared)
	mothur > sub.sample(shared=stability.an.shared, size=2440)

### Diversidad alfa 

### Diversidad Beta

Primero, utilizaremos la función `dist.shared` para calcular las distancias entre pares de muestras. **ThetaYC** es una medida de la estructura de la comunidad y **Jclass** es una medida de pertenencia a la comunidad, es decir, presencia o ausencia de OTU.

![arbol]() 
	# Para salir de Mothur, simplemente escribe...
	mothur > quit

---

![bot](https://github.com/microgenomics/Workshop-UTA/blob/master/images/huinchaunab.jpg?raw=true)