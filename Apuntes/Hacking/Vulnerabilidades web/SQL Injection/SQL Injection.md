----------------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #sql  #exploiting 
----------
# Definición

> La **inyección SQL (SQL Injection)** es una técnica de ataque utiliza para explotar vulnerabilidades en aplicaciones web donde no se realiza correctamente la validación de la entrada del usuario en la consulta enviada a la base de datos relacionales, a diferencia de las [[NoSQLi]]. Se puede utilizar esta técnica para conseguir información confidencial almacenada en la base de datos.

Hay distintos tipos de inyecciones, entre los que explicamos los siguientes:
- [[Error-Based SQL Injection]]: Con este tipo de inyecciones, el atacante se aprovecha de **errores en el código SQL** para obtener información. 
- [[Non-Error-Based SQL Injection]]: Con este tipo de inyecciones, el atacante se aprovecha de **errores en el código SQL** para obtener información. 
- [[Blind SQL Injection]]: En este tipo de inyecciones, el atacante no obtiene respuestas directas de la aplicación, sino que se evaluan **respuestas ciegas**
  - [[Time-based Blind SQL injection]]
  - [[Boolean-based Blind SQL Injection]]


Podemos hacer uso de herramientas automáticas como [[SQLMap]] para detectar este tipo de vulnerabilidad.



