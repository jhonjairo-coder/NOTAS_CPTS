Descubrimiento de documentación

Incluso si la documentación de la API no está disponible abiertamente, es posible que puedas acceder a ella explorando aplicaciones que usan la API.

Para ello, puede usar Burp Scanner para rastrear la API. También puede explorar las aplicaciones manualmente con el navegador de Burp. Busque puntos finales que puedan hacer referencia a la documentación de la API, por ejemplo:

- `/api`
- `/swagger/index.html`
- `/openapi.json`
Si identifica un punto final para un recurso, asegúrese de investigar la ruta base. Por ejemplo, si identifica el punto final del recurso `/api/swagger/v1/users/123`, debería investigar las siguientes rutas:

- `/api/swagger/v1`
- `/api/swagger`
- `/api`


## Identifying API endpoints


Identifying supported HTTP methods

![[Pasted image 20260122080859.png]]

The HTTP method specifies the action to be performed on a resource. For example:

- `GET` - Retrieves data from a resource.
- `GET`- Recupera datos de un recurso.
- `PATCH` - Applies partial changes to a resource.
- `PATCH`- Aplica cambios parciales a un recurso.
- `OPTIONS` - Retrieves information on the types of request methods that can be used on a resource.
- `OPTIONS`- Recupera información sobre los tipos de métodos de solicitud que se pueden utilizar en un recurso.

``` bash
#  All HTTP Verbs Defined in RFC's + 1 ARBITRARY Verb 1.0 - (Update: 16 March 2010)
# creative commons
OPTIONS
GET
HEAD
POST
PUT
DELETE
TRACE
CONNECT
PROPFIND
PROPPATCH
MKCOL
COPY
MOVE
LOCK
UNLOCK
VERSION-CONTROL
REPORT
CHECKOUT
CHECKIN
UNCHECKOUT
MKWORKSPACE
UPDATE
LABEL
MERGE
BASELINE-CONTROL
MKACTIVITY
ORDERPATCH
ACL
PATCH
SEARCH
ARBITRARY
```

Se realizo peticion y respondio no autortizado porque no se tenia login de sesion

![[Pasted image 20260122081250.png]]

Con el login se envió la petición nuevamente y nos dio una respuesta confirmando que debe tener un content typ

![[Pasted image 20260122083554.png]]

![[Pasted image 20260122084331.png]]

### Using Intruder to find hidden endpoints

Una vez identificados algunos puntos finales de API iniciales, puede usar Intruder para descubrir puntos finales ocultos. Por ejemplo, considere un escenario en el que ha identificado el siguiente punto final de API para actualizar la información del usuario:

`PUT /api/user/update`

Para identificar puntos finales ocultos, podría usar Burp Intruder para encontrar otros recursos con la misma estructura. Por ejemplo, podría agregar una carga útil a la `/update`posición de la ruta con una lista de otras funciones comunes, como `delete`y `add`.

Al buscar endpoints ocultos, utilice listas de palabras basadas en convenciones de nomenclatura de API comunes y términos del sector. Asegúrese de incluir también términos relevantes para la aplicación, según su reconocimiento inicial.

-  `Burp Intruder` enables you to automatically discover hidden parameters, using a wordlist of common parameter names to replace existing parameters or add new parameters. Make sure you also include names that are relevant to the application, based on your initial recon.
- The  `Param miner BApp ` enables you to automatically guess up to 65,536 param names per request. Param miner automatically guesses names that are relevant to the application, based on information taken from the scope.
- The Content discovery tool enables you to discover content that isn't linked from visible content that you can browse to, including parameters.
## Identifying hidden parameters

Dado que la asignación masiva crea parámetros a partir de campos de objetos, a menudo puedes identificar estos parámetros ocultos examinando manualmente los objetos devueltos por la API.

Por ejemplo, considere una `PATCH /api/users/`solicitud que permite a los usuarios actualizar su nombre de usuario y correo electrónico, e incluye el siguiente JSON:

`{ "username": "wiener", "email": "wiener@example.com", }`

Una `GET /api/users/123` solicitud concurrente devuelve el siguiente JSON:

`{ "id": 123, "name": "John Doe", "email": "john@example.com", "isAdmin": "false" }`

Esto puede indicar que los parámetros ocultos `id`están `isAdmin`vinculados al objeto de usuario interno, junto con los parámetros de nombre de usuario y correo electrónico actualizados.

## Prueba de vulnerabilidades de asignación masiva

Para probar si puede modificar el `isAdmin`valor del parámetro enumerado, agréguelo a la `PATCH`solicitud:

