# Proyecto-de-ventas
**Proyecto de Ventas de Lácteos 2024.**

Herramientas utilizadas: Python, SQL y Power Bi

**Estructura del Proyecto**<br>
1. [Dataset](#Estructura-del-Proyecto)<br>
2. [Tareas](#Estructura-del-Proyecto)<br>
3. [Python-Limpieza de Datos](#Estructura-del-Proyecto)<br>
4. [SQL-Consultas de negocio](#Estructura-del-Proyecto)<br>
5. [PowerBi-Dashboard](#Estructura-del-Proyecto)<br>


**1. Dataset**

Este dataset simula un historial de 10,000 ventas de productos lácteos realizadas a lo largo del año 2024, cumpliendo requisitos de trazabilidad comercial en supermercados de cinco estados de Estados Unidos.

Está diseñado para análisis comerciales, de comportamiento de compra y de desempeño de productos/líneas de negocio.

Podemos encontrar el datasets en el siguiente enlace: https://www.kaggle.com/datasets/hectorconde/dataset-ventas-lacteos-2024

<br>**2. Tareas**
<br>En este análisis comercial, ayudo a responder lo siguiente:
<br>
<br>1.-¿Qué meses registraron el mejor desempeño comercial durante el año 2024?
<br>2.-¿A cuánto ascendieron las ventas totales registradas durante el mes de diciembre del 2024?
<br>3.-¿Qué categoría genera más ingresos?
<br>4.-¿Qué porcentaje de las ventas totales representa cada categoría?
<br>5.-¿Cuál es la categoría más vendida en el estado de Massachusetts?
<br>6.-¿Cuántos productos distintos de lácteos se han vendido en los supermercados?
<br>7.-Productos lácteos más vendidos.
<br>8.-¿Qué estados tienen mayores ventas?
<br>9.-¿Qué supermercado concentra el mayor valor total de ventas?
<br>10.-¿Quiénes son los 3 vendedores con mayores ventas generadas?
<br>11.-¿Cuáles son las formas de pago disponibles y cuántas operaciones se registraron con cada una?

<br>**3. Python-Limpieza de Datos**
<br>Primero debemos asegurarnos de que el dataset tenga el formato correcto y estén listos para ser procesados.

<img width="1248" height="436" alt="Captura-proy-01" src="https://github.com/user-attachments/assets/932a75b5-1d49-477d-ba2c-d6052b6b363a" />


<br>**4. SQL-Consultas de negocio**
<br>
<br>
**Pregunta #1: ¿Qué meses registraron el mejor desempeño comercial durante el 2024?**
<br>Encontré los meses que registraron mejor desempeño utilizando las funciones ROUND,SUM y GROUP BY.
```sql
SELECT TOP 5
	   DATENAME(month, Fecha) AS Mes,
	   ROUND(SUM([Valor total (USD)]),2) AS Valor_total
FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Month(Fecha),DATENAME(month, Fecha)
ORDER BY Valor_total DESC
;
```
<img width="250" height="229" alt="image" src="https://github.com/user-attachments/assets/5248fdf8-002a-4f10-94db-724fa509a944" />
<br>


**Pregunta #2.-¿A cuánto ascendieron las ventas totales registradas durante el mes de diciembre del 2024?**
```sql
SELECT DATENAME(month, Fecha) AS Mes,
	   ROUND(SUM([Valor total (USD)]),2) AS Valor_total
FROM dbo.dataset_ventas_lacteos_2024
WHERE MONTH(Fecha) = 12
GROUP BY Month(Fecha),DATENAME(month, Fecha)
;
```
<img width="250" height="111" alt="image" src="https://github.com/user-attachments/assets/7b48c8fa-8c6f-4170-a19c-c7391441ab1c" />
<br>

**Pregunta #3.-¿Qué categoría genera más ingresos?**
```sql
SELECT TOP 1
       Categoria,
	   ROUND(SUM([Valor total (USD)]),2) AS Valor_total
FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Categoria
ORDER BY Valor_total DESC
;
```
<img width="230" height="70" alt="image" src="https://github.com/user-attachments/assets/4dec0d57-9848-4964-8099-22c246fd5584" />
<br>


**Pregunta #4.-¿Qué porcentaje de las ventas totales representa cada categoría?**
```sql
SELECT 
    Categoria,
    ROUND(SUM([Valor total (USD)]),2) AS Venta_Total,
    ROUND(
        (SUM([Valor total (USD)]) * 100.0) / SUM(SUM([Valor total (USD)])) OVER(), 2
    ) AS Porcentaje_Ventas
FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Categoria
ORDER BY Porcentaje_Ventas DESC
;
```
<img width="280" height="211" alt="image" src="https://github.com/user-attachments/assets/5463e1fc-e8d3-4de1-9eed-dda61fca4570" />
<br>
<br>

**Pregunta #5.-¿Cuál es la categoría más vendida en el estado de Massachusetts?**
```sql
SELECT TOP 1
       Estado,
       Categoria,
       SUM(TRY_CAST([Cantidad_comprada] AS INT)) AS Total_Vendidos
FROM dbo.dataset_ventas_lacteos_2024
WHERE Estado = 'Massachusetts'
GROUP BY Estado, Categoria
ORDER BY SUM(TRY_CAST([Cantidad_comprada] AS INT)) DESC
;
```
<img width="280" height="84" alt="image" src="https://github.com/user-attachments/assets/61acb680-ccfe-4965-b30a-037a40ba7183" />
<br>
<br>

**Pregunta #6.-¿Cuántos productos distintos de lácteos se han vendido en los supermercados?**
```sql
SELECT COUNT (DISTINCT producto) AS Productos_únicos
FROM dbo.dataset_ventas_lacteos_2024
;
```
<img width="180" height="77" alt="image" src="https://github.com/user-attachments/assets/f223f4c9-7854-4e4a-b0e3-31e2e8b043cc" />
<br>


**Pregunta #7.-**





<br>**5. PowerBi-Dashboard**
<br><br>
<img width="1100" height="601" alt="Ventas-lácteos-2024" src="https://github.com/user-attachments/assets/bf4999d6-ae29-4c72-a89a-c7b7cdf1bc3e" />


