# Análisis de Datos de Airbnb en R

## Cargar datos y mostrar las primeras filas
- Cargar datos desde 'airbnb.csv' a un dataframe llamado 'airbnb'.
- Mostrar las primeras 6 filas del dataframe.
- Cambiar los nombres de las columnas según la tabla proporcionada.

## 1.Crea una nueva columna llamada MetrosCuadrados a partir de la columna PiesCuadrados.
- Crear una nueva columna 'MetrosCuadrados' a partir de 'PiesCuadrados' utilizando la conversión de unidades (0,92903sq ft = 1m)
```R
airbnb$MetrosCuadrados <- airbnb$PiesCuadrados * 0.92903
head(airbnb, 6)
```

## 2. Miremos el código postal. Es una variable con entradas erroneas. Hay valores como '','-' y '28' que deberían ser considerados como NA. Así mismo también debería ser NA todos los que no compiencen por 28, ya que estamos con códigos postales de Madrid

El código postal 28002, 28004 y 28051 tienen entradas repetidas. Por ejemplo las entradas 28002\n20882 deberían ir dnetro de 28002

El codigo 2804 debería ser 28004, 2805 deberia ser 28005 y 2815 juncto con 2815 debería ser 28015

Limpia los datos de la columna Codigo Postal
- Realizar operaciones para limpiar y corregir la columna 'CodigoPostal' según las especificaciones dadas.
- Eliminamos los códigos postales erroneos de o incompletos
```R
airbnb[which(airbnb$CodigoPostal == '28'),'CodigoPostal'] <- NA
airbnb[which(airbnb$CodigoPostal == '-'),'CodigoPostal'] <- NA
airbnb[which(airbnb$CodigoPostal == ''),'CodigoPostal'] <- NA
```
- Eliminar códigos postales que comienzan con '2' seguido de dígitos específicos salvo el 8
```R
airbnb[which(grepl("^2[0,1,2,3,4,5,6,7,9]",airbnb$CodigoPostal)),'CodigoPostal']<-NA
```
- Corregir códigos postales específicos:
```R
airbnb$CodigoPostal<-gsub("Madrid 28004","28004",airbnb$CodigoPostal)
airbnb$CodigoPostal<-gsub("^2815$","28015",airbnb$CodigoPostal)
airbnb$CodigoPostal<-gsub("^2804$","28004",airbnb$CodigoPostal)
airbnb$CodigoPostal<-gsub("^2805$","28005",airbnb$CodigoPostal)
airbnb$CodigoPostal<-gsub("28051\n28051","28051",airbnb$CodigoPostal)
airbnb$CodigoPostal<-gsub("28002\n28002","28002",airbnb$CodigoPostal)

```
Eliminamos los códigos postales erroneos de o incompletos
## 3. Una vez limpios los datos ¿Cuales son los códigos postales que tenemos?
- Mostrar los códigos postales únicos en el dataframe después de limpiar los datos.Utilizo la función unique y na.omit para mostrar los valores únicos en la columna CodigoPostal del dataframe airbnb después de eliminar los valores NA (valores faltantes).

 ```
[1] "28004"  "28015"  "28013"  "28005"  "28012"  "28014"  "28045"  "28007"  "28028"  "28009"  "28001" 
[12] "28006"  "28010"  "28002"  "28034"  "28050"  "28008"  "28011"  "28049"  "28038"  "28053"  "28047" 
[23] "28025"  "28019"  "28024"  "28016"  "28036"  "28046"  "28039"  "28020"  "28003"  "28029"  "28054" 
[34] "28041"  "28026"  "28058"  "28018"  "28030"  "28017"  "28027"  "28043"  "28033"  "28055"  "28021" 
[45] "28032"  "28037"  "28022"  "28042"  "28094"  "280013" "28035"  "28040"  "28031"  "28044"  "28105" 
[56] "28023"  "28051"  "28850"  "28048"  "28056"  "28060"  "28052" 
```
## 4.¿Cuales son los 5 códigos postales con más entradas? ¿Y con menos? ¿Cuantas entradas tienen?
- Encontrar y mostrar los 5 códigos postales con más y menos entradas, así como la cantidad de entradas.

    ```

   Resultado :

```  
28012 28004 28005 28013 28014 
 2060  1796  1195  1019   630 

