---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #AXFR
--------------
# Definición

> Un **ataque de transferencia de zona** es una técnica de ciberataque dirigida a servidores de nombres de dominio (**DNS**) con el objetivo de obtener información confidencial sobre la estructura y los registros de un dominio específico. En este tipo de ataque, el atacante intenta persuadir al servidor **DNS** para que le proporcione una copia completa de la zona, que es una lista de todos los registros de un dominio. Esto puede exponer información sensible, como direcciones IP internas y configuraciones de seguridad, lo que facilita la planificación de ataques adicionales.
> 
> El ataque **AXFR** se lleva a cabo enviando una solicitud de transferencia de zona desde un servidor **DNS falso** a un servidor **DNS legítimo**. Esta solicitud se realiza utilizando el protocolo de transferencia de zona DNS (AXFR), que es utilizado por los servidores DNS para transferir registros DNS de un servidor a otro.

-----------
# Detección de vulnerabilidad

- **Utiliza herramientas de prueba de transferencia de zona**: Hay herramientas en línea y de línea de comandos que pueden ayudarte a probar si un dominio es vulnerable a la transferencia de zona DNS. Una de las más populares es [[dig]].
- **Verifica la configuración DNS**: Puedes usar herramientas de verificación de DNS, como **DNSCheck**, para evaluar la configuración general de DNS de un dominio.

---------
# Explotación

Para comenzar, una base de datos que almacene información del **DNS** puede tener la siguiente forma:

![[Pasted image 20231010163608.png]]

La línea 8 y 9 hace alusión a los servidores de nombre que lo que se encargan es de aplicar la resolución de nombres de dominio para la zona. 
Las líneas de la 11 a la 19 se refieren a los subdominios existentes. Suponiendo que el dominio se llama *example.com*, sabemos que existe un subdominio *admin.example.com*. El **CNAME** se encarga de "sustituir" las palabras por la que siguen, por ejemplo se sustituiría *wap* por *www*.

Podemos utilizar [[dig]] para intentar conseguir dicha información:

``` bash
dig axfr @<IP> <DOMINIO>
```

---------------
# Ejemplos prácticos

- **DNS-Zone-Transfer** : [https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer](https://github.com/vulhub/vulhub/tree/master/dns/dns-zone-transfer)