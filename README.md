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

---

🛠️ Cómo crear un índice

```sql
-- Crear un índice en la columna 'titulo' de la tabla 'libros'
CREATE INDEX idx_titulo ON libros(titulo);
```


```sql
-- Índice compuesto en varias columnas
CREATE INDEX idx_autor_anio ON libros(autor, anio);
```
```sql
-- Índice único (impide duplicados)
CREATE UNIQUE INDEX idx_isbn ON libros(isbn);
```

---
🚀 ¿Cuándo usar índices?
Usa índices cuando:

- Realizas muchas búsquedas por una columna específica.

- Filtras u ordenas frecuentemente por esa columna.

- La tabla tiene muchos registros y las consultas se vuelven lentas.

Evita crear índices en:

- Tablas pequeñas (PostgreSQL puede escanearlas rápido).

- Columnas con muchos valores repetidos (como booleanos).

- Columnas que cambian constantemente (los índices se deben actualizar).
---

🔍 Consultar el uso de índices
Puedes verificar si PostgreSQL está usando un índice con:

```sql
EXPLAIN SELECT * FROM libros WHERE titulo = 'Cien Años de Soledad';
```
Esto te muestra el plan de ejecución. Si ves Index Scan, ¡el índice está funcionando!

🧹 Mantenimiento de índices  

- **REINDEX**: Reconstituye un índice dañado o ineficiente.
- **DROP INDEX**: Elimina un índice que ya no se necesita.
- **VACUUM / ANALYZE**: Ayuda al optimizador a decidir cuándo usar índices.

---
🔍 ¿Qué son los operadores relacionales?
Los operadores relacionales comparan dos valores y devuelven un resultado booleano:   
`TRUE`, `FALSE` o `NULL`. Se usan principalmente en cláusulas como `WHERE`, `JOIN`, `HAVING`, y en expresiones condicionales.

📋 Tabla de operadores relacionales en SQL
| Operador | Significado               | Ejemplo                          | Resultado esperado             |
|----------|---------------------------|----------------------------------|--------------------------------|
| =        | Igual a                   | `autor = 'Cervantes'`            | Verdadero si autor es Cervantes |
| <>       | Distinto de               | `anio <> 2020`                   | Verdadero si el año no es 2020 |
| !=       | Distinto de (alternativo) | `anio != 2020`                   | Igual que `<>`                 |
| >        | Mayor que                 | `precio > 100`                   | Verdadero si precio es mayor a 100 |
| <        | Menor que                 | `edad < 18`                      | Verdadero si edad es menor a 18 |
| >=       | Mayor o igual que         | `stock >= 10`                    | Verdadero si hay al menos 10 unidades |
| <=       | Menor o igual que         | `descuento <= 20`                | Verdadero si el descuento es 20 o menos |
| IS NULL  | Es nulo                   | `fecha_entrega IS NULL`          | Verdadero si no hay fecha registrada |
| IS NOT NULL | No es nulo             | `email IS NOT NULL`              | Verdadero si el email está presente |

---

🧠 ¿Dónde se usan?
1. Filtrar resultados
```sql
SELECT * FROM libros WHERE anio > 2000;
```
2. Condiciones en relaciones
```sql
SELECT * FROM prestamos
WHERE fecha_prestamo IS NOT NULL;
```
3. Validaciones en procedimientos o funciones
```sql
IF stock <= 0 THEN
  RAISE NOTICE 'Producto agotado';
END IF;
```

⚠️ Consideraciones importantes
Comparar con `NULL` requiere `IS NULL` o `IS NOT NULL`, no `=` ni `<>`.

En PostgreSQL, puedes usar `!=` aunque `<>` es el estándar SQL.

Las comparaciones pueden verse afectadas por el tipo de datos (por ejemplo, texto vs número).
