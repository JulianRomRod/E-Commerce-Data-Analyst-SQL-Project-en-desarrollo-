<img width="300" height="150" alt="image" src="https://github.com/user-attachments/assets/cca4af75-4d2f-4368-b03f-d969b518a1e8" />

# E-Commerce Data Analyst SQL Project (en desarrollo)

## Acerca del proyecto
Este proyecto analiza los datos de ventas de **Olist**, una plataforma brasileña de e-commerce que actúa como intermediaria entre pequeños y medianos vendedores (*sellers*) y los principales marketplaces de Brasil, permitiéndoles vender sus productos y gestionar sus pedidos desde un único sistema. El objetivo es analizar el comportamiento de compra de los clientes, el desempeño de los vendedores y las categorías de producto, la evolución de las ventas en el tiempo, la experiencia de entrega y la satisfacción del cliente a través de las reseñas. El propósito final es aportar información que permita optimizar la operativa comercial y logística de la plataforma.

## Objetivos del Proyecto
El objetivo principal es extraer conocimiento a partir de los datos reales de pedidos de Olist, explorando los distintos factores (clientes, productos, vendedores, pagos, reseñas, tiempo y geografía) que influyen en el rendimiento de la plataforma.

## Fuente de los Datos
Los datos utilizados forman parte del dataset público **Brazilian E-Commerce Public Dataset by Olist**, disponible en Kaggle:
🔗 https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

El dataset original contiene 9 tablas relacionadas entre sí a través de identificadores únicos (`order_id`, `customer_id`, `product_id`, `seller_id`). Para este proyecto se han utilizado **8 de las 9 tablas**, descartando únicamente la tabla `olist_geolocation_dataset` (información de coordenadas geográficas a nivel de código postal), por no ser necesaria para el alcance de este análisis. La tabla `product_category_name_translation`, que traduce el nombre de las categorías de producto del portugués al inglés, sí se incluye como tabla auxiliar de apoyo a la exploración de dimensiones.

## Acerca de los Datos
Los datos corresponden a un modelo relacional compuesto por **7 tablas**, con información real y anonimizada de pedidos realizados en Olist entre **septiembre de 2016 y octubre de 2018**, abarcando **99.441 pedidos**, **99.441 registros de clientes**, **32.951 productos** y **3.095 vendedores**.

### `olist_orders_dataset` (Pedidos)
Tabla central del modelo, contiene 8 columnas y 99.441 registros, cada uno correspondiente a un pedido:

| Columna                        | Descripción                                                        | Tipo de dato |
|---------------------------------|---------------------------------------------------------------------|--------------|
| order_id                        | Identificador único del pedido (clave primaria)                     | VARCHAR(32)  |
| customer_id                     | Identificador del cliente que realizó el pedido (relaciona con customers) | VARCHAR(32) |
| order_status                    | Estado del pedido (delivered, shipped, canceled, etc.)               | VARCHAR(20)  |
| order_purchase_timestamp        | Fecha y hora en la que se realizó el pedido                          | DATETIME     |
| order_approved_at               | Fecha y hora en la que se aprobó el pago del pedido                  | DATETIME     |
| order_delivered_carrier_date    | Fecha en la que el pedido fue entregado al transportista              | DATETIME     |
| order_delivered_customer_date   | Fecha en la que el pedido fue entregado al cliente                    | DATETIME     |
| order_estimated_delivery_date   | Fecha estimada de entrega comunicada al cliente                       | DATETIME     |

### `olist_customers_dataset` (Clientes)
Contiene 5 columnas y 99.441 registros:

| Columna                   | Descripción                                                     | Tipo de dato |
|----------------------------|-------------------------------------------------------------------|--------------|
| customer_id                | Identificador del cliente asociado a un pedido concreto (clave primaria) | VARCHAR(32) |
| customer_unique_id         | Identificador único real del cliente (permite detectar clientes recurrentes) | VARCHAR(32) |
| customer_zip_code_prefix   | Prefijo del código postal del cliente                              | VARCHAR(10)  |
| customer_city              | Ciudad de residencia del cliente                                   | VARCHAR(50)  |
| customer_state             | Estado/provincia de residencia del cliente (27 estados)             | VARCHAR(2)   |

### `olist_order_items_dataset` (Líneas de Pedido)
Contiene 7 columnas y 112.650 registros, cada uno correspondiente a un producto dentro de un pedido:

