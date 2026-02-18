Los  endpoint y flujos de autenticación son activos que deben protegerse. Además, la opción "Olvidé mi contraseña/Restablecer contraseña" debe tratarse del mismo modo que los mecanismos de autenticación.

Una API es vulnerable si:

- Permite el robo de credenciales donde el atacante usa fuerza bruta con una lista de nombres de usuario y contraseñas válidos.
- Permite a los atacantes realizar un ataque de fuerza bruta en la misma cuenta de usuario, sin presentar un mecanismo de bloqueo de cuenta/captcha.
- Permite contraseñas débiles.
- Envía detalles de autenticación confidenciales, como tokens de autorización y contraseñas en la URL.
- Permite a los usuarios cambiar su dirección de correo electrónico, contraseña actual o realizar cualquier otra operación sensible sin pedir confirmación de contraseña.
- No valida la autenticidad de los tokens.
- Acepta tokens JWT sin firmar o con firma débil ( `{"alg":"none"}`)
- No valida la fecha de vencimiento de JWT.
- Utiliza contraseñas de texto simple, no cifradas o con un hash débil.
- Utiliza claves de cifrado débiles.

Además, un microservicio es vulnerable si:

- Otros microservicios pueden acceder a él sin autenticación
- Utiliza tokens débiles o predecibles para imponer la autenticación

## Escenario n.° 1

Para realizar la autenticación del usuario, el cliente debe emitir una solicitud API como la que se muestra a continuación con las credenciales del usuario:

Si las credenciales son válidas, se devuelve un token de autenticación, que debe proporcionarse en solicitudes posteriores para identificar al usuario. Los intentos de inicio de sesión están sujetos a una limitación de velocidad: solo se permiten tres solicitudes por minuto.

## Escenario #2

Para actualizar la dirección de correo electrónico asociada a la cuenta de un usuario, los clientes deben emitir una solicitud de API como la que se muestra a continuación:

Debido a que la API no requiere que los usuarios confirmen su identidad proporcionando su contraseña actual, los actores maliciosos capaces de ponerse en posición de robar el token de autenticación podrían tomar el control de la cuenta de la víctima iniciando el flujo de trabajo de restablecimiento de contraseña después de actualizar la dirección de correo electrónico de la cuenta de la víctima.

## Cómo prevenir

- Asegúrate de conocer todos los flujos posibles para autenticarte en la API (móvil, web, enlaces profundos que implementan autenticación con un solo clic, etc.). Pregunta a tus ingenieros qué flujos te faltó.
- Infórmate sobre tus mecanismos de autenticación. Asegúrate de comprender qué son y cómo se utilizan. OAuth no es autenticación, ni tampoco lo son las claves API.
- No reinventes la rueda en la autenticación, la generación de tokens ni el almacenamiento de contraseñas. Usa los estándares.
- Los puntos finales de recuperación de credenciales/contraseña olvidada deben tratarse como puntos finales de inicio de sesión en términos de fuerza bruta, limitación de velocidad y protección de bloqueo.
- Requerir nueva autenticación para operaciones sensibles (por ejemplo, cambiar la dirección de correo electrónico del propietario de la cuenta o el número de teléfono 2FA).
- Utilice la [hoja de trucos de autenticación de OWASP](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html) .
- Siempre que sea posible, implemente la autenticación multifactor.
- Implemente mecanismos anti-fuerza bruta para mitigar el robo de credenciales, los ataques de diccionario y los ataques de fuerza bruta en sus endpoints de autenticación. Este mecanismo debe ser más estricto que los mecanismos habituales de limitación de velocidad en sus API.
- Implementar mecanismos [de bloqueo de cuentas](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/04-Authentication_Testing/03-Testing_for_Weak_Lock_Out_Mechanism\(OTG-AUTHN-003\)) y captcha para evitar ataques de fuerza bruta contra usuarios específicos. Implementar comprobaciones de contraseñas débiles.
- Las claves API no deben usarse para la autenticación de usuarios. Solo deben usarse para la autenticación [de clientes API](https://cloud.google.com/endpoints/docs/openapi/when-why-api-key) .
