# Cron

> **Cron** es un servicio de Unix/Linux que se utiliza para programar tareas automáticas en el sistema. Las tareas que se desean automatizar se escriben en un archivo conocido como **crontab**, que especifica la hora y la frecuencia con la que se deben ejecutar los comandos o scripts.

El intervalo de tiempo se especifica mediante 5 campos que representan, de izquierda a derecha:

* Minutos: de 0 a 59.
* Horas: de 0 a 23.
* Día del mes: de 1 a 31.
* Mes: de 1 a 12.
* Día de la semana: de 1 a 6 lunes a sábado (1=lunes, 2=martes, etc.) y 0 o 7 el domingo.

Si quisiéramos especificar todos los valores posibles de un parámetro (minutos, horas, etc.) podemos hacer uso del asterisco. Esto implica que si en lugar de un número utilizamos un asterisco, el comando indicado se ejecutará cada minuto, hora, día de mes, mes o día de la semana

A continuación un par de ejemplos:

```
* * * * * comando      cada minuto se ejecuta esa instrucción
1 * * * * comando      se va a ejecutar cada hora xx:01
*/5 * * * * comando     se va a ejecutar cada 5 minutos
* 2 * * * comando      se va a ejecutar cada minuto en este rango 02:00-02:59
30 2 * * * comando     se va a ejecutar a las 02:30
*/5 2 * * * comando    se va a ejecutar cada 5 minutos en el rango 02:00-02:59
*/2 */2 * * *       se va a ejecutar cada 2 minutos, cada 2 horas
* * 2 * * comando      se va a ejecutar cada minuto, todos los días 2 de los meses
* * * 5 * comando      se va a ejecutar cada minuto, en mayo
2 3 2 5 * comando      se va a ejecutar cada dos minutos, en el rango 03:00-03:59, el dia 2 de mayo uwu
* * * * 4 comando      se va a ejecutar cada minuto todos los jueves
* * 2 * 4 comando      se va a ejecutar cada minuto, todos los días 2 de los meses, y ADEMÁS los jueves
```

Una web bastante interesante para practicar es la siguiente: [site24x7](https://www.site24x7.com/es/tools/crontab/cron-generator.html)