280013  28048  28052  28056  28058 
     1      1      1      1      1 
```
## 5.¿Cuales son los barrios que hay en el código postal 28012?
- Encontrar los barrios dentro del código postal 28012.
```
[1] Sol             Acacias         Palos de Moguer Embajadores     Cortes          Palacio        
 [7] Universidad     Delicias        Arapiles        Atocha          Goya           
125 Levels: Abrantes Acacias Adelfas Aeropuerto Aguilas Alameda de Osuna Almagro Almenara ... Zofío
```
## 6. ¿Cuantas entradas hay en cada uno de esos barrios para el codigo postal 28012?
- Mostrar la cantidad de entradas para cada barrio dentro del código postal 28012.
```
   Description:df [11 × 2]
 
 
Var1
<fctr>
Freq
<int>
2	Acacias	13		
14	Arapiles	1		
18	Atocha	1		
41	Cortes	216		
45	Delicias	1		
49	Embajadores	1449		
56	Goya	1		
```
## 7.¿Cuantos barrios hay en todo el dataset airbnb? ¿Cuales son?
- Total barrios:
```
[1] 13207
 [1] Universidad                  Sol                          Imperial                    
  [4] Acacias                      Chopera                      Delicias                    
  [7] Palos de Moguer              Embajadores                  Cortes                      
 [10] Atocha                       Pacífico                     Adelfas                     
 [13] Estrella                     Ibiza                        Jerónimos                   
 [16] Niño Jesús                   Palacio                      Justicia                    
 [19] Recoletos                    Goya                         Fuente del Berro            
 [22] Arapiles                     Trafalgar                    Almagro                     
 [25] Guindalera                   Lista                        Castellana                  
 [28] El Viso                      Prosperidad                  Valverde                    
 [31] Casa de Campo                El Goloso                    Numancia                    
 [34] Cármenes                     Puerta del Angel             Lucero                      
 [37] Aluche                       San Isidro                   Campamento                  
 [40] Comillas                     Opañel                       Vista Alegre                
 [43] Ciudad Jardín                Hispanoamérica               Nueva España                
 [46] Castilla                     Bellas Vistas                Cuatro Caminos              
 [49] Castillejos                  Vallehermoso                 Almenara                    
 [52] Valdeacederas                Berruguete                   Gaztambide                  
 [55] Rios Rosas                   Peñagrande                   Argüelles                   
 [58] Puerta Bonita                Buenavista                   Abrantes                    
 [61] Orcasur                      San Fermín                   Almendrales                 
 [64] Pradolongo                   Portazgo                     Entrevías                   
 [67] San Diego                    Palomeras Bajas              Fontarrón                   
 [70] Vinateros                    Ventas                       Pueblo Nuevo                
 [73] Quintana                     Concepción                   San Juan Bautista           
 [76] Costillares                  Piovera                      Canillas                    
 [79] Pinar del Rey                Apostol Santiago             San Andrés                  
 [82] Valdefuentes                 Butarque                     Los Angeles                 
 [85] Casco Histórico de Vicálvaro Simancas                     Rejas                       
 [88] Salvador                     Casco Histórico de Barajas   Pilar                       
 [91] La Paz                       Mirasierra                   Ciudad Universitaria        
 [94] Moscardó                     Palomeras Sureste            Marroquina                  
 [97] Media Legua                  Los Rosales                  Casco Histórico de Vallecas 
[100] Timón                        Corralejos                   Cuatro Vientos              
[103] Colina                       San Cristobal                Alameda de Osuna            
[106] Aeropuerto                   Palomas                      Zofío                       
[109] Aguilas                      Legazpi                      Fuentelareina               
[112] Aravaca                      Ambroz                       Canillejas                  
[115] Valdezarza                   Amposta                      San Pascual                 
[118] Santa Eugenia                Arcos                        Rosas                       
[121] Valdemarín                   El Plantío                   Hellín                      
[124] Pavones                      Orcasitas                   
125 Levels: Abrantes Acacias Adelfas Aeropuerto Aguilas Alameda de Osuna Almagro Almenara ... Zofío
```

## 8.¿Cuales son los 5 barrios que tienen mayor número entradas?
- Identificar los tipos de alquiler únicos, mostrar sus frecuencias y crear un diagrama de cajas para visualizar la distribución de precios por tipo de alquiler.
 ``` 
