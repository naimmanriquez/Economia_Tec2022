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

Con eso, podemos usar mucho más fácilmente la función facet_wrap() para mostrar elegantemente todas las curvas de demanda individuales. La función facet_wrap() sirve para realizar un gráfico por diferentes grupos o categorías, en nuestro caso para las demandas individuales.

```ruby
# Grafica de curvas de demanda individuales
ggplot(data = Wrangled_Data, aes(x = Quantity, y = Price)) +
        geom_line(color = "steelblue", size = 1) +
        geom_point(color = "steelblue") +
        facet_wrap(. ~ Qd_num)
```

Lo que se puede observar es que, a medida que el precio baja, la mayoría de las empresas comienzan a comprar más licencias. Algunos realmente no están comprando muchas licencias sin importar el precio. 

Uno de los conceptos fundamentales es que las empresas tienen demandas individuales. Algunas empresas simplemente valoran nuestro producto más que otras, y es posible que bajar el precio no tenga mucho impacto en su decisión de compra. Por eso, vamos a hacer un análisis de elasticidad/precio. 

### Demanda de mercado
La definición más simple de demanda de mercado es que es la suma de las curvas de demanda de todas las empresas individuales. Esto significa que si tenemos 10 empresas en nuestro mercado, sumando todas las cantidades demandadas. Para hacer esto, tendremos que usar la función rowSums. 

```ruby
# Creamos data frame con la demanda del mercado
Market_Demand <- data.frame(Price = Demanda_Data$Price, Market_Demand = rowSums(Demanda_Data[2:11]))
# Checamos los datos
head(Market_Demand)
```
