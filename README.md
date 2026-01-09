PIPELINE DE CALIDAD DE DATOS CON SNOWFLAKE

Este proyecto demuestra la construcción de un pipeline de calidad y preparación de datos utilizando Snowflake y su interfaz Snowsight.

El objetivo es transformar un dataset de e-commerce solo lectura (Marketplace) en una tabla analítica limpia, consistente y lista para consumo, aplicando buenas prácticas reales de ingeniería de datos.

DATASET

Origen: Snowflake Marketplace

Tipo: Datos agregados de e-commerce por producto y mes

Métricas principales:

- estimated_views
- estimated_purchases

ARQUITECTURA Y CAPAS DE DATOS

- RAW DATA
- CLEAN
- DEDUP
- FINAL

PROCESO DE CALIDAD DE DATOS

Análisis y tratamiento de valores nulos

- Se realizó un perfilado de datos para identificar valores nulos por columna.
- Se definieron reglas de negocio aplicadas en una vista clean, sin modificar la capa raw:
- Campos de texto → 'UNKNOWN' / 'NO TITLE'
- Métricas numéricas → 0
- La fuente original se mantiene intacta para trazabilidad y auditoría.


Eliminación de duplicados lógicos

- Se definió una llave lógica de negocio. SITE + COUNTRY + YEAR + MONTH + PRODUCT
- Se agregaron las métricas (SUM) para asegurar una sola fila por entidad de negocio.
- Se conservaron atributos descriptivos mediante ANY_VALUE.


Tratamiento de anomalías de negocio

- Se identificaron casos donde las compras eran mayores a las veces que se visitó el sitio.
- Se aplicó una corrección conservadora:
  - Las compras se limitan al número de vistas.
  - Se agrega un campo IS_ANOMALY para mantener trazabilidad.
- No se elimina información ni se modifica la fuente original.

Materialización de la capa final

- Se creó una tabla persistente a partir de la vista final.
- Esta tabla está optimizada para consumo frecuente y mejor performance.
- Lista para ser utilizada por herramientas de visualización o análisis.


RESULTADO FINAL

Tabla final: DATASET_FINAL

Características:

- Sin valores nulos en campos analíticos
- Sin duplicados lógicos
- Sin anomalías de negocio activas
- Con trazabilidad de correcciones (IS_ANOMALY)
- Preparada para dashboards y análisis


APRENDIZAJES CLAVE

- Uso de Snowflake Marketplace y data sharing
- Diseño de capas de datos (raw → clean → final)
- Control de calidad de datos (nulos, duplicados, anomalías)
- Aplicación de reglas de negocio en capas analíticas
- Preparación de datos para BI y Analytics


NOTAS

El repositorio no incluye datos reales, solo scripts SQL y documentación, ya que el dataset es compartido vía Marketplace.
