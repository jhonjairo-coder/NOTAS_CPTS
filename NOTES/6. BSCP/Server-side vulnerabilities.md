
+ **Path traversal**
+ **Access control**
+ **Authentication**
+ **Server-side request forgery (SSRF)**
+ **File upload vulnerabilities**
+ **OS command injection**
+ **SQL injection**

## What is path traversal?

Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application. This might include:

#### Examples

`<img src="/loadImage?filename=218.png">`
`/var/www/images/218.png`
`https://insecure-website.com/loadImage?filename=../../../etc/passwd`
`/var/www/images/../../../etc/passwd`
`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

![[Pasted image 20260121084803.png]]

## What is access control?

Access control is the application of constraints on who or what is authorized to perform actions or access resources. In the context of web applications, access control is dependent on authentication and session management:

- **Authentication** confirms that the user is who they say they are.
- **Session management** identifies which subsequent HTTP requests are being made by that same user.
- **Access control** determines whether the user is allowed to carry out the action that they are attempting to perform.
### Vertical privilege escalation

Si un usuario puede acceder a funciones a las que no tiene permiso, se trata de una escalada vertical de privilegios. Por ejemplo, si un usuario no administrador puede acceder a una página de administración donde puede eliminar cuentas de usuario, se trata de una escalada vertical de privilegios

### Unprotected functionality ( Funcionalidad desprotegida)

En su forma más básica, la escalada vertical de privilegios se produce cuando una aplicación no aplica ninguna protección a las funciones sensibles. Por ejemplo, las funciones administrativas podrían estar vinculadas desde la página de bienvenida de un administrador, pero no desde la página de bienvenida de un usuario. Sin embargo, un usuario podría acceder a las funciones administrativas navegando a la URL de administrador correspondiente.

Ejemplos
`https://insecure-website.com/admin`
`https://insecure-website.com/robots.txt`

En algunos casos, se ocultan funciones sensibles al asignarles una URL menos predecible.

Ejemplo

`https://insecure-website.com/administrator-panel-yb556`

La URL podría revelarse en JavaScript

``` javascript
<script> var isAdmin = false; if (isAdmin) { ... var adminPanelTag = document.createElement('a'); adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556'); adminPanelTag.innerText = 'Admin panel'; ... } </script>
```

### Parameter-based access control methods

Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:

Ejemplo

```
https://insecure-website.com/login/home.jsp?admin=true 
https://insecure-website.com/login

/home.jsp?role=1
```


### Horizontal privilege escalation

La escalada horizontal de privilegios se produce si un usuario puede acceder a los recursos de otro usuario, en lugar de a los suyos propios.

Por ejemplo, si un empleado puede acceder a los registros de otros empleados además de a los suyos propios, se trata de una escalada horizontal de privilegios.

Ejemplo se busco en un post en guid de otra cuenta y en la url se envio la petición. con ese GUID
`https://web-security-academy.net/my-account?id=c51b05df-5900-4ef5-8c55-b8f35cb967f7`

### Escalada de privilegios de horizontal a vertical

A menudo, un ataque de escalada horizontal de privilegios puede transformarse en uno vertical, comprometiendo a un usuario con más privilegios.

	

### Server-side request forgery (SSRF)

![[Pasted image 20260121140417.png]]


### File upload vulnerabilities 

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their 
name, type, contents, or size. Failing to properly enforce restrictions on these could mean that even a basic image upload function can be used to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution.

### Exploiting unrestricted file uploads to deploy a web shell

`<?php echo file_get_contents('/path/to/target/file'); ?>`

`<?php echo system($_GET['command']); ?>`

`GET /example/exploit.php?command=id HTTP/1.1`

## Validación de tipo de archivo defectuosa

Al enviar formularios HTML, el navegador suele enviar los datos proporcionados en una `POST`solicitud con el tipo de contenido `application/x-www-form-urlencoded`. Esto es adecuado para enviar texto simple, como su nombre o dirección. Sin embargo, no es adecuado para enviar grandes cantidades de datos binarios, como un archivo de imagen completo o un documento PDF. En este caso, `multipart/form-data`se prefiere el tipo de contenido.

## Flawed file type validation - Continued

One way that websites may attempt to validate file uploads is to check that this input-specific `Content-Type` header matches an expected MIME type. If the server is only expecting image files, for example, it may only allow types like `image/jpeg` and `image/png`. Problems can arise when the value of this header is implicitly trusted by the server. If no further validation is performed to check whether the contents of the file actually match the supposed MIME type, this defense can be easily bypassed using tools like Burp Repeater.


### What is OS command injection?

OS command injection is also known as shell injection. It allows an attacker to execute operating system (OS) commands on the server that is running an application, and typically fully compromise the application and its data. Often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, and exploit trust relationships to pivot the attack to other systems within the organization.

|Purpose of command|Linux|Windows|
|---|---|---|
|Name of current user|`whoami`|`whoami`|
|Operating system|`uname -a`|`ver`|
|Network configuration|`ifconfig`|`ipconfig /all`|
|Network connections|`netstat -an`|`netstat -an`|
|Running processes|`ps -ef`|`tasklist`|

### What is SQL injection (SQLi)?

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.














