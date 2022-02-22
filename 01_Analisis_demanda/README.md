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
### Trazar la demanda del mercado
La visualización de nuestros datos se puede hacer con un poco de ayuda de ggplot() simplemente trazando la suma de nuestras cantidades demandadas para cada precio.
Aquí está el código:

```ruby
ggplot(data = Market_Demand, aes(x = Market_Demand, y = Price)) +
  geom_line(color = "steelblue", size = 1) +
  geom_point(color = "steelblue") +
  geom_vline(xintercept = 0) +
  geom_hline(yintercept = 0)
```
Cuando miramos esto, notamos instantáneamente que en ciertos rangos de esta curva de demanda de mercado, un cambio en el precio no producirá el mismo cambio en la cantidad demandada por el mercado en cada punto de la curva porque no es lineal.
Ahora, echemos un vistazo a eso e introduzcamos el concepto de elasticidad.

### Elasticidad
La elasticidad de la demanda es simplemente la idea de ver cómo cambia la combinación de precios y cantidades entre dos puntos de una curva. Puede expresarse matemáticamente como el cambio porcentual en la cantidad dividido por el cambio porcentual en el precio. La pregunta a responder es:
¿en cuánto cambia la cantidad demandada por cada unidad de cambio de precio?

```ruby
# Rangos de precios y elasticidades
# 10-6.5 Rango 1
# 6-4 Rango 2
# 3.5-2 Rango 3
# 1.5-0 Rango 4
Market_Demand$Elasticity_Zone <- as.character(c(1,1,1,1,1,1,1,1,2,2,2,2,2,3,3,3,3,4,4,4,4))
Market_Demand
```
### Trazar la demanda del mercado con zonas de elasticidad

Si bien ggplot puede ser frustrante a veces, ofrece algunas funciones excelentes. Vamos a agregar a nuestro código de demanda de mercado anterior agregando color = Elasticity_Zone a la función aes() para que ggplot sepa asignar un color diferente a cada zona.
También agregaremos la función geom_smooth() con el parámetro method = “lm” para que haga modelos lineales para cada zona de elasticidad. Por ahora, esto muestra claramente que diferentes secciones de la curva de demanda pueden tener pendientes y números de elasticidad bastante diferentes.

```ruby
# Grafica de demanda de mercado con rangos/elasticidades
ggplot(data = Market_Demand, aes(x = Market_Demand, y = Price, color = Elasticity_Zone)) +
        geom_line(size = 1) +
        geom_point() +
        geom_smooth(method = "lm") +
        geom_vline(xintercept = 0) +
        geom_hline(yintercept = 0) +
        ggtitle("Demanda de mercado con rangos de precios/elasticidades") +
        theme(plot.title = element_text(hjust = 0.5))
```
