# Blind SQL Injection

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #sql #exploiting

***

## Definición

> La **Inyección SQL a ciegas (Blind SQL Injection)** es un ataque en aplicaciones web donde los atacantes insertan código SQL malicioso en campos de entrada para obtener información confidencial de la base de datos. A diferencia de otros tipos de Inyección SQL, en este caso, los atacantes no obtienen respuestas directas de la aplicación, sino que evalúan las respuestas ciegas para deducir información.

Nos enfrentamos a algo parecido a lo siguiente, donde no vemos el resultado por la página web, pero obtenemos distintos códigos según exista lo solicitado o no:

```php
<?php
......
$id = mysqli_real_escape_string($conn, $_GET['id']);

$data = mysql_query($conn, "select username form useres where id = $id");

$response = mysqli_fetch_array($data);

if(!isset($response['username'])){
	http_response_code(404)
}
.....
?>
```

Existen dos tipos de **Blind SQL Injection**:

* \[\[Time-based Blind SQL injection]]: En este tipo de inyecciones, el atacante utiliza una consulta que **tarda tiempo en ejecutarse** para obtener información.
* \[\[Boolean-based Blind SQL Injection]]: En este caso, se utilizan consultas con **expresiones booleanas** para obtener información adicional.
