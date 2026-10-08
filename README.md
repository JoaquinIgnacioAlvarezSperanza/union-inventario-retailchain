# Practica UNION y UNION ALL - RetailChain

Respuestas a las preguntas del ejercicio sobre consolidacion de inventario:

## 1. ¿Cuántas filas devuelve cada consulta y por qué son distintas?

* **Consulta 1 (UNION):** Devuelve 11 filas.
* **Consulta 2 (UNION ALL):** Devuelve 14 filas.

Las consultas devuelven distinta cantidad de filas porque UNION elimina las coincidencias duplicadas para armar el catálogo único, mientras que UNION ALL mantiene la totalidad de los registros de ambas sucursales.

En los datos de prueba, los productos 103 (Monitor 4K 27"), 104 (Teclado Mecánico) y 106 (SSD Externo 1TB) están registrados tanto en la sucursal Norte como en la Sur. Al seleccionar los campos de catálogo (`id_producto`, `nombre_producto`, `categoria`), UNION detecta esas 3 filas idénticas y deja una sola representación de cada producto. En cambio, UNION ALL suma los 7 registros de la sucursal Norte con los 7 registros de la sucursal Sur dando un total de 14 filas.

## 2. ¿Por qué UNION ALL es más eficiente que UNION?

UNION ALL es más eficiente porque se limita a concatenar los conjuntos de datos verticalmente uno debajo del otro, sin modificar ni inspeccionar la información.

Por el contrario, UNION realiza un proceso adicional intensivo: compara cada fila contra las demás para identificar y remover duplicados. Para lograr esto, el motor de la base de datos debe aplicar una operación interna de ordenamiento (`SORT`) o tabla de dispersión (`HASH DISTINCT`) en memoria o disco. En tablas grandes, este filtro consume recursos significativos de CPU y memoria.

## 3. ¿En qué casos de negocio usarías cada uno?

**Casos para usar UNION:**

* Consolidar listas de clientes provenientes de dos bases de datos distintas (por ejemplo, CRM antiguo y CRM nuevo) para armar una campaña publicitaria sin enviar correos duplicados.
* Unificar catálogos de proveedores tras la fusión de dos empresas para mantener un registro maestro limpio.

**Casos para usar UNION ALL:**

* Consolidar reportes de ventas o facturación de distintos períodos (por ejemplo, `ventas_2024` y `ventas_2025`) para calcular la recaudación o volumen total.
* Unificar archivos de auditoría o logs de tráfico generados por distintos servidores web para analizar el volumen de tráfico global.

## 4. ¿Qué pasa si las columnas de ambas consultas no coinciden en número o tipo?

* **Número de columnas distinto:** La consulta falla inmediatamente y el gestor de SQL devuelve un error de sintaxis indicando que las sentencias combinadas deben tener exactamente el mismo número de expresiones (columnas) en sus selecciones.
* **Tipos de datos incompatibles:** Si la cantidad de columnas coincide pero sus tipos no son compatibles entre sí en la misma posición (por ejemplo, intentar unir un campo de texto `VARCHAR` con un campo `DATE`), el motor arroja un error de conversión de tipo de datos o falla al intentar castear los valores implícitamente.
