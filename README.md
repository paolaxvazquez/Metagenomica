# Metagenómica 
Proyecto desarrollado por alumnos y docentes del Laboratorio Nacional de Secuenciación Genomica del Instituto Tecnológico y de Estudios superiores de Monterrey.

Este programa fue diseñado para hacer un análisis metagenómico de 16S y shotgun, a partir de datos en formato TXT o CSV provenientes de un análisis taxonómico de [KRAKEN 2](https://github.com/DerrickWood/kraken2.) 

Este programa busca hacer análisis estadísticos y permitir al usuario la visualización de los datos taxonómicos arrojados por KRAKEN 2. 

# Utils y librerías 
La primera parte del código está diseñada para declarar las diferentes funciones que serán utilizadas para la creación de barplots y boxplots que formarán parte de los resultados de este análisis.

Es importante correr estas lineas del código ya que algunas de las funciones no se encuentran en ninguna librería y son necesarias para el funcionamiento del programa.

A su vez, también se instalan (en caso de no tenerlas) y se llama a las librerías que serán utilizadas para hacer los diferentes análisis estadísticos.  

En la línea 
```Rscript
geom_boxplot(fill=sample(mypal, length(treatcol)),fatten=1, outlier.shape = NA)
```
se generan colores aleatorios para los boxplots de los indices shannon y simpson. Es posible cambiar el parámetro `fill =` para poder poner colores específicos a las variables. Esto se debe de hacer de la siguiente forma: `fill = c(C1,C2,C3,...`. Se recomienda usar los códigos de hexadecimales de color para poder obtener colores más específicos y personalizados. 

# Datos
En la segunda sección del código se declara el pathway de la carpeta donde se encuentran los datos con los cuales va a trabajar el programa. Es importante mencionar que dentro de esta carpeta se van a guardar los diferentes documentos e imágenes resultantes de este análisis. Por esto se recomienda destinar Carpetas específicas para cada análisis indívidual. 

Dentro de esta sección se hace un tratado de los datos para asegurar que el formato es el correcto y R o Rstudio no arroje códigos de error y códigos de advertencia. El input de este código debe de ser un archivo **.csv** o **.txt** proveniente del análisis taxonómico de [KRAKEN 2](https://github.com/DerrickWood/kraken2.). 

En la línea
```Rscript
raw_data <- read.table(file.choose(), header = T, sep = "\t",quote = "\"", stringsAsFactors = F, fill = F)
``` 

se debe de modificar el parámetro "sep" dependiendo del tipo de documento que se esté usando: para **.txt** se usa `"\t"` y para **.csv** se usa `","`.  


# Filtrado

En esta parte del programa se realiza una matriz donde los datos están organizados de acuerdo a sus niveles taxonómicos. Todos los datos que correspondan a un cierto nivel taxónomico con agrupados y se desplega información sobre la muestra a la que pertenece, su nombre científico y su valor de abundancia. 
Posteriormente este set de datos continua agrupado por nivel taxónomico, pero únicamente los diez valores de abundancia más altos para cada muestra de cada nivel taxonómico son los que serán tratados en el resto del script. "Metatop" es la tabla de datos donde se encuentran los datos más abundantes para cada muestra según su nivel taxonómico.
A partir de la variable Metatop se realiza un documento .csv para cada nivel taxonómico y sus datos correspondientes.


# Gráficas Barplot
Se utiliza una función que se declaró al inicio llamada "genbarplot" que cuenta con parámetros de los datos a tratar, el % de especies que se quiere generalizar como 'otro', el nombre genérico de los boxplots y la paleta de colores. Esta función realiza gráficas para cada nivel taxonómico donde se visualicen los organismos de esta clasificación que se presentan en mayor medida para cada muestra. La cantidad de gráficas que se generan depende de la cantidad de niveles taxónomicos que se indicaron.

# Diversity tables and boxplots
La función a llamar "genDiversityIndexTable" calcula el valor de Shannon y Simpson para cada muestra según los niveles taxonómicos. Arroja una matriz donde de acuerdo al nivel taxonómico se crea una tabla de estos valores para cada muestra.
Posteriormente esta matriz se modifica para que las muestras sean agrupadas a partir de sus similitudes (estas normalmente vienen indicadas en los nombres o claves) para que puedan ser ...
