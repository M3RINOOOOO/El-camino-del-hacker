-----------------
- Tags: #web #wordpress #reconocimiento #herramientas
-----------------

La herramienta **Wpscan** [^1] es comúnmente utilizada para enumerar páginas web que dispongan de un gestor de contenidos [[WordPress]]

Para realizar un escaneo a modo de prueba podemos ejecutar el siguiente comando:

```bash
wpscan --url https://prueba.com
```

Si quisiéramos enumerar usuarios válidos potenciales existentes detrás de este gestor de contenidos, podríamos ejecutar el siguiente comando:

```bash
wpscan --url https://prueba.com --enumerate u
```

Otra de las utilidades de esta herramienta es que es capaz de aplicar fuerza bruta para descubrir credenciales en los paneles de autentificación:

```bash
wpscan --url https://prueba.com -U user -P /usr/share/wordlists/rockyou.txt
```

Cabe destacar que este procedimiento, también puede aplicarse de forma manual [^2] abusando del archivo **xmlrpc.php**, siendo necesario crear para ello un script en Bash o python.

------------------
## Referencias

[^1]: Enlace al proyecto en Github: [Enlace](https://github.com/wpscanteam/wpscan)
[^2]: Procedimiento manual abusando del XMLRPC: [[XMLRPC]]