`{ "username": "wiener", "email": "wiener@example.com", "isAdmin": false, }`

Además, envíe una `PATCH`solicitud con un `isAdmin`valor de parámetro no válido:

`{ "username": "wiener", "email": "wiener@example.com", "isAdmin": "foo", }`

Si la aplicación se comporta de forma diferente, esto podría indicar que el valor no válido afecta la lógica de la consulta, pero el valor válido no. Esto podría indicar que el usuario puede actualizar el parámetro correctamente.

Luego puede enviar una `PATCH`solicitud con el `isAdmin`valor del parámetro establecido en `true`, para intentar explotar la vulnerabilidad:

`{ "username": "wiener", "email": "wiener@example.com", "isAdmin": true, }`

Si el `isAdmin`valor de la solicitud está vinculado al objeto de usuario sin la validación y el saneamiento adecuados, `wiener`es posible que se le otorguen privilegios de administrador incorrectamente. Para determinar si este es el caso, explore la aplicación para `wiener`comprobar si puede acceder a la función de administrador.

### Testing for server-side parameter pollution in the query string

Para probar la contaminación de parámetros del lado del servidor en la cadena de consulta, coloque caracteres de sintaxis de consulta como `#`, `&`, y `=`en su entrada y observe cómo responde la aplicación.

Considere una aplicación vulnerable que le permite buscar a otros usuarios basándose en su nombre de usuario. Al buscar un usuario, su navegador realiza la siguiente solicitud:

`GET /userSearch?name=peter&back=/home`

Para recuperar información del usuario, el servidor consulta una API interna con la siguiente solicitud:

`GET /users/search?name=peter&publicProfile=true`

## Truncating query strings

You can use a URL-encoded `#` character to attempt to truncate the server-side request. To help you interpret the response, you could also add a string after the `#` character.

For example, you could modify the query string to the following:

`GET /userSearch?name=peter%23foo&back=/home`

The front-end will try to access the following URL:

`GET /users/search?name=peter#foo&publicProfile=true`

#### Note

%% It's essential that you URL-encode the `#` character. Otherwise the front-end application will interpret it as a fragment identifier and it won't be passed to the internal API.

Review the response for clues about whether the query has been truncated. For example, if the response returns the user `peter`, the server-side query may have been truncated. If an `Invalid name` error message is returned, the application may have treated `foo` as part of the username. This suggests that the server-side request may not have been truncated.

If you're able to truncate the server-side request, this removes the requirement for the `publicProfile` field to be set to true. You may be able to exploit this to return non-public user profiles. %%



## Injecting invalid parameters

You can use an URL-encoded `&` character to attempt to add a second parameter to the server-side request.

For example, you could modify the query string to the following:

`GET /userSearch?name=peter%26foo=xyz&back=/home`

This results in the following server-side request to the internal API:

`GET /users/search?name=peter&foo=xyz&publicProfile=true`

Review the response for clues about how the additional parameter is parsed. For example, if the response is unchanged this may indicate that the parameter was successfully injected but ignored by the application.

To build up a more complete picture, you'll need to test further.

## Injecting valid parameters

If you're able to modify the query string, you can then attempt to add a second valid parameter to the server-side request.

#### Related pages

For information on how to identify parameters that you can inject into the query string, see the Finding hidden parameters section.

For example, if you've identified the `email` parameter, you could add it to the query string as follows:

`GET /userSearch?name=peter%26email=foo&back=/home`

This results in the following server-side request to the internal API:

`GET /users/search?name=peter&email=foo&publicProfile=true`

Review the response for clues about how the additional parameter is parsed.

## Anulación de parámetros existentes

Para confirmar si la aplicación es vulnerable a la contaminación de parámetros del servidor, puede intentar sobrescribir el parámetro original. Para ello, inyecte un segundo parámetro con el mismo nombre.

Por ejemplo, podría modificar la cadena de consulta de la siguiente manera:

`GET /userSearch?name=peter%26name=carlos&back=/home`

Esto da como resultado la siguiente solicitud del lado del servidor a la API interna:

`GET /users/search?name=peter&name=carlos&publicProfile=true`

La API interna interpreta dos `name`parámetros. Su impacto depende de cómo la aplicación procese el segundo parámetro. Esto varía según la tecnología web. Por ejemplo:

- PHP solo analiza el último parámetro. Esto generaría una búsqueda del usuario `carlos`.
- ASP.NET combina ambos parámetros. Esto provocaría una búsqueda del usuario `peter,carlos`, lo que podría generar un `Invalid username`mensaje de error.
- Node.js/express analiza solo el primer parámetro. Esto generaría una búsqueda del usuario `peter`, con un resultado sin cambios.

