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
