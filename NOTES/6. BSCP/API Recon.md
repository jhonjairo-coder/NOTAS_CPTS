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

![[Pasted image 20260122081250.png]]