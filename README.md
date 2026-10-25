# Base de datos SQL - Trabajo Académico

**Asignatura:** Base de Datos

**Motor:** SQLite (DB Browser for SQLite)

**Base de Datos Utilizada:** `sistema_colegio.db`

## INTRODUCCIÓN

En estas prácticas trabajamos con un archivo físico de base de datos relacional llamado `sistema_colegio.db`.

El proceso se dividió en dos etapas:

1. **TP N°2 (DDL - Data Definition Language):** Creación de la estructura de la base de datos (tablas, claves primarias, columnas y tipos de datos) y modificaciones en las tablas.

2. **TP N°3 (DML - Data Manipulation Language):** Inserción de registros, consultas con filtros, actualización de datos y eliminación de registros obsoletos.

# TRABAJO PRÁCTICO N°2: CREACIÓN DE ESTRUCTURAS Y TIPOS DE DATOS

### Ejercicio 1: Creación del Archivo Raíz y Análisis de Tipos de Datos

Se creó el archivo raíz de la base de datos mediante la opción *Nueva Base de Datos* de DB Browser, guardándolo como `sistema_colegio.db`.

A continuación, se definieron los tipos de datos apropiados para SQLite según el análisis del dominio:

* **A) Número de DNI:** `INTEGER` (Números enteros sin coma).

* **B) Precio de cuota mensual:** `REAL` (Decimales para representar importes).

* **C) Adeuda documentación:** `INTEGER` (Servirá como valor booleano: 1 = Verdadero/Entregado, 0 = Falso/Adeuda).

```
-- Ejercicio 1: Tipos de datos sugeridos
-- A) DNI: INTEGER
-- B) Cuota Mensual: REAL
-- C) Documentación Pendiente: INTEGER (0 o 1)

```

### Ejercicio 2: Creación de Registros Escolares (Tabla Alumnos)

Se creó la tabla `Alumnos` con sus tipos de datos, definiendo `Legajo` como clave primaria autoincremental (`PRIMARY KEY AUTOINCREMENT`).

```
CREATE TABLE Alumnos (
    Legajo INTEGER PRIMARY KEY AUTOINCREMENT,
    Nombre_Completo TEXT,
    Curso TEXT,
    Promedio REAL,
    Documentacion_Ok INTEGER
);

```

### Ejercicio 3: El Inventario de la Despensa (Tabla Productos)

Se diseñó la tabla `Productos` fijando `Codigo_Barras` como clave primaria no autoincremental, ya que este código viene predefinido comercialmente.

```
CREATE TABLE Productos (
    Codigo_Barras INTEGER PRIMARY KEY,
    Nombre TEXT,
    Precio REAL,
    Stock_Actual INTEGER
);

```

### Ejercicio 4: Modificación de Estructura (`ALTER TABLE`)

Se utilizó la instrucción `ALTER TABLE` combinada con `ADD COLUMN` para agregar el atributo `Categoria` a la tabla `Productos` previamente creada.

```
ALTER TABLE Productos
ADD COLUMN Categoria TEXT;

```

### Ejercicio 5: Borrado y Limpieza (`DROP TABLE`)

Para evitar mantener tablas temporales u obsoletas, se creó primero la tabla de prueba `Proveedores_Viejos` y posteriormente se eliminó por completo del sistema utilizando `DROP TABLE`.

```
-- Creación de la tabla de prueba
CREATE TABLE Proveedores_Viejos (
    ID INTEGER,
    Nombre TEXT
);

-- Eliminación definitiva de la tabla
DROP TABLE Proveedores_Viejos;

```

# TRABAJO PRÁCTICO N°3: MANIPULACIÓN Y CONSULTA DE DATOS

Con la estructura de la base de datos ya lista, se procedió a cargar y consultar información.

### Ejercicio 1: Matriculación de Alumnos (`INSERT INTO`)

Se insertaron 4 estudiantes en la tabla `Alumnos`. Nota: La columna `Legajo` se omitió de la lista de campos ya que SQLite asigna su valor automáticamente al ser de tipo `AUTOINCREMENT`.

