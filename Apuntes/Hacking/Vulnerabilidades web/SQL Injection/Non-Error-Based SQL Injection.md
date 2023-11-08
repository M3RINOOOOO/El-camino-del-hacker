----------------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #sql  #exploiting 
----------
# Definición

> La **Inyección de SQL no basada en errores** es un tipo de ataque informático donde se inserta código malicioso a través de entradas en aplicaciones web. A diferencia de la [[Error-Based SQL Injection]], no se apoya en mensajes de error para obtener información. En su lugar, busca alterar la ejecución de consultas SQL para acceder a datos no autorizados o manipular la base de datos.

# Pasos

> Para este tipo de inyecciones, emplearemos el mismo procedimiento utilizado en [[Error-Based SQL Injection]], solo que ahora iremos un poco a ciegas, pero los pasos y resultados serán los mismos. Si cuando hacemos una petición, obtenemos una respuesta, la cosa es ir jugando con los métodos mencionados hasta que no obtengamos respuesta o hasta que encontremos algo que nos llame la atención.