| Columna              | Descripción                                                        | Tipo de dato   |
|-----------------------|-----------------------------------------------------------------------|----------------|
| order_id              | Identificador del pedido (relaciona con orders)                       | VARCHAR(32)    |
| order_item_id         | Número secuencial del producto dentro del pedido                       | INT            |
| product_id            | Identificador del producto vendido (relaciona con products)            | VARCHAR(32)    |
| seller_id             | Identificador del vendedor que gestionó la línea (relaciona con sellers) | VARCHAR(32)  |
| shipping_limit_date   | Fecha límite para que el vendedor entregue el producto al transportista | DATETIME     |
| price                 | Precio unitario del producto                                            | DECIMAL(10,2)  |
| freight_value         | Coste de envío asociado a la línea de pedido                            | DECIMAL(10,2)  |

### `olist_products_dataset` (Productos)
Contiene 9 columnas y 32.951 registros:

| Columna                      | Descripción                                                  | Tipo de dato |
|-------------------------------|-----------------------------------------------------------------|--------------|
| product_id                    | Identificador único del producto (clave primaria)                | VARCHAR(32)  |
| product_category_name         | Categoría del producto en portugués (73 categorías distintas)    | VARCHAR(50)  |
| product_name_lenght           | Longitud del nombre del producto (nº de caracteres)               | INT          |
| product_description_lenght    | Longitud de la descripción del producto (nº de caracteres)        | INT          |
| product_photos_qty            | Cantidad de fotos publicadas del producto                          | INT          |
| product_weight_g              | Peso del producto en gramos                                       | INT          |
| product_length_cm             | Largo del producto en centímetros                                  | INT          |
| product_height_cm             | Alto del producto en centímetros                                   | INT          |
| product_width_cm              | Ancho del producto en centímetros                                  | INT          |

### `olist_sellers_dataset` (Vendedores)
Contiene 4 columnas y 3.095 registros:

| Columna                 | Descripción                                       | Tipo de dato |
|--------------------------|------------------------------------------------------|--------------|
| seller_id                | Identificador único del vendedor (clave primaria)     | VARCHAR(32)  |
| seller_zip_code_prefix   | Prefijo del código postal del vendedor                | VARCHAR(10)  |
| seller_city              | Ciudad donde opera el vendedor                        | VARCHAR(50)  |
| seller_state             | Estado/provincia donde opera el vendedor (23 estados) | VARCHAR(2)   |

### `olist_order_payments_dataset` (Pagos)
Contiene 5 columnas y 103.886 registros, ya que un mismo pedido puede tener varios pagos asociados:

| Columna                | Descripción                                                       | Tipo de dato   |
|--------------------------|-----------------------------------------------------------------------|----------------|
| order_id                 | Identificador del pedido (relaciona con orders)                       | VARCHAR(32)    |
| payment_sequential       | Número secuencial del pago dentro del pedido                           | INT            |
| payment_type             | Método de pago utilizado (credit_card, boleto, voucher, debit_card)    | VARCHAR(20)    |
| payment_installments     | Número de cuotas en las que se dividió el pago                          | INT            |
| payment_value            | Importe pagado                                                          | DECIMAL(10,2)  |

### `olist_order_reviews_dataset` (Reseñas)
Contiene 7 columnas y 99.224 registros:

| Columna                    | Descripción                                                        | Tipo de dato |
|------------------------------|-----------------------------------------------------------------------|--------------|
| review_id                    | Identificador único de la reseña                                       | VARCHAR(32)  |
| order_id                     | Identificador del pedido reseñado (relaciona con orders)                | VARCHAR(32)  |
| review_score                 | Puntuación otorgada por el cliente (escala de 1 a 5)                    | INT          |
| review_comment_title         | Título del comentario de la reseña (opcional)                           | VARCHAR(100) |
| review_comment_message       | Mensaje del comentario de la reseña (opcional)                          | TEXT         |
| review_creation_date         | Fecha en la que se envió la encuesta de satisfacción al cliente          | DATETIME     |
| review_answer_timestamp      | Fecha y hora en la que el cliente respondió la reseña                    | DATETIME     |

## Modelo de Datos
A diferencia de un esquema estrella clásico, este dataset presenta un modelo relacional con múltiples tablas conectadas entre sí:

- `olist_orders_dataset` es la tabla central y se relaciona con `olist_customers_dataset` a través de `customer_id`.
- `olist_order_items_dataset` relaciona los pedidos (`order_id`) con los productos (`product_id`) y los vendedores (`seller_id`).
- `olist_order_payments_dataset` y `olist_order_reviews_dataset` se relacionan con `olist_orders_dataset` a través de `order_id`.

- `product_category_name_translation` se relaciona con `olist_products_dataset` a través de `product_category_name` y se utiliza como tabla de apoyo para explorar las categorías de producto en inglés.

