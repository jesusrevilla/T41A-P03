# T41A-P03
Descripción de índices en PostgreSQL

📘 ¿Qué es un índice en PostgreSQL?
Un índice es una estructura de datos que permite acceder rápidamente a las filas de una tabla    
según los valores de una o más columnas. Funciona como el índice de un libro: en lugar de leer   
página por página, puedes ir directo al contenido que buscas.

⚙️ Tipos de índices en PostgreSQL
PostgreSQL ofrece varios tipos de índices, cada uno optimizado para distintos casos de uso:

| Tipo de Índice | Descripción                                                      | Uso común                                           |
|----------------|------------------------------------------------------------------|-----------------------------------------------------|
| B-tree         | El más común. Ordena los datos para búsquedas rápidas.           | Comparaciones (=, <, >, BETWEEN)                   |
| Hash           | Usa funciones hash para búsquedas exactas.                       | Comparaciones con =                                |
| GIN            | Índice invertido para arrays, JSONB, texto completo.             | Búsqueda en documentos o arrays                    |
| GiST           | Índice generalizado para datos espaciales o personalizados.      | Geometría, texto aproximado                        |
| BRIN           | Índice compacto para grandes tablas ordenadas.                   | Consultas en columnas con valores correlacionados  |

🛠️ Cómo crear un índice

```sql
-- Crear un índice en la columna 'titulo' de la tabla 'libros'
CREATE INDEX idx_titulo ON libros(titulo);
```

-- Índice compuesto en varias columnas
CREATE INDEX idx_autor_anio ON libros(autor, anio);

-- Índice único (impide duplicados)
CREATE UNIQUE INDEX idx_isbn ON libros(isbn);
🚀 ¿Cuándo usar índices?
Usa índices cuando:

Realizas muchas búsquedas por una columna específica.

Filtras o ordenas frecuentemente por esa columna.

La tabla tiene muchos registros y las consultas se vuelven lentas.

Evita crear índices en:

Tablas pequeñas (PostgreSQL puede escanearlas rápido).

Columnas con muchos valores repetidos (como booleanos).

Columnas que cambian constantemente (los índices se deben actualizar).

🔍 Consultar el uso de índices
Puedes verificar si PostgreSQL está usando un índice con:

sql
EXPLAIN SELECT * FROM libros WHERE titulo = 'Cien Años de Soledad';
Esto te muestra el plan de ejecución. Si ves Index Scan, ¡el índice está funcionando!

🧹 Mantenimiento de índices
REINDEX: Reconstituye un índice dañado o ineficiente.

DROP INDEX: Elimina un índice que ya no se necesita.

VACUUM / ANALYZE: Ayuda al optimizador a decidir cuándo usar índices.
