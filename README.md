# Análisis de Ventas E-commerce

**Limpieza de Datos, SQL y Dashboard en Power BI**

Proyecto de análisis de datos de extremo a extremo sobre una plataforma de comercio electrónico: desde la limpieza de datos crudos hasta un dashboard interactivo y 15 consultas SQL de nivel básico a avanzado.

---

## Problema

Una plataforma de e-commerce dispone de datos crudos de ventas y clientes (20.000 clientes y 150.000 eventos de compra), pero sin procesar no permiten tomar decisiones. Los datos presentaban problemas de calidad:

- Edades imposibles (valores negativos y mayores a 2000)
- Fechas en formato texto
- Ausencia de información de producto (categoría y precio) necesaria para calcular ingresos

El objetivo fue transformar esos datos en información accionable que respondiera preguntas clave del negocio: ¿cuánto se vende?, ¿qué categorías y productos generan más ingresos?, ¿cómo evolucionan las ventas en el tiempo? y ¿quiénes son los clientes más valiosos?

---

## Solución

El proyecto se desarrolló en tres etapas:

### 1. Limpieza y preparación de datos (Python / pandas)

- Conversión de `basket_date` a formato fecha y de `customer_age` a entero.
- Tratamiento de valores atípicos en la edad: los registros con edad menor a 18 o mayor a 90 se reemplazaron por la mediana válida (32 años).
- Generación de un catálogo de productos sintético (13.161 productos) con categoría y precio, por no venir incluidos en los datos originales.
- Cálculo de la columna `revenue` (precio × cantidad) mediante el cruce de ventas con el catálogo.

### 2. Análisis SQL (SQLite)

Se resolvió un reto de 15 consultas, organizadas en tres niveles:

- **Básico:** total de ventas, ingreso total, precio promedio, top 10 de productos y categoría con mayores ingresos.
- **Intermedio:** facturación por cliente, clientes con más de 5 compras, producto más vendido por categoría, crecimiento mensual y participación porcentual por categoría.
- **Avanzado:** funciones de ventana (`RANK()`, `LAG()`, `SUM() OVER()`), clasificación de clientes con `CASE WHEN` y detección de clientes por encima del gasto promedio mediante subconsultas.

**Técnicas aplicadas:** `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `JOIN`, `CASE WHEN`, CTE, subconsultas y funciones de ventana.

### 3. Dashboard interactivo (Power BI)

- Modelo de datos en esquema estrella (tabla de hechos de ventas + dimensiones).
- Medidas DAX: `Ingreso_Total`, `Total_Ventas`, `Precio_Promedio`, `Clientes_Unicos`, `Ticket_Promedio` y `Crecimiento_Mensual`.
- Cinco KPIs principales, gráfico de ingresos por categoría, tendencia temporal, segmentación de clientes, tabla de top 10 productos y un segmentador interactivo por categoría.

---

## Resultados

- **Ingreso total:** USD 7.766.640,17 sobre 15.000 transacciones.
- **Precio promedio:** USD 240,27 | **Ticket promedio:** USD 517,78.
- **Clientes únicos:** 13.871.
- **Concentración por categoría:** Electrónica genera el 53,94% de los ingresos; le siguen Hogar (17,34%) y Deportes (14,88%).
- **Tendencia:** las ventas de junio cayeron un 31,57% respecto a mayo de 2019.
- **Segmentación de clientes:** 160 clientes VIP (gasto superior a USD 3.000), 1.886 Frecuentes y 11.825 Ocasionales.

Un grupo reducido de clientes concentra una parte desproporcionada del ingreso, lo que sugiere oportunidades claras de fidelización.

---

## Desafíos y aprendizajes

- **Formato numérico regional:** al importar los CSV en Power BI, la configuración regional interpretaba mal el punto decimal y desbordaba los valores. Se resolvió cambiando el tipo de dato con configuración regional "Inglés (Estados Unidos)" aplicada a ambas columnas a la vez.
- **Fechas como texto en SQL:** `strftime` dejó de reconocer la fecha; se usó `substr(basket_date, 1, 7)` como solución robusta para agrupar por mes.
- **IDs no coincidentes:** solo 64 de los `customer_id` coincidían entre las tablas de ventas y clientes. La segmentación VIP se recalculó directamente sobre la tabla de ventas (en Python y SQL) y se exportó ya clasificada, garantizando resultados correctos en el dashboard.

---

## Herramientas utilizadas

- **Python (pandas)** para limpieza y preparación.
- **SQLite** para el análisis SQL.
- **Power BI (DAX, esquema estrella)** para el dashboard.
- **Jupyter Lab** como entorno de desarrollo.

---

## Estructura del repositorio

```  
.  
├── data/  
│   ├── basket_details.csv  
│   ├── customer_details.csv  
│   ├── product_catalog.csv  
│   ├── ventas_powerbi.csv  
│   └── segmentacion_clientes.csv  
├── notebooks/  
│   └── 01_exploracion.ipynb  
├── sql/  
│   └── consultas.sql  
├── dashboard/  
│   └── dashboard.pdf  
└── README.md  
```

---

## Autor

**Javier Eduardo Ramos Rivera**
Analista de Datos | Ingeniería de Sistemas