Embajadores Universidad     Palacio         Sol    Justicia 
       1844        1358        1083         940         785 
```
## 9.¿Cuantos Tipos de Alquiler diferentes hay? ¿Cuales son? ¿Cuantas entradas en el dataframe hay por cada tipo?
- 3 tipos de Alquiler hay
```
  [1] Private room    Entire home/apt Shared room    
Levels: Entire home/apt Private room Shared room


Entire home/apt       Private room     Shared room 
           7903            5113             191 
```


## 10.Cual es el precio medio de alquiler de cada uno, la diferencia que hay ¿es estadísticamente significativa? ¿Con que test lo comprobarías?
- Filtrar el dataframe para incluir solo apartamentos enteros en el barrio Sol.

```
TipoAlquiler
<fctr>
Precio
<dbl>
Entire home/apt	87.29661			
Private room	34.25514			
Shared room	29.85340			
3 rows


	Shapiro-Wilk normality test

data:  entireHome$Precio[1:5000]
W = 0.64959, p-value < 2.2e-16

                Df   Sum Sq Mean Sq F value Pr(>F)    
TipoAlquiler     2  8981217 4490608    1828 <2e-16 ***
Residuals    13195 32417217    2457                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
9 observations deleted due to missingness

```

## 11. Filtra el dataframe cuyos tipo de alquiler sea 'Entire home/apt' y guardalo en un dataframe llamado *airbnb_entire*. Estas serán las entradas que tienen un alquiler del piso completo.
- Encontrar y mostrar los 5 barrios con más apartamentos enteros en alquiler.
```
  [1] "Embajadores" "Universidad" "Palacio"     "Sol"         "Cortes"     
```
## 12.¿Cuales son los 5 barrios que tienen un mayor número de apartamentos enteros en alquiler? Nota: Mirar solo en airbnb_entire. A partir de este punto y hasta que se diga lo contrario partiremos de airbnb_entire.
- Calcular el precio medio y el número de entradas para los 5 barrios con el mayor precio.
```
  [1] "Embajadores" "Universidad" "Palacio"     "Sol"         "Cortes"     
```

## 13.Barrios con mayor precio y más de 100 entradas
- Filtrar y mostrar los 5 barrios con mayor precio y más de 100 entradas.


 
 ```
Barrio
<fctr>
PrecioMedio
<dbl>
77	Palomas	309.7500		
50	Fuentelareina	180.0000		
93	Recoletos	161.9254		
43	El Plantío	150.0000		
30	Castellana	141.3889
```
## 14.¿Cuantos apartamentos hay en cada uno de esos barrios?
- Crear diagramas de densidad de precios para cada uno de los 5 barrios.

## 15.Tamaño medio de apartamentos por barrio
- Calcular el tamaño medio de los apartamentos en metros cuadrados para los 5 barrios con mayor precio.

## 16.Diagramas de densidad por tamaño de apartamento
- Crear diagramas de densidad de tamaños de apartamentos para cada uno de los 5 barrios.

## 17.Prueba de Kruskal-Wallis para tamaños de apartamento
- Realizar una prueba de Kruskal-Wallis para determinar si las distribuciones de tamaños de apartamentos son estadísticamente diferentes entre los 5 barrios.

## 18.Correlación entre variables
- Calcular la correlación entre diferentes variables.

## 19.Filtrar datos para mejorar correlación
- Filtrar los datos para mejorar la correlación entre el número de dormitorios y los metros cuadrados.

## 20.Modelo lineal para predecir el precio
- Calcular dos modelos lineales para predecir el precio utilizando el número de dormitorios y los metros cuadrados.

## 21.Incremento del precio por metro cuadrado extra
- Calcular el incremento del precio por cada metro cuadrado extra con su intervalo de confianza al 95%.

## 22.Incremento del precio por habitación
- Calcular el incremento del precio por cada habitación con su intervalo de confianza al 95%.

## 23.Probabilidad e intervalo de confianza para 3 dormitorios
- Calcular la probabilidad de encontrar un apartamento con 3 dormitorios en el barrio Sol y su intervalo de confianza.
