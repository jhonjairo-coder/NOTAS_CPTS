La autorización a nivel de objeto es un mecanismo de control de acceso que generalmente se implementa a nivel de código para validar que un usuario solo pueda acceder a los objetos a los que debería tener permisos de acceso.
Cada punto final de API que recibe el ID de un objeto y realiza alguna acción en él debe implementar comprobaciones de autorización a nivel de objeto. Estas comprobaciones deben validar que el usuario conectado tenga permisos para realizar la acción solicitada en el objeto.

## Ejemplos de escenarios de ataque

### Escenario n.° 1

Una plataforma de comercio electrónico para tiendas online proporciona una página de listado con los gráficos de ingresos de sus tiendas alojadas. Al inspeccionar las solicitudes del navegador, un atacante puede identificar los endpoint de la API utilizados como fuente de datos para dichos gráficos y su patrón: `/shops/{shopName}/revenue_data.json`. Mediante otro punto final de la API, el atacante puede obtener la lista de todos los nombres de las tiendas alojadas. Con un simple script para manipular los nombres de la lista, reemplazando " `{shopName}`en la URL", el atacante obtiene acceso a los datos de ventas de miles de tiendas de comercio electrónico.

### Escenario #2

Un fabricante de automóviles ha habilitado el control remoto de sus vehículos mediante una API móvil para la comunicación con el teléfono móvil del conductor. La API permite al conductor arrancar y parar el motor, así como bloquear y desbloquear las puertas, de forma remota. Como parte de este proceso, el usuario envía el Número de Identificación Vehicular (VIN) a la API. La API no valida que el VIN corresponda a un vehículo del usuario conectado, lo que genera una vulnerabilidad BOLA. Un atacante puede acceder a vehículos que no le pertenecen.

### Escenario n.° 3

Un servicio de almacenamiento de documentos en línea permite a los usuarios ver, editar, almacenar y eliminar sus documentos. Cuando se elimina un documento de un usuario, se envía a la API una mutación de GraphQL con el ID del documento.

```
POST /graphql
{
  "operationName":"deleteReports",
  "variables":{
    "reportKeys":["<DOCUMENT_ID>"]
  },
  "query":"mutation deleteReports($siteId: ID!, $reportKeys: [String]!) {
    {
      deleteReports(reportKeys: $reportKeys)
    }
  }"
}
```

Dado que el documento con la ID dada se elimina sin más comprobaciones de permisos, un usuario podrá eliminar el documento de otro usuario.

## Cómo prevenir

- Implementar un mecanismo de autorización adecuado que se base en las políticas y la jerarquía del usuario.
- Utilice el mecanismo de autorización para verificar si el usuario que inició sesión tiene acceso para realizar la acción solicitada en el registro en cada función que utiliza una entrada del cliente para acceder a un registro en la base de datos.
- Prefiera el uso de valores aleatorios e impredecibles como GUID para los ID de registros.
- Escriba pruebas para evaluar la vulnerabilidad del mecanismo de autorización. No implemente cambios que hagan que las pruebas fallen.

## Escenario 1 – BOLA en crAPI 

La API **confía en el ID del objeto que envía el cliente**, pero **no valida si ese objeto pertenece al usuario autenticado**.

---

### Flujo típico del escenario

1. **Usuario A** se autentica correctamente (JWT válido).
    
2. La app hace una request del tipo:
    
    `GET /identity/api/v2/vehicle/77ce1c61-187f-4284-8fd1-980ed9245f9b/location`
![[Pasted image 20260203121510.png]]
    
3. El backend:
    
    - ✔️ Valida el token
        
    - ❌ **NO valida que el vehículo 123 sea del Usuario A**


4. El atacante identifica el IDs de los de mas usuarios
    
    `GET /community/api/v2/community/posts/recent?limit=30&offset=0`
![[Pasted image 20260203121314.png]]
    
4. El atacante realiza cambio del  objeto en la petición y recibe la información del otro usuario.
    ![[Pasted image 20260203121621.png]]


## Ejercicio 2 - Access mechanic reports of other users

crAPI allows vehicle owners to contact their mechanics by submitting a "contact mechanic" form. This challenge is about accessing mechanic reports that were submitted by other users.

- Analyze the report submission process
    
- Find an hidden API endpoint that exposes details of a mechanic report
    
- Change the report ID to access other reports

After adding a vehicle, i have an option to send the service request to mechanic by using the “Contact Mechanic” page, which presented me with a form to fill out. After completing the form, i received a response through Burp Suite and discovered a link that is report_link and changing the value of the report_id in the request i gain access to a mechanic report.

### Flujo típico del escenario

1. Se autentica el usuario
2. Se envía una solicitud para un requerimiento 

![[Pasted image 20260203142145.png]]

2.  Se intercepta la petición `POST /workshop/api/merchant/contact_mechanic`
![[Pasted image 20260203142436.png]]

3. Se captura la petición del generada para el reporte `workshop/api/mechanic/mechanic_report?report_id=3 `  Se cambia el ID u se visualiza que tenemos acceso la información de otro usuario.


![[Pasted image 20260203142656.png]]

