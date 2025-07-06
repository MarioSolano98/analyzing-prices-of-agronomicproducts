# Tablero de Seguimientos de Precios de Productos Agronomicos

## Enunciado

Este tablero muestra los precios de productos agronomicos en los más grandes mercados de El Salvador. Los datos provienen desde la pagina del ministerio de agricultura y que he scrapeado para descargar, procesar y consolidad los datos. En este tablero se pueden ver los cambios de los productos a traves de los años organizados en cuatro categorias principales (Futas, Granos, Vegetales y Agroindustriales) desde diferentestes zonas en el pais. 

### Pasos Seguidos 

- Paso 1 : Cargar los datos en PowerBI Desktop, los datos están en formato csv.
- Paso 2 : Filtrar los productos que no tienen un correlativo indicativo.
- Paso 3 : Enriquecer la información añadiendo una tabla con las unidadesdes de diferentes productos.
- Paso 4 : Enriqueces la información añadiendo una tabla con las ubicaciones de los mercados de El Salvador.
- Paso 5 : Añadir una tabla calendario.
- Paso 6 : Añadir una tabla dedicada de medidas.

Las medidas crearan fueron: 
       
        Precio Promedio = 
        /*
        Average Prices
        */
        AVERAGE('Reporte_Consolidado Precios de Productos'[Promedio Mayoristas])

        Precio Promedio Mes Anterior = 
        /*
        Average Price of the previus Month usint time intelligence
        */
        CALCULATE([Precio Promedio],PARALLELPERIOD('Tabla Calendario'[Fecha],-1,MONTH))
        
        
        
        Precio Promedio y Unidad de Medida = 
        /*
        Average Price and Measure Unit, display the avg price measure and the unit of measure as a string
        */
        CONCATENATE([Precio Promedio],FIRSTNONBLANK('Unidades de Productos'[Unidad])) 
        
        Primer Precio Promedio = 
              /*
              First Montly Average Price, this measure shows the average price for the first month of the context
              If we select a year as a filter this will show the average price of january
              */
              CALCULATE([Precio Promedio],
                  FILTER(
                      'Reporte_Consolidado Precios de Productos',
                      AND(
                      MONTH('Reporte_Consolidado Precios de Productos'[Fecha])=
                      MONTH(MIN('Reporte_Consolidado Precios de Productos'[Fecha])),
                      YEAR('Reporte_Consolidado Precios de Productos'[Fecha])=
                      YEAR(MIN('Reporte_Consolidado Precios de Productos'[Fecha]))
                      )
                      ))
        Ultimo Precio Promedio =
              /*
              Last Montly Average Price, this measure shows the average price for the last month of the context
              If we select a year as a filter this will show the average price of december
              */
              CALCULATE([Precio Promedio],
                  FILTER(
                      'Reporte_Consolidado Precios de Productos',
                      AND(
                      MONTH('Reporte_Consolidado Precios de Productos'[Fecha])=
                      MONTH(MAX('Reporte_Consolidado Precios de Productos'[Fecha])),
                      YEAR('Reporte_Consolidado Precios de Productos'[Fecha])=
                      YEAR(MAX('Reporte_Consolidado Precios de Productos'[Fecha]))
                      )
                     ))
       Cambios en Precios =
               /*
              Actual Montly Average Price Change, this measure shows the change bewtween the actual price and the price at the begining of the selected preiod
              */
               [Precio Promedio]-[Precio Promedio Mes Anterior]
               
       Cambios en Precios Periodo Selecionado =
              /*
              Changes in prices in the selected period
              If a year is selected the result will be the average price in december vs the average price in january
              */
              [Ultimo Precio Promedio]-[Primer Precio Promedio]
       
        
        Cambios en Precios Periodo Selecionado % =
               /*
              Porcentual changes in prices in the selected period
              */
               DIVIDE([Cambios en Precios Periodo Selecionado],[Primer Precio Promedio],0)

 
 # Capturas de Pantalla del Tablero (Power BI DESKTOP)
![general-categories](https://github.com/MarioSolano98/analyzing-prices-of-agronomicproducts/blob/main/analizing-agriculturalproductos-prices-dashboard2.png)

![prodcuts](https://github.com/MarioSolano98/analyzing-prices-of-agronomicproducts/blob/main/analizing-agriculturalproductos-prices-dashboard1.png)

# Visualizaciones Creadas

1 Crecimiento Porcentual por Categoria
![prodcuts](https://github.com/MarioSolano98/analyzing-prices-of-agronomicproducts/blob/main/Percentual%20Changes%20Line%20Chart.png)

2 Tarjetas por Categoria
![prodcuts](https://github.com/MarioSolano98/analyzing-prices-of-agronomicproducts/blob/main/Category%20KPI%20card.png)

3. Mercados y Zonas
![prodcuts](https://github.com/MarioSolano98/analyzing-prices-of-agronomicproducts/blob/main/Markets%20and%20Zones%20Map.png)

4. Productos y categorias
![prodcuts](https://github.com/MarioSolano98/analyzing-prices-of-agronomicproducts/blob/main/Product%20within%20Subcategory%20Line%20Chart.png)

