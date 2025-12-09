# Documentación: Dashboard "Estado de situación"

Resumen
-------
Documentación de la solución presentada para el proyecto final del curso “Análisis de Datos” del Informatorio. El dashboard “Estado de situación de ventas” muestra la evolución mensual de las ventas, desgloses por segmento y subcategoría, análisis geolocalizado y KPIs clave. Además, incorpora una “Zona de Conclusión” con mensajes dinámicos generados mediante medidas DAX, que sintetizan hallazgos y recomendaciones accionables.

Estructura del modelo (tablas principales)
-----------------------------------------
- dimFecha
  - Tabla de fechas calculada con CALENDAR desde la fecha mínima a la máxima de `factVentas`.
  - Columnas importantes: `Date`, `Año`, `Mes`, `MesNum` (o `YearMonth`, `YearMonthNum`).
  - Está marcada como tabla de fechas local (LocalDateTable_...).
- dimCliente
  - Columnas: `ID_Cliente`, `Nombre`, `Segmento`, `Pais`, `Ciudad`, `ID_Fecha`.
- dimProducto
  - Columnas: `ID_Producto`, `Nombre_Producto`, `Categoria`, `Subcategoria`, `Precio`, `Costo`, `Margen`.
- factVentas
  - Hechos transaccionales con `ID_Venta`, `ID_Cliente`, `ID_Producto`, `ID_Fecha`, `Cantidad`, `Monto`, `Bruto`, `Fecha` (datetime).
  - Expandida con ubicaciones (Pais, Ciudad, Latitude, Longitude).


Medidas principales (DAX)
-------------------------
A continuación están las medidas más relevantes utilizadas. 

- Total Ventas
```dax
Total Ventas = SUM(factVentas[Monto])
```

- Ticket Promedio 
- Ventas Año Anterior
- Ventas Año Actual 
- Ventas Mes Actual
- Ticket Promedio Mes Actual 
- Variación % YoY


Medidas dinámicas para la "Zona de Conclusión"
----------------------------------------------
Se proporcionaron dos medidas DAX que generan texto dinámico para la sección de recomendaciones

1) Mensaje Resumen Ejecutivo (texto corto con ventas MTD, ticket promedio y top segmento)
2) Mensaje Recomendación Accionable (decisión simple según YoY y top subcategoría)

Notas sobre estas medidas
- Ambas usan MTD (mes hasta hoy).
- Las medidas respetan filtros del informe (por ejemplo proveedor o categoría)

Diseño del informe (visualización)
---------------------------------
El informe "Estado de situación" incluye los siguientes items:

Gráficos:
1. [ANUAL] - Variación de ventas interanual 
2. [MENSUAL] - Ventas por segmento de cliente
3. [MENSUAL] - Ventas por subcategoría de producto 
4. [MENSUAL] - Ventas geolocalizadas

KPIs:
1. Ventas mensuales
2. Ticket promedio mensual
3. Total ventas en año actual
4. Ticket promedio anual

Zona de Conclusión 
- Smart narrative con los resultados de las medidas "Mensaje..."