---------
- Tags: #OWASP_TOP_10 #vulnerabilidad #hacking #Mass_Asigment_Attack
--------------
# Definición

> Un **Ataque de asignación masiva (Mass Assignment Attack) - Parameter Binding** se refiere a un escenario de seguridad en el que un atacante intenta explotar la falta de restricciones adecuadas en la asignación de parámetros de una aplicación web. En este tipo de ataque, un atacante manipula los valores de los parámetros utilizados por la aplicación para actualizar o modificar recursos, como bases de datos o perfiles de usuarios, y puede realizar cambios no autorizados por el servidor.
> 
> Por ejemplo, si una aplicación web no valida correctamente quién puede modificar campos específicos de un perfil de usuario, un atacante podría enviar solicitudes manipuladas para cambiar información sensible de otros usuarios.

-----------
# Detección de vulnerabilidad

- **Observa los patrones de URL**: Examina las URLs generadas por la aplicación para ver si puedes identificar parámetros que parecen relacionarse con las asignaciones o modificaciones de recursos
- **Comparación de roles y permisos**: Si la aplicación tiene roles de usuario con diferentes niveles de acceso, intenta usar cuentas de prueba con diferentes roles para ver si puedes acceder a funcionalidades o realizar asignaciones que no deberían estar disponibles para tu rol.
---------
# Explotación

Una posible vía de ataque a una aplicación web vulnerable, es teniendo un campo de registro, intentar añadir más campos de los que se envían. Por ejemplo, si interceptamos la petición con [[Burpsuite]], y como respuesta al registro nos estan asignando un rol, podemos intentar enviar en el registro el rol que nos interese. Si no obtuviéramos como respuesta el rol o lo que queramos modificar, deberíamos probar con un poco de intuición y fuerza bruta.

---------------
# Ejemplos prácticos

- **Juice Shop**: [https://hub.docker.com/r/bkimminich/juice-shop](https://hub.docker.com/r/bkimminich/juice-shop)
- **skf-labs**: [https://github.com/blabla1337/skf-labs/tree/master/ruby/parameter-binding](https://github.com/blabla1337/skf-labs/tree/master/ruby/parameter-binding)