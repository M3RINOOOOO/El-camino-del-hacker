# Control de flujo

En este apartado, veremos diferentes sintaxis que se pueden emplear para ejecutar comandos a nuestro parecer o del resultado de los comandos ejecutados.

Si queremos ejecutar dos acciones seguidas, independientemente del resultado de los comandos:

```bash
accion1; accion2
```

Si queremos ejecutar una acción solo si se ha completado exitosamente la primera **echo $?** hacemos uso del operador &&:

```bash
accion1 && accion2     #Se ejecuta accion1 y si devuelve un estado correcto, se ejecuta accion2, de lo contrario no
```

Si queremos ejecutar una acción solo si **no** se ha completado exitosamente la primera hacemos uso del operador ||:

```bash
accion1 || accion2     #Se ejecuta accion1 y si devuelve un estado de error, se ejecuta accion2, de lo contario no
```

Si queremos no ver por consola los errores que ha proporcionado un comando, lo podemos redirigir al **stderr**:

```bash
accion 2>/dev/null     #Si accion devuelve un error no se mostrará por consola
```

Si queremos no ver por consola la salida del comando, lo podemos redirigir al **stdout**:

```bash
accion &>/dev/null
```

Si queremos poner en segundo plano alguna tarea que queramos lanzar, se añade al final un &:

```bash
accion &       #accion quedará en segundo plano, aunque dependerá de la consola desde que se lanzó (si se cierra la terminal, finaliza accion)
```

Si queremos poner en segundo plano alguna tarea que queramos lanzar y además que no tenga relación con la terminal desde la que se ejecutó, hacemos uso del comando disown

```bash
accion & disown
```
