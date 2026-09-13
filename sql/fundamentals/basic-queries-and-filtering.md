# SQL: Basic Queries and Data Filtering

En este documento se recopilan los conceptos fundamentales de SQL que he estudiado hasta ahora, utilizando **SQLite** para realizar los ejercicios prácticos.

Los temas abarcan consultas básicas, filtrado de datos, búsquedas por patrones, ordenamiento, eliminación de duplicados y paginación de resultados.

---

# Concepto de Bases de Datos

Una base de datos es una colección de datos organizada. El adjetivo **"informática"** indica que esos datos se encuentran almacenados y gestionados mediante herramientas informáticas. En el caso de las bases de datos relacionales, un **SGBD (Sistema Gestor de Bases de Datos)** proporciona mecanismos para crear, consultar, modificar y administrar los datos, además de ofrecer características como eficiencia, concurrencia, integridad, seguridad y persistencia.

Podríamos definir una base de datos en sentido amplio como una colección de datos organizados y almacenados. Por ejemplo, podríamos imaginar distintos ficheros organizados y guardados en sus respectivos cajones. Sin embargo, una base de datos gestionada mediante un SGBD proporciona capacidades que permiten trabajar con esos datos de una forma estructurada y controlada.

Entre estas capacidades se encuentran:

1. **Gestión mediante un SGBD (Sistema Gestor de Bases de Datos).**  
   Permite crear, consultar, modificar y administrar los datos.

2. **Concurrencia controlada.**  
   Permite que múltiples usuarios o procesos accedan y, cuando corresponde, modifiquen los datos de manera controlada y consistente. No se refiere a consultas automatizadas, sino al acceso simultáneo a los datos.

3. **Integridad de los datos.**  
   Permite establecer reglas y restricciones para mantener los datos consistentes y válidos. La **integridad referencial**, por ejemplo, se utiliza para mantener relaciones válidas entre tablas mediante mecanismos como las claves foráneas (**foreign keys**).

4. **Persistencia y recuperación ante fallos.**  
   La persistencia significa que los datos pueden conservarse aunque termine el proceso o la sesión que los estaba utilizando. Los mecanismos de respaldo (**backups**) y recuperación permiten recuperar los datos después de determinados fallos.

5. **Seguridad y control de acceso.**  
   Permite administrar usuarios, permisos y mecanismos de autenticación y autorización.

6. **Independencia de los datos.**  
   Permite realizar determinados cambios en la estructura o en la forma de almacenamiento sin tener que modificar necesariamente todas las aplicaciones que utilizan esos datos.

El **SGBD** es el software encargado de gestionar y administrar una base de datos. No es correcto decir que el SGBD convierte una colección de datos en una base de datos: una base de datos puede entenderse como una colección organizada de datos, mientras que el SGBD proporciona las herramientas y mecanismos para gestionarla de forma estructurada.

# SQL

SQL es un **lenguaje**. **SQL (Structured Query Language)** es el lenguaje estándar utilizado principalmente para **consultar y manipular** datos en bases de datos **relacionales**.

SQL permite realizar operaciones como consultar, insertar, modificar y eliminar datos, además de definir y administrar estructuras dentro de una base de datos.

## SELECT

`SELECT` indica qué columnas o expresiones queremos recuperar en el resultado de una consulta.

Se utiliza escribiendo `SELECT` seguido del nombre de la columna de la cual queremos extraer los datos. Si queremos recuperar todas las columnas, podemos utilizar el signo `*`.

Para recuperar más de una columna lo podemos hacer escribiendo los nombres de las columnas separados por comas.

```sql
SELECT nombre, edad, altura
FROM personas;
```

También podemos utilizar `*` para recuperar todas las columnas:

```sql
SELECT *
FROM personas;
```

## FROM

Indica de qué tabla provienen los datos. Se utiliza después de `SELECT` y antes del nombre de la tabla de la cual queremos obtener los datos.

```sql
SELECT nombre
FROM personas;
```

En este caso:

- `SELECT` indica qué columna queremos recuperar.
- `FROM` indica de qué tabla provienen los datos.
- `personas` es la tabla que estamos consultando.

## LIMIT

Indica cuántas filas queremos obtener como máximo. Por ejemplo, podemos obtener las primeras 5 filas de la tabla:

```sql
SELECT *
FROM personas
LIMIT 5;
```

Podemos combinar esta cláusula con una consulta que seleccione determinadas columnas:

```sql
SELECT title
FROM videogames
LIMIT 3;
```

Esta consulta nos dará como máximo las primeras 3 filas del resultado, mostrando únicamente la columna `title`.

`LIMIT` no filtra los datos dependiendo del valor de una columna. Simplemente limita la cantidad de filas que devuelve el resultado.

## COMENTARIOS

Los comentarios son texto que el motor SQL ignora durante la ejecución de una consulta. Se utilizan principalmente para explicar partes de nuestro código, por ejemplo, qué hace una consulta.

Su sintaxis para un comentario de una línea utiliza el doble guion: `--`, seguido del comentario.

