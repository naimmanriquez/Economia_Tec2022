## Introducción
En este apartado se exploran los conceptos de la demanda usando programación en R, particularmente de manera gráfica. 

Estructuralmente, utilizaremos 10 empresas hipotéticas para analizar la demanda de una aplicación hipotética de software que cobra una tarifa mensual por licencia. Aquí hay una buena combinación de empresas que van desde pequeñas a grandes con una combinación diversa de qué precio están dispuestos a pagar por cuántas licencias.

Luego examinamos la demanda del mercado y desglosamos el concepto de elasticidad de la demanda.

### Código a utilizar

#### Instalar librerias
Necesitaremos las librerias readxl y tidyverse para este proyecto. 

Si no los tienes instalados en tu entorno R, simplemente elimina el signo " # " antes de las líneas de código " install.packages... " 

```ruby
# install.packages("readxl") 
# install.packages("tidyverse")

# Cargar librerias 
require(readxl) 
require(tidyverse)
```

#### Cargar datos
Vamos a utilizar los datos Demanda_data.xls que se encuentran en la carpeta de datos. 

```ruby
# Importar datos 
Demanda_Data <- read_excel("Demanda_Data.xlsx")
```

Posteriormente verificamos los datos que hemos cargado

```ruby
# Checar datos 
head(Demanda_Data)
```

### Gráfica de prueba
Cuando se trabaja con datos económicos, siempre es una buena idea hacer un gráfico rápido para ver como se comportan los datos y ver alguna posible anomalia.

```ruby
plot(Demanda_Data$Qd_6, Demanda_Data$Price, type = "l", main = "Grafica", xlab = "Cantidad", ylab = "Precio")
```
### Trazar curvas de demanda individuales
El siguiente paso es ver las curvas de demanda individuales al mismo tiempo. Para esto usamos ggplot y la función stack para compilar los datos. La función stack() básicamente combina varios vectores en un único vector. En este caso, hacemos un solo vector con las demandas individuales. 

Finalmente, cambiaremos los nombres de columna de los generados por la función stack() para hacerlos un poco más apropiados para nuestro proyecto.

```ruby
# Manipular los datos para una mejor visualizacion, wrangling data
Wrangled_Data <- data.frame(Price = Demanda_Data$Price, stack(Demanda_Data[2:11]))
names(Wrangled_Data)[2] <- "Quantity"
names(Wrangled_Data)[3] <- "Qd_num"
# Checamos los datos
head(Wrangled_Data)
```