Si logras anular el parámetro original, podrías usar un exploit. Por ejemplo, podrías agregarlo `name=administrator`a la solicitud. Esto podría permitirte iniciar sesión como administrador.



![[Pasted image 20260123082215.png]]

![[Pasted image 20260123082259.png]]

![[Pasted image 20260123082508.png]]

![[Pasted image 20260123082807.png]]

![[Pasted image 20260123083351.png]]

![[Pasted image 20260123083702.png]]

Lista para ver variables validas con fuerza bruta [[Server-side variable names]]

![[Pasted image 20260123085311.png]]

![[Pasted image 20260123085732.png]]


![[Pasted image 20260123085701.png]]

![[Pasted image 20260123085913.png]]

## Testing for server-side parameter pollution in REST paths

Una API RESTful puede colocar los nombres y valores de los parámetros en la ruta URL, en lugar de en la cadena de consulta. Por ejemplo, considere la siguiente ruta:

`/api/users/123`

La ruta URL se puede desglosar de la siguiente manera:

- `/api` es el punto final de la API raíz.
- `/users` representa un recurso, en este caso `users`.
- `/123` representa un parámetro, aquí un identificador para el usuario específico.

Considere una aplicación que permite editar perfiles de usuario según su nombre de usuario. Las solicitudes se envían al siguiente punto final:

`GET /edit_profile.php?name=peter`

Esto da como resultado la siguiente solicitud del lado del servidor:

`GET /api/private/users/peter`

Un atacante podría manipular los parámetros de la ruta URL del servidor para explotar la API. Para comprobar esta vulnerabilidad, añada secuencias de recorrido de ruta para modificar los parámetros y observar cómo responde la aplicación.

Podrías enviar la URL codificada `peter/../admin` como valor del `name` parámetro:

`GET /edit_profile.php?name=peter%2f..%2fadmin`

Esto puede generar la siguiente solicitud del lado del servidor:

`GET /api/private/users/peter/../admin`

Si el cliente del lado del servidor o la API de back-end normalizan esta ruta, se puede resolver como `/api/private/users/admin`.

## Testing for server-side parameter pollution in structured data formats

Un atacante podría manipular parámetros para explotar vulnerabilidades en el procesamiento del servidor de otros formatos de datos estructurados, como JSON o XML. Para comprobarlo, inyecte datos estructurados inesperados en las entradas del usuario y observe cómo responde el servidor.

Considere una aplicación que permite a los usuarios editar su perfil y luego aplica los cambios mediante una solicitud a una API del servidor. Al editar su nombre, su navegador realiza la siguiente solicitud:

``` javascript
POST /myaccount 
name=peter`
```

Esto da como resultado la siguiente solicitud del lado del servidor:

```
PATCH /users/7312/update 
{"name":"peter"}`
```

Puede intentar agregar el `access_level`parámetro a la solicitud de la siguiente manera:
```
POST /myaccount name=peter",
"access_level":"administrator
```

Si la entrada del usuario se agrega a los datos JSON del lado del servidor sin una validación o desinfección adecuada, esto da como resultado la siguiente solicitud del lado del servidor:

```
PATCH /users/7312/update 
{name="peter","access_level":"administrator"}
```

Esto puede resultar en que al usuario `peter`se le otorgue acceso de administrador.

#### Páginas relacionadas

Para obtener información sobre cómo identificar los parámetros que puede inyectar en la cadena de consulta, consulte la sección Encontrar parámetros ocultos.

## Testing for server-side parameter pollution in structured data formats - Continued

Consider a similar example, but where the client-side user input is in JSON data. When you edit your name, your browser makes the following request:

```
POST /myaccount 
{"name": "peter"}`
```


This results in the following server-side request:

```
PATCH /users/7312/update 
{"name":"peter"}
```

You can attempt to add the `access_level` parameter to the request as follows:

```
POST /myaccount 
{"name": "peter\",\"access_level\":\"administrator"}`
```

If the user input is decoded, then added to the server-side JSON data without adequate encoding, this results in the following server-side request:


```
PATCH /users/7312/update 
{"name":"peter","access_level":"administrator"}`
```

Again, this may result in the user `peter` being given administrator access.

Structured format injection can also occur in responses. For example, this can occur if user input is stored securely in a database, then embedded into a JSON response from a back-end API without adequate encoding. You can usually detect and exploit structured format injection in responses in the same way you can in requests.

```
Note

This example below is in JSON, but server-side parameter pollution can occur in any structured data format. For an example in XML, see the XInclude attacks section in the XML external entity (XXE) injection topic.
```