<img width="2486" height="1496" alt="dataset" src="https://github.com/user-attachments/assets/813cbd9a-ae1f-40d2-885d-724d238bbbbd" />

*Nota: se ha excluido del análisis la tabla `olist_geolocation_dataset`, que contiene coordenadas de latitud y longitud por código postal, al no ser necesaria para el alcance de las preguntas de negocio planteadas.*

## Lista de Análisis

1. **Análisis de Pedidos y Logística**

> Estudiar el estado de los pedidos, los tiempos de aprobación, envío y entrega, y compararlos con la fecha estimada de entrega para evaluar el desempeño logístico de la plataforma.

2. **Análisis de Productos y Vendedores**

> Analizar las categorías de producto con mejor y peor desempeño, así como identificar a los vendedores con mayor volumen de ventas y mejor valoración.

3. **Análisis de Clientes y Geografía**

> Identificar la distribución geográfica de los clientes (estado, ciudad), detectar clientes recurrentes y entender los patrones de compra por región.

4. **Análisis de Pagos**

> Estudiar los métodos de pago más utilizados, el número de cuotas empleadas y el importe medio pagado por pedido.

5. **Análisis de Satisfacción del Cliente**

> Analizar la distribución de las puntuaciones de las reseñas y su relación con los tiempos de entrega, para identificar factores que impactan en la satisfacción del cliente.

## Enfoque Utilizado de Análisis Exploratorio de Datos (EDA)

**1. Preparación y entendimiento de la base de datos**

En esta fase inicial se examina la estructura completa de la base de datos, tabla por tabla, para conocer qué información hay disponible antes de comenzar cualquier análisis.
- Exploración de la base de datos para identificar el conjunto completo de tablas cargadas.
- Exploración columna por columna de cada una de las siete tablas principales revisando nombres, orden y tipos de datos.
- Construcción del modelo relacional a partir de las tablas identificadas y detección de valores nulos, especialmente en `product_category_name` (610 nulos), en las fechas de entrega de `orders` (hasta 2.965 nulos en `order_delivered_customer_date`) y en los comentarios de `order_reviews`.

**2. Exploración de dimensiones principales y rango temporal**

En esta fase se examinan aquellas dimensiones principales que más tarde servirán como guía para agrupar y segmentar datos, así como se examinan aquellas columnas que nos aportan información sobre fechas con el objetivo de entender la temporalidad de los datos.
- Estudio de la dimensión "customer_state", utilizando DISTINCT para observar sus valores únicos.
- Estudio de la dimensión "order_status", para identificar los distintos estados de pedido existentes.
- Estudio de la dimensión "column2" (nombre de categoría de producto traducido), para observar las categorías de producto disponibles.
- Estudio de la dimensión "seller_state", para conocer la distribución geográfica de los vendedores.
- Estudio de la dimensión "review_score", para conocer la escala de puntuaciones posibles.
- Estudio de la dimensión "payment_type", para identificar los métodos de pago existentes.
- Estudio de las columnas asociadas a fechas, entendiendo el rango temporal cubierto por los pedidos y por las reseñas (fecha del primer y del último registro, y años de amplitud de cada rango).

**3. Feature Engineering y limpieza de columnas**

En esta fase se enriquece el modelo con columnas derivadas que facilitarán el análisis posterior, y se eliminan aquellas columnas de texto libre que no aportan valor cuantitativo.
- Creación de la columna `review_significado` en `olist_order_reviews_dataset`, que traduce cada `review_score` numérico (1 a 5) a una categoría interpretativa (Muy insatisfecho, Insatisfecho, Moderado, Satisfecho, Muy satisfecho).
- Eliminación de las columnas de texto libre `review_comment_title` y `review_comment_message` en `olist_order_reviews_dataset`, al no ser necesarias para el análisis cuantitativo planteado.
- Creación de la columna `profit` en `olist_order_items_dataset`, calculada como la diferencia entre el precio de venta (`price`) y el coste de flete (`freight_value`).
- Creación de las columnas `AÑO` y `MES` en `olist_orders_dataset`, extraídas a partir de `order_purchase_timestamp`, para facilitar el análisis temporal por periodos.

**4. Análisis inicial de datos cuantitativos**

En esta fase entramos de manera más profunda en aquellas dimensiones numéricas de valor que determinan el rendimiento de la plataforma, apoyándonos en las columnas creadas en la fase de feature engineering. Hablamos principalmente de datos como número de órdenes, clientes, vendedores, reseñas y productos, así como beneficio, precio y coste de flete.
- Conteo de órdenes, clientes, vendedores, reseñas, productos y categorías de producto distintas.
- Cálculo del beneficio total (`profit`) generado por la plataforma.
- Cálculo del precio medio (`price`) y del coste de flete medio (`freight_value`) derivados de las ventas.
- Consolidación de todas estas medidas en un único informe exploratorio (reporte cuantitativo), combinando los resultados mediante `UNION ALL`.

