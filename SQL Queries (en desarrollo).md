## Exploración de la Base de Datos

**Objetos de la base de datos**
```sql
SELECT * FROM INFORMATION_SCHEMA.TABLES
```

**Exploración de columnas de cada tabla**
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_customers_dataset'
```
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_order_items_dataset'
```
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_order_payments_dataset'
```
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_order_reviews_dataset'
```
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_orders_dataset'
```
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_products_dataset'
```
```sql
SELECT * FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_sellers_dataset'
```

---

## 1. Exploración de Dimensiones

**1. ¿Qué estados (`customer_state`) distintos existen en la tabla de clientes?**
```sql
SELECT DISTINCT customer_state
FROM dbo.olist_customers_dataset
```

**2. ¿Qué estados de pedido (`order_status`) distintos existen en la tabla de pedidos?**
```sql
SELECT DISTINCT order_status
FROM dbo.olist_orders_dataset
```

**3. ¿Qué categorías de producto distintas existen según la tabla de traducción `product_category_name_translation`?**
```sql
SELECT DISTINCT column2
FROM dbo.product_category_name_translation
```

**4. ¿Qué estados (`seller_state`) distintos existen en la tabla de vendedores?**
```sql
SELECT DISTINCT seller_state
FROM dbo.olist_sellers_dataset
```

**5. ¿Qué puntuaciones (`review_score`) distintas pueden otorgar los clientes en sus reseñas?**
```sql
SELECT DISTINCT review_score
FROM dbo.olist_order_reviews_dataset
```

**6. ¿Qué métodos de pago (`payment_type`) distintos se utilizan en la plataforma?**
```sql
SELECT DISTINCT payment_type
FROM dbo.olist_order_payments_dataset
```

---

## 2. Exploración de Fechas

**1. ¿Cuál es la fecha del primer y del último pedido registrado (`order_purchase_timestamp`), y cuántos años de actividad abarca el conjunto de datos?**
```sql
SELECT
	MIN(order_purchase_timestamp) as Primera_Compra,
	MAX(order_purchase_timestamp) as Última_Compra,
	DATEDIFF(year, MIN(order_purchase_timestamp), MAX(order_purchase_timestamp)) as Rango_Temporal
FROM dbo.olist_orders_dataset
```

**2. ¿Cuál es la fecha de la primera y la última reseña registrada (`review_creation_date`), y cuántos años de amplitud tiene este rango?**
```sql
SELECT
	MIN(review_creation_date) as Primera_Review,
	MAX(review_creation_date) as Última_Review,
	DATEDIFF(year, MIN(review_creation_date), MAX(review_creation_date)) as Rango_Temporal
FROM dbo.olist_order_reviews_dataset
```

---

## 3. Exploración de Medidas

**1. ¿Cuántas órdenes distintas (`order_id`) se han registrado en total?**
```sql
SELECT
	COUNT(DISTINCT order_id) as Número_Órdenes
FROM dbo.olist_orders_dataset
```

**2. ¿Cuántos clientes distintos (`customer_id`) han realizado pedidos?**
```sql
SELECT
	COUNT(DISTINCT customer_id) as Número_Clientes
FROM dbo.olist_orders_dataset
```

**3. ¿Cuántos vendedores distintos (`seller_id`) están registrados en la plataforma?**
```sql
SELECT
	COUNT(DISTINCT seller_id) as Número_Vendedores
FROM dbo.olist_sellers_dataset
```

**4. ¿Cuántas reseñas distintas (`review_id`) se han recibido?**
```sql
SELECT
	COUNT(DISTINCT review_id) as Número_Reviews
FROM dbo.olist_order_reviews_dataset
```

**5. ¿Cuántos productos distintos (`product_id`) componen el catálogo?**
```sql
SELECT
	COUNT(DISTINCT product_id) as Número_Productos
FROM dbo.olist_products_dataset
```

**6. ¿Cuántas categorías de producto distintas (`product_category_name`) existen?**
```sql
SELECT
	COUNT(DISTINCT product_category_name) as Número_Categorías
FROM dbo.olist_products_dataset
```

**7. ¿Cuál es el beneficio total (`profit`) generado por la plataforma, calculado como la diferencia entre el precio de venta y el coste de flete?**
```sql
SELECT
	SUM(profit) as Beneficio_Total
FROM dbo.olist_order_items_dataset
```

**8. ¿Cuál es el precio medio (`price`) de los productos vendidos?**
```sql
SELECT
	ROUND(AVG(price),2) as Precio_Medio
FROM dbo.olist_order_items_dataset
```

**9. ¿Cuál es el coste medio de flete (`freight_value`) derivado de las ventas?**
```sql
SELECT
	ROUND(AVG(freight_value),2) as Costes_Medio_Flete
FROM dbo.olist_order_items_dataset
```

**10. ¿Cómo se resumen todas estas medidas en un único informe exploratorio?**
```sql
SELECT
	'Número de órdenes' as Medida,
	COUNT(DISTINCT order_id) as Valor
FROM dbo.olist_orders_dataset
UNION ALL
SELECT
	'Número de clientes' as Medida,
	COUNT(DISTINCT customer_id) as Valor
FROM dbo.olist_orders_dataset
UNION ALL
SELECT
	'Número de vendedores' as Medida,
	COUNT(DISTINCT seller_id) as Valor
FROM dbo.olist_sellers_dataset
UNION ALL
SELECT
	'Número de reviews' as Medida,
	COUNT(DISTINCT review_id) as Valor
FROM dbo.olist_order_reviews_dataset
UNION ALL
SELECT
	'Número de productos' as Medida,
	COUNT(DISTINCT product_id) as Valor
FROM dbo.olist_products_dataset
UNION ALL
SELECT
	'Número de categorías' as Medida,
	COUNT(DISTINCT product_category_name) as Valor
FROM dbo.olist_products_dataset
UNION ALL
SELECT
	'Beneficio Total' as Medida,
	SUM(profit) as Valor
FROM dbo.olist_order_items_dataset
UNION ALL
SELECT
	'Precio medio' as Medida,
	ROUND(AVG(price),2) as Valor
FROM dbo.olist_order_items_dataset
UNION ALL
SELECT
	'Coste de flete medio' as Medida,
	ROUND(AVG(freight_value),2) as Valor
FROM dbo.olist_order_items_dataset
```

---

## 4. Análisis de Magnitudes

*(en desarrollo)*

## 5. Análisis de Ranking (Top N)

*(en desarrollo)*