```
INSERT INTO Alumnos (Nombre_Completo, Curso, Promedio, Documentacion_Ok) 
VALUES ('Lucía Gómez', '1ro A', 8.5, 1);

INSERT INTO Alumnos (Nombre_Completo, Curso, Promedio, Documentacion_Ok) 
VALUES ('Matías Pérez', '2do B', 6.2, 0);

INSERT INTO Alumnos (Nombre_Completo, Curso, Promedio, Documentacion_Ok) 
VALUES ('Sofía Benítez', '1ro A', 9.0, 1);

INSERT INTO Alumnos (Nombre_Completo, Curso, Promedio, Documentacion_Ok) 
VALUES ('Lucas Díaz', '3ro C', 5.8, 0);

```

### Ejercicio 2: Verificación y Reportes (`SELECT`)

1. **Consulta general:** Muestra todos los campos y registros cargados.

2. **Consulta específica:** Extrae un reporte simplificado únicamente con el nombre y curso.

```
-- 1. Ver todos los campos de todos los alumnos
SELECT * FROM Alumnos;

-- 2. Reporte simplificado para preceptoría
SELECT Nombre_Completo, Curso FROM Alumnos;

```

### Ejercicio 3: Filtros Académicos (`WHERE`)

Se aplicaron condiciones lógicas para filtrar los resultados según criterios académicos:

```
-- 1. Alumnos aprobados / destacados (Promedio igual o mayor a 7.0)
SELECT * FROM Alumnos 
WHERE Promedio >= 7.0;

-- 2. Alumnos que deben papeles (Documentacion_Ok igual a 0)
SELECT * FROM Alumnos 
WHERE Documentacion_Ok = 0;

```

### Ejercicio 4: Abasteciendo la Despensa (`INSERT` y `SELECT` con condición)

Se cargaron productos en el inventario incluyendo el código de barras manual de 4 dígitos y la nueva categoría. Luego se consultó cuáles requerían reposición urgente.

```
-- 1. Carga de productos
INSERT INTO Productos (Codigo_Barras, Nombre, Precio, Stock_Actual, Categoria) 
VALUES (1001, 'Yerba', 2500.00, 15, 'Almacén');

INSERT INTO Productos (Codigo_Barras, Nombre, Precio, Stock_Actual, Categoria) 
VALUES (1002, 'Azúcar', 1200.50, 8, 'Almacén');

INSERT INTO Productos (Codigo_Barras, Nombre, Precio, Stock_Actual, Categoria) 
VALUES (1003, 'Leche', 1400.00, 5, 'Lácteos');

INSERT INTO Productos (Codigo_Barras, Nombre, Precio, Stock_Actual, Categoria) 
VALUES (1004, 'Fideos', 950.00, 20, 'Almacén');


SELECT * FROM Productos 
WHERE Stock_Actual < 10;

```

### Ejercicio 5: Actualización de Precios y Correcciones (`UPDATE`)

Se usó el comando `UPDATE` junto con la cláusula `WHERE` para restringir los cambios a registros específicos y prevenir la modificación accidental de toda la tabla.

```
-- 1. Modificación de precio a la Yerba usando su clave primaria
UPDATE Productos 
SET Precio = 2800.00 
WHERE Codigo_Barras = 1001;

-- 2. Corrección de stock para el Azúcar
UPDATE Productos 
SET Stock_Actual = 50 
WHERE Nombre = 'Azúcar';

-- 3. Verificación final de cambios
SELECT * FROM Productos;

```

### Ejercicio 6: Limpieza de Catálogo (`DELETE`)

Se dio de baja del catálogo al producto "Fideos" mediante el uso de `DELETE` condicionado con `WHERE`.

```
-- Eliminación del producto Fideos
DELETE FROM Productos 
WHERE Nombre = 'Fideos';

-- Comprobación final del inventario
SELECT * FROM Productos;

```