Asimismo, podemos comentar varias líneas utilizando la sintaxis:

```sql
/*
Comentario
de varias líneas
*/
```

Por ejemplo:

```sql
SELECT nombres
FROM personas;
--- Este texto también pertenece al comentario porque la sentencia de comentario comienza con --.
```

El punto y coma `;` se utiliza normalmente para indicar el final de una sentencia SQL. Sin embargo, su necesidad puede depender del entorno o herramienta que estemos utilizando, por lo que no debe considerarse universalmente obligatorio en todos los contextos.

## WHERE

Esta cláusula permite filtrar las **filas** que devuelve una consulta. Solo mostrará las filas que cumplan la condición especificada.

```sql
SELECT columna
FROM tabla
WHERE condición;
```

Por ejemplo:

```sql
SELECT *
FROM personas
WHERE edad > 18;
```

En este caso, únicamente se devolverán las filas donde el valor de `edad` sea mayor que 18.

## Operadores de Comparación

Los operadores de comparación permiten establecer condiciones dentro de una consulta.

| Operador | Descripción | Ejemplo |
|---|---|---|
| `=` | Igual a | `year = 2018` |
| `!=` | Distinto de | `year != 2018` |
| `>` | Mayor que | `edad > 50` |
| `<` | Menor que | `year < 2000` |
| `>=` | Mayor o igual que | `year >= 2010` |
| `<=` | Menor o igual que | `edad <= 20` |

Podemos filtrar por un valor exacto, por texto, utilizando los operadores de comparación, o combinar condiciones utilizando operadores lógicos como `AND`, `OR` y `NOT`.

```sql
SELECT *
FROM personas
WHERE year = 2018 AND edad > 16;
```

En este ejemplo, ambas condiciones deben cumplirse para que una fila sea incluida en el resultado.

- **AND:** Devuelve solo las filas que cumplen todas las condiciones a la vez.
- **OR:** Devuelve las filas que cumplen al menos una de las condiciones.
- **NOT:** Invierte una condición, es decir, devuelve las filas que no la cumplen.

## BETWEEN

Selecciona los valores que se encuentran dentro de un rango. **Los dos extremos del rango están incluidos.**

```sql
SELECT nombre, edad
FROM personas
WHERE year BETWEEN 2010 AND 2015;
```

En este caso se incluyen los valores `2010` y `2015`.

La palabra `AND` forma parte de la sintaxis específica de `BETWEEN`:

```sql
valor BETWEEN mínimo AND máximo
```

Aunque aparece la palabra `AND`, aquí no estamos utilizando el operador lógico `AND` de la misma forma que en:

```sql
WHERE year >= 2010
AND year <= 2015;
```

## IN

Comprueba si un valor coincide con alguno de los valores de una lista:

```sql
SELECT *
FROM personas
WHERE nombres IN ('Mario', 'Maria', 'Carmen');
```

Esta consulta devuelve las filas cuyo valor en `nombres` coincida con alguno de los valores indicados.

## LIKE

Permite realizar búsquedas por patrones específicos. `LIKE` permite buscar texto que coincida con un patrón determinado.

Para crear estos patrones usamos **wildcards**:

| Comodín | Descripción | Ejemplo |
|---|---|---|
| `%` | Cualquier secuencia de caracteres | `'The%'` |
| `_` | Un solo carácter | `'_inecraft'` |

Por ejemplo:

```sql
SELECT *
FROM videogames
WHERE title LIKE 'Ma%';
```

Esta consulta busca valores de `title` que comiencen con `Ma`.

### Wildcard `%`

`%` representa cero o más caracteres.

Por ejemplo:

```sql
SELECT *
FROM videogames
WHERE title LIKE 'The%';
```

Puede coincidir con valores como:

```text
The Legend of Zelda
The Witcher 3
The Last of Us
```

También podemos utilizar `%` al principio:

```sql
SELECT *
FROM videogames
WHERE title LIKE '%Craft';
```

En este caso, el patrón busca valores que terminen en `Craft`.

### Wildcard `_`

`_` representa exactamente un carácter.

Por ejemplo:

```sql
SELECT *
FROM videogames
WHERE title LIKE '_inecraft';
```

El patrón puede coincidir con:

```text
Minecraft
```

porque `_` representa el carácter `M`.

## ORDER BY

Podemos controlar el orden de los datos. Para ello podemos utilizar `ASC` y `DESC` (**Ascendente** y **Descendente**, respectivamente).

Se utiliza con la columna que deseemos ordenar, seguida del orden que queremos que tenga.

```sql
SELECT columna
FROM tabla
ORDER BY columna ASC;
```

También podemos utilizar:

```sql
SELECT columna
FROM tabla
ORDER BY columna DESC;
```

`ASC` y `DESC` indican la dirección del ordenamiento.

## DISTINCT

La función de esta cláusula es eliminar los duplicados del resultado. `DISTINCT` elimina las filas duplicadas del resultado, devolviendo únicamente valores únicos para las columnas seleccionadas.