**5. Preparación al análisis descriptivo**

En esta última fase del análisis exploratorio se produce el comienzo del cruzado de datos, estableciendo relación entre dimensiones como el estado, la categoría de producto o el método de pago con columnas de valores numéricos como el importe pagado, el precio o la puntuación de reseña.
- Análisis cruzados: ventas según estado, pedidos según método de pago, precio medio según categoría, puntuación media según tiempo de entrega...
- Introducción al análisis descriptivo estableciendo rankings de dimensiones como productos, vendedores o estados según variables métricas.

## Preguntas de Negocio a Responder

### 1. Exploración de la Estructura y Dimensiones Principales
1. ¿Qué tablas componen la base de datos de Olist cargada en el proyecto?
2. ¿Qué columnas y tipos de datos contiene cada una de las siete tablas principales del dataset (`olist_customers_dataset`, `olist_order_items_dataset`, `olist_order_payments_dataset`, `olist_order_reviews_dataset`, `olist_orders_dataset`, `olist_products_dataset`, `olist_sellers_dataset`)?
3. ¿Qué estados (`customer_state`) distintos existen en la tabla de clientes?
4. ¿Qué estados de pedido (`order_status`) distintos existen en la tabla de pedidos?
5. ¿Qué categorías de producto distintas existen según la tabla de traducción `product_category_name_translation`?
6. ¿Qué estados (`seller_state`) distintos existen en la tabla de vendedores?
7. ¿Qué puntuaciones (`review_score`) distintas pueden otorgar los clientes en sus reseñas?
8. ¿Qué métodos de pago (`payment_type`) distintos se utilizan en la plataforma?

### 2. Exploración de Fechas
1. ¿Cuál es la fecha del primer y del último pedido registrado (`order_purchase_timestamp`), y cuántos años de actividad abarca el conjunto de datos?
2. ¿Cuál es la fecha de la primera y la última reseña registrada (`review_creation_date`), y cuántos años de amplitud tiene este rango?

### 3. Exploración de Medidas (Análisis Cuantitativo Inicial)
1. ¿Cuántas órdenes distintas (`order_id`) se han registrado en total?
2. ¿Cuántos clientes distintos (`customer_id`) han realizado pedidos?
3. ¿Cuántos vendedores distintos (`seller_id`) están registrados en la plataforma?
4. ¿Cuántas reseñas distintas (`review_id`) se han recibido?
5. ¿Cuántos productos distintos (`product_id`) componen el catálogo?
6. ¿Cuántas categorías de producto distintas (`product_category_name`) existen?
7. ¿Cuál es el beneficio total (`profit`) generado por la plataforma, calculado como la diferencia entre el precio de venta y el coste de flete?
8. ¿Cuál es el precio medio (`price`) de los productos vendidos?
9. ¿Cuál es el coste medio de flete (`freight_value`) derivado de las ventas?
10. ¿Cómo se resumen todas estas medidas (número de órdenes, clientes, vendedores, reseñas, productos, categorías, beneficio total, precio medio y coste de flete medio) en un único informe exploratorio?

### 4. Análisis de Magnitudes
1. ¿Cuántos clientes hay por estado, ordenados de mayor a menor?
2. ¿Cuántos vendedores hay por estado, ordenados de mayor a menor?
3. ¿Cuántos productos hay por categoría, ordenados de mayor a menor?
4. ¿Cuál es el importe total pagado por método de pago (`payment_type`)?
5. ¿Cuál es el total de ventas (`price`) por categoría de producto?
6. ¿Cuál es el total de ventas por estado del cliente?
7. ¿Cuál es la distribución de pedidos según el número de cuotas (`payment_installments`)?
8. ¿Cuál es la distribución de puntuaciones (`review_score`) otorgadas por los clientes?

### 5. Análisis de Ranking (Top N)
1. ¿Cuáles son las 5 categorías de producto que más ventas han generado?
2. ¿Cuáles son las 5 categorías de producto que menos ventas han generado?
3. ¿Cuáles son los 10 vendedores (`seller_id`) que más ventas han generado?
4. ¿Cuáles son los 3 estados con el mayor número de pedidos con retraso en la entrega?
5. ¿Cuáles son los 10 clientes (`customer_unique_id`) con mayor número de pedidos realizados?
