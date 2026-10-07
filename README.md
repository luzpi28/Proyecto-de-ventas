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
<img width="240" height="111" alt="image" src="https://github.com/user-attachments/assets/7b48c8fa-8c6f-4170-a19c-c7391441ab1c" />
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
<img width="210" height="70" alt="image" src="https://github.com/user-attachments/assets/4dec0d57-9848-4964-8099-22c246fd5584" />
<br><br>

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
<img width="270" height="80" alt="image" src="https://github.com/user-attachments/assets/61acb680-ccfe-4965-b30a-037a40ba7183" />
<br>
<br>

**Pregunta #6.-¿Cuántos productos distintos de lácteos se han vendido en los supermercados?**
```sql
SELECT COUNT (DISTINCT producto) AS Productos_únicos
FROM dbo.dataset_ventas_lacteos_2024
;
```
<img width="170" height="74" alt="image" src="https://github.com/user-attachments/assets/f223f4c9-7854-4e4a-b0e3-31e2e8b043cc" />
<br><br>

**Pregunta #7.- Productos lácteos más vendidos.**
```sql
SELECT TOP 3
	   Producto, 
       SUM(Cantidad_comprada) AS Cantidad_vendida
FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Producto
ORDER BY 2 DESC
;
```
<img width="420" height="98" alt="image" src="https://github.com/user-attachments/assets/8f4cd2fa-e31b-476f-a556-de7c436a7012" />
<br><br>


**Pregunta #8.- ¿Qué estados tienen mayores ventas?**
```sql
SELECT	TOP 3
        Estado,
		ROUND(SUM([Valor_total(USD)]),2) AS Venta_Total
		FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Estado
ORDER BY 2 DESC
;
```
<img width="318" height="100" alt="image" src="https://github.com/user-attachments/assets/bc418d79-fe34-485b-af24-5a56b75a5458" />
<br><br>

**Pregunta #9.- ¿Qué supermercado concentra el mayor valor total de ventas?**
```sql
SELECT	TOP 3
        Nombre_del_supermercado,
		ROUND(SUM([Valor_total(USD)]),2) AS Venta_Total
		FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Nombre_Del_supermercado
ORDER BY Venta_Total DESC
;
```
<img width="378" height="99" alt="image" src="https://github.com/user-attachments/assets/8a4711d4-bd6a-4f86-b17d-6e24b518a954" />
<br><br>

**Pregunta #10.- ¿Quiénes son los 3 vendedores con mayores ventas generadas?**
```sql
SELECT TOP 3
       Nombre_del_vendedor,
	   ROUND(SUM([Valor_total(USD)]),2) AS Venta_total
	   FROM dbo.dataset_ventas_lacteos_2024
GROUP BY Nombre_del_vendedor
ORDER BY Venta_total DESC
;
```
<img width="375" height="103" alt="image" src="https://github.com/user-attachments/assets/79f2e1fe-a502-40a5-8562-9f1c05d57785" />
<br><br>

**Pregunta #11.-¿Cuáles son las formas de pago disponibles y cuántas operaciones se registraron con cada una?**
```sql
SELECT  Forma_de_pago,
		COUNT(Forma_de_pago) AS cantidad_pago
FROM dbo.dataset_ventas_lacteos_2024
Group by Forma_de_pago
ORDER BY cantidad_pago DESC
;
```
<img width="378" height="166" alt="image" src="https://github.com/user-attachments/assets/71645615-a314-4a09-816b-7cb3fc610833" />
<br><br>



<br>**5. PowerBi-Dashboard**
<br><br>
<img width="1000" height="550" alt="Ventas-lácteos-2024" src="https://github.com/user-attachments/assets/bf4999d6-ae29-4c72-a89a-c7b7cdf1bc3e" />