No es una función, sino una **keyword** de SQL.

Se utiliza después de `SELECT` y antes de indicar la columna o columnas de las cuales queremos obtener valores únicos.

```sql
SELECT DISTINCT columna
FROM tabla;
```

Por ejemplo:

```sql
SELECT DISTINCT developer
FROM videogames;
```

Esta consulta devuelve cada desarrollador una sola vez, aunque aparezca en varias filas de la tabla.

## LIMIT y OFFSET

`LIMIT` restringe el número de filas que devuelve una consulta. En cambio, `OFFSET` indica cuántas filas saltar antes de empezar a devolver los resultados.

Combinando ambas cláusulas podemos implementar **paginación**, igual que cuando navegamos entre páginas de resultados en un sitio web:

```sql
SELECT *
FROM tabla
LIMIT 10 OFFSET 20;
```

De esta manera, `OFFSET 20` hace que se salten las primeras 20 filas y `LIMIT 10` hace que se devuelvan las siguientes 10.

Si utilizamos 10 resultados por página:

```text
Página 1 → LIMIT 10 OFFSET 0
Página 2 → LIMIT 10 OFFSET 10
Página 3 → LIMIT 10 OFFSET 20
```

Por lo tanto, `LIMIT 10 OFFSET 20` correspondería a la tercera página bajo este sistema de paginación.

# EJERCICIOS

Nombre de la tabla: **videogames**

| id | title | developer | year | playtime_hours |
|---:|---|---|---:|---:|
| 1 | The Legend of Zelda | Nintendo | 1986 | 20 |
| 2 | Super Mario Bros. | Nintendo | 1985 | 10 |
| 3 | Final Fantasy VII | Square Enix | 1997 | 40 |
| 4 | The Witcher 3 | CD Projekt Red | 2015 | 100 |
| 5 | Minecraft | Mojang Studios | 2011 | 9999 |
| 6 | Grand Theft Auto V | Rockstar Games | 2013 | 80 |
| 7 | Dark Souls | FromSoftware | 2011 | 60 |
| 8 | Portal 2 | Valve | 2011 | 8 |
| 9 | Red Dead Redemption 2 | Rockstar Games | 2018 | 70 |
| 10 | The Last of Us | Naughty Dog | 2013 | 15 |
| 11 | Halo: Combat Evolved | Bungie | 2001 | 10 |
| 12 | God of War | Santa Monica Studio | 2018 | 25 |

```sql
-- LISTAS DE EJERCICIOS --

-- 1. Selecciona todos los videojuegos ordenados por año de publicación de forma ascendente.
SELECT *              -- Seleccionamos todos los datos de la tabla.
FROM videogames       -- Elegimos la tabla de la cuál queramos sacar los datos.
ORDER BY year ASC;    -- Utilizamos esta cláusula seguido del nombre de la columna que queramos ordenar
                      -- y el orden que llevará, en este caso ascendente.


-- 2. Selecciona el título y las horas de juego, ordenados por horas de juego de mayor a menor.
SELECT title, playtime_hours
FROM videogames
ORDER BY playtime_hours DESC;


-- 3. Selecciona los desarrolladores únicos (sin repetir) de la tabla `videogames`.
SELECT DISTINCT developer
FROM videogames;

-- 4. Selecciona los años únicos en los que se publicaron videojuegos, ordenados de más antiguo a más reciente.
SELECT DISTINCT year
FROM videogames
ORDER BY year ASC;

-- 5. Muestra el título y las horas de los 3 videojuegos con más horas de juego (un top 3). Usa ORDER BY y LIMIT.
SELECT title, playtime_hours
FROM videogames
ORDER BY playtime_hours DESC -- Primero ordenamos la columna y al final le pedimos que nos entregue
                              -- los primeros 3 resultados y no al revés.
LIMIT 3;

-- 6. Pagina los resultados: muestra el título y el año ordenados por año ascendente,
-- saltando los 3 primeros y devolviendo los 3 siguientes. Usa LIMIT y OFFSET.
SELECT title, year
FROM videogames
ORDER BY year ASC
LIMIT 3 OFFSET 3;
```

---
# Key Takeaways

- `SELECT` permite especificar qué datos queremos recuperar.
- `FROM` indica de qué tabla provienen los datos.
- `WHERE` permite filtrar filas mediante condiciones.
- `AND`, `OR` y `NOT` permiten combinar o modificar condiciones.
- `BETWEEN` permite buscar valores dentro de un rango.
- `IN` permite comprobar si un valor pertenece a una lista.
- `LIKE` permite realizar búsquedas mediante patrones.
- `%` representa cero o más caracteres.
- `_` representa exactamente un carácter.
- `ORDER BY` permite ordenar los resultados.
- `DISTINCT` elimina valores duplicados del resultado.
- `LIMIT` limita el número de filas devueltas.
- `OFFSET` permite saltar un número determinado de filas.
- `LIMIT` y `OFFSET` pueden utilizarse para implementar paginación.
