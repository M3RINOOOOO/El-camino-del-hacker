# SQLMap

***

* Tags: #herramientas #sql #hacking #exploiting #automático #no\_válido

***

La herramienta **SQLMap** es utilizada como un tipo de explotación automática hacia un servicio que podría ser vulnerable a \[\[SQL Injection]].

Para hacer uso de la herramienta, una vez en la página web en el lugar en el que pensemos que hay una vulnerabilidad a \[\[SQL Injection]], rellenamos el campo a enviar e interceptamos la petición con \[\[Burpsuite]]. Una vez interceptada, la copiamos a un archivo, por ejemplo, request.req.

Una vez tengamos dicho archivo, podemos ejecutar el siguiente comando para ver si muestra algún tipo de vulnerabilidad:

```bash
sqlmap -r request.req -p <parámetro_inyectable> --batch
```

Donde cada parámetro significa lo siguiente:

* `-r`: Indica cuál es la petición (mencionada anteriormente)
* `-p`: Indica cuál es el parámetro de la solicitud que creemos que puede ser vulnerable
* `--batch`: Indica que si encuentra a qué base de datos nos estamos enfrentando, no pruebe vulnerabilidades de otros tipos de bases de datos

Una vez sepamos que la página web es vulnerable, podemos listar las bases de datos existentes ejecutando el siguiente comando:

```bash
sqlmap -r request.req -p <parámetro_inyectable> --batch --dbs
```

Donde el nuevo parámetro significa:

* `-dbs`: Indica que liste las bases de datos existentes

Una vez sepamos las bases de datos existentes, podemos listar las tablas de alguna en específico de la siguiente manera:

```bash
sqlmap -r request.req -p <parámetro_inyectable> --batch -D <db_name> --tables
```

Donde los nuevos parámetros significan:

* `-D`: Indica qué base de datos se va a utilizar
* `--tables`: Indica que queremos que nos muestre las tablas existentes

Una vez sepamos las tablas que existen en una base de datos encontrada, podemos listar las columnas de la siguiente manera:

```bash
sqlmap -r request.req -p <parámetro_inyectable> --batch -D <db_name> -T <table_name> --columns
```

Donde los nuevos parámetros significan:

* `-T`: Indica qué tabla se va a utilizar
* `--columns`: Indica que queremos que nos muestre las columnas existentes

Una vez sepamos las columnas existentes en una tabla, podemos dumpear el contenido de las mismas con el siguiente comando:

```bash
sqlmap -r request.req -p <parámetro_inyectable> --batch -D <db_name> -T <table_name> -C <column_name> --dump
```

Donde los nuevos parámetros significan:

* `-C`: Indica qué columna se va a utilizar
* `--dump`: Indicamos que queremos dumpear el contenido seleccionado, además si se estuvieran mostrando contraseñas cifradas, trataría de descifrarlas

Si queremos enviar una solicitud al proxy (comúnmente lo haremos con \[\[Burpsuite]]), podemos ejecutar el siguiente comando:

```bash
sqlmap -r request.req -p <parámetro_inyectable> --batch --proxy <URL>
```

Donde el nuevo parámetro significa:

* `--proxy`: Indicamos la url de donde queremos establecer el proxy
