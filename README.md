# EyCD2022
Repositorio de respuestas de la materia: Exploración y curación de datos (2022), en el marco de la Diplomatura en Ciencia de Datos - Universidad Nacional de Córdoba.

# **DIPLOMATURA 2022**

# EXPLORACIÓN Y CURACIÓN DE DATOS

## GRUPO Nº24

## INTEGRANTES:
   - [x] Nico Rosales 
   - [x] Daniel Rubio
   - [x] Diana Fonnegra
   - [x] Clarisa Manzone

----   
En este repositorio se encuentran las entregas con resultados correspondientes a la asignatura de _Exploración y curación de datos_.

## **Requerimientos - Librerías necesarias**:
   - [x] matplotlib.pyplot
   - [x] pandas
   - [x] seaborn
   - [x] numpy
   - [x] missingno
   - [x] plotly
   - [x] sqlalchemy
   - [x] sklearn
----

# Documentación:
Para la limpieza de la base de datos realizamos los siguientes pasos:

![image](https://user-images.githubusercontent.com/11649711/174284658-a02ecfcc-df61-49cf-aaac-459826c12d45.png)

  ## Criterios de exclusión (Entregable 2 Parte1 Ejercicio 2)
  A partir de dos conjuntos de datos:
   - [x] df_melb -> Presenta los datos de los precios de las casas de Melbourne.
   - [x] df_airbnb -> Contiene los datos de los alquileres de airbnb en Melbourne.
  ----
   - [x] Procesos sobre df_melb: 
  1. Filtramos la cantidad de valores átipicos presentes en la variable precio, definimos los umbrales según los cuantiles 0.05 y 0.95*.
  2. Se construyó un contador que separa y totaliza la cantidad de habitaciones presentes en el conjunto.
  3. Filtramos la información teniendo en cuenta aquellos registros que solo tengan una habitación.
  4. Excluimos aquellos los registros en donde los baños y dormitorios superen el número de habitaciónes habitación.
   - [x] Procesos sobre df_airbnb: 
  5. Unificamos la la variable Zipcode dado que se encontraba con valores flotantes y enteros. Agrupamos los registros dada esta variable.
  6. Se tomaron del conjunto de datos df_airbnb las variables relevantes: price, weekly_price, monthly_price, zipcode y se formo: airbnb_price_by_zipcode con columnas adicionales (media y conteo)
  ----
   - [x] Unimos las bases de datos, usando la funcion merge a partir de zipcode.
   - [x] Consultamos el porcentaje de faltantes en la base de datos y eliminamos variables que aportaban información poco relevante y/o redundante.
   - [x] Almacenamos los datos resultantes en el repositorio: [Grupo242022/EyCD](https://github.com/Grupo242022/EyCD/blob/main/melb_data_extended.csv)
  ----

  ## Características seleccionadas
  En general tomamos las siguientes características:
  ![image](https://user-images.githubusercontent.com/11649711/174290630-f3463f34-645d-4f76-b188-5fdb5ccd8fd2.png)
 
  ### Características categóricas
 
  1. Type: tipo de propiedad. 3 valores posibles.
  2. Suburb: Suburbio en donde se encuentra la propiedad. 304 valores posibles.
  3. Date: Fecha. Clase datetime. 

   ### Características numéricas

  1. Rooms: cantidad de habitaciones
  2. Distance: distancia al centro de la ciudad.
  3. airbnb_price_median: se agrega la mediana al precio diario de 
     publicaciones de la plataforma AirBnB en el mismo código 
     postal.
  4. Bathroom: cantidad de cuartos de baño en el domicilio registrado.
  5. Postcode: código postal del registro.
  6. Lattitude: latitud.
  7. Longtitude: logitud.
  8. Car: número de estacionamientos.
  10. BuildingArea: área construida.
  11. Landsize: tamaño del lote.
  12. YearBuilt: año de construcción.
  13. Propertycount: Cantidad de domicilios que existen en el suburbio.
  14. Price: Precio en dólares.
       
  ### Transformaciones
  1. Utilizamos DictVetorizer para codificar las variables categóricas y lo convertimos en un DataFrame de filas:12138 y columnas: 378 (dataframe: df_melb_enc).
  2. Las columnas `YearBuilt` y `BuildingArea` fueron imputadas utilizando el algoritmo: `KNeighborsRegressor()`. 
  2.1. Se realizaron pruebas sin escalar (y solo usando para imputar las columnas:). Por ejemplo (`YearBuilt_modificado` vs .`YearBuilt_original`):    
     ![image](https://user-images.githubusercontent.com/11649711/174295448-b90044cd-9b44-4342-b2fb-4fc5e7c51acc.png)
  3. Todas las características numéricas fueron escaladas. Se analizarón dos escaladores: `RobustScaler()` y `MinMaxScaler()`.     
  3.1. Combinando `RobustScaler()` e imputación por `KNeighborsRegressor()` para las columnas - `YearBuilt` y `BuildingArea`. 
  En este ejemplo se muestra `YearBuilt`(modificado sin escalar, con escalador RobustScaler() y datos originales).
  ![image](https://user-images.githubusercontent.com/11649711/174295858-0722a177-fb95-4e51-872c-f58601eb7b90.png)
  3.2. Combinando `MinMaxScaler()` e imputación por `KNeighborsRegressor()` para las columnas - `YearBuilt` y `BuildingArea`. 
  En este ejemplo se muestra `YearBuilt`(modificado sin escalar, con escalador MinMaxScaler() y datos originales).  
  ![image](https://user-images.githubusercontent.com/11649711/174296287-23c786fd-e88a-4046-a0ab-86ba9afc728e.png)
  4. Aplicamos `PCA` para obtener $n$ componentes principales de la matriz. Construimos un conjunto de datos que solo contara con las variables numéricas continuas. A su vez, escalamos y estandarizamos a la matriz original (dataframe: pca_data).
  5. Obtuvimos el porcentaje de varianza explicada por cada componente (modelo_pca.explained_variance_ratio_).
  6. Al contar con el modelo_pca, usamos el método `transform()`, reducimos la dimensionalidad de las nuevas observaciones (dataframe: proyecciones). 
  7. Graficamos las componentes principales:
 ![image](https://user-images.githubusercontent.com/11649711/174299545-888b48f0-8042-46ea-aed6-5afa70563262.png)
 
  ### Composición del resultado y Datos aumentados
  
    1. A partir dela base de datos original, segmentamos el conjunto en dos:
    
   - [x] melb_df_num -> Data frame con variables númericas
   - [x] melb_df_cat -> Data frame con variables categóricas
  
    2. Aplicamos el Encode usando el método de `OneHotEncoder()` sobre la matriz con datos categóricos.
    3. Incorporamos a la matriz con datos numéricos.
    4. Añadimos las variables imputadas en el punto 2 ('buildingArea_imp', 'yearBuilt_imp', data frame: inver_imputa).
    5. Incorporamos las 6 primeras columnas del PCA (data frame: proyecciones).
    6. Reconstruimos el resultado obteniendo el DataFrame: processed_melb_df.

