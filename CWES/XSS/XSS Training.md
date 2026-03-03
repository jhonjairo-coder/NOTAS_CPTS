# Hello, Reflected XSS

Inyeción simple de una etiqueta HTML que ejecute JavaScript. La carga útil más simple

`<script>alert("XSS")</script>`

### Por qué funcionó
Tu entrada se insertó directamente en la respuesta HTML sin codificación ni filtrado. El navegador analizó tu `<script>`etiqueta (o cualquier HTML que inyectaste) como parte de la estructura de la página y la ejecutó.

### Lección clave
Esta es la forma más básica de XSS. Siempre que la entrada del usuario se refleja en HTML sin codificar, un atacante puede inyectar marcado arbitrario. La solución es sencilla: **codificar en HTML** toda la salida del usuario ( `<`→ `&lt;`, `>`→ `&gt;`, etc.).

### Aplicación en el mundo real
Las páginas de búsqueda, los mensajes de error que reflejan parámetros de URL y las páginas 404 que muestran la ruta solicitada son objetivos clásicos del XSS reflejado.

## Stored XSS — Persistent Injection

### Por qué funcionó

Su carga útil se almacenó en la base de datos del servidor (matriz en memoria) y se renderizó cada vez que un usuario visita la página. A diferencia del XSS reflejado, el atacante no necesita que la víctima haga clic en un enlace creado.

### Lección clave

El XSS almacenado es más peligroso que el reflejado, ya que afecta automáticamente **a todos los visitantes** . La carga útil persiste y puede robar sesiones, desfigurar páginas o propagarse como un gusano. Siempre desinfecte tanto la entrada como la salida.

### Aplicación en el mundo real

Secciones de comentarios, publicaciones en foros, perfiles de usuario, aplicaciones de chat y cualquier función donde se guarde el contenido del usuario y se muestre a otros.

![[Pasted image 20260225131507.png]]

![[Pasted image 20260225131526.png]]

## Script Tag Filter Bypass

La etiqueta <script> está bloqueada, pero muchos otros elementos HTML pueden ejecutar JavaScript. Pruebe con <img src=x onerror=alert("XSS")> o <svg onload=alert("XSS")>.

### Por qué funcionó

El filtro solo bloqueaba `<script>`etiquetas, pero muchos otros elementos HTML pueden ejecutar JavaScript mediante controladores de eventos. Elementos como `<img>`, `<svg>`, `<body>`, `<input>`, `<details>`, y muchos más admiten `on*`atributos de evento.

### Lección clave

El filtrado basado en listas de bloqueo (que bloquea etiquetas específicas) presenta fallas fundamentales. Hay demasiados vectores para bloquearlos todos. El enfoque correcto se **basa en listas de permitidos** : permitir únicamente etiquetas y atributos seguros o usar codificación de salida contextual.

### Aplicación en el mundo real

Muchos WAF y filtros personalizados solo bloquean `<script>`. En las recompensas por errores, prueba siempre etiquetas alternativas: `<img>`, `<svg>`, `<math>`, `<iframe>`, `<object>`, `<embed>`, `<video>`, `<audio>`, `<marquee>`, `<details>`.


## Script Tag Filter Bypass


