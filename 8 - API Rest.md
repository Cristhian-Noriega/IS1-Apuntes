
## ¿ Qué NO es REST ?

* NO es un standard
* NO es un protocolo
* NO es un reemplazo de SOAP
* NO es una biblioteca

## ¿ Qué es REST ? 

* Surge de la tesis doctoral de Roy Fielding en el año 2000 
* Significa Representational State Transfer 
* Utiliza estándares existentes como HTTP 
* Comunicación cliente - servidor 
* Se presenta en 3 niveles de madurez -> Nivel 0 (SOAP previo a REST) , Nivel 1 (recursos en un endpoint), Nivel 2 (GET, POST, PUT, La respuesta nos permite navegar a otro recurso)

## Caracteristicas

* Arquitectura cliente - servidor 
* Stateless 
* Cacheable 
* Expone recursos (URIs) 
* Usa explícitamente los verbos HTTP 
* Navegable

## Stateless

No manejar el estado del lado del servidor. 

* Cada request se ejecuta de forma independiente del resto 
* Cada request contiene toda la información necesaria para completarse 
* La API no mantiene ningún tipo de sesión 
* Se promueve el uso de tokens para manejo de seguridad


## Cacheable

* Reduce ancho de banda usado 
* Reduce latencia 
* Reduce carga en servidores 
* Oculta fallos de red

Suelen tener headers que lo indican.

Esta info que me estas devolviendo, se que eso no cambia durante un largo tiempo. 

El browser no vuelve a hacer una petición al server.

Lo que se define como “cacheabilidad” en los sistemas REST es la capacidad de estos sistemas para etiquetar de alguna forma las respuestas para que otros mecanismos intermedios funcionen como un caché. 

Estos sistemas o mecanismos intermedios (existen entre el cliente y el servidor) deben ser por lo general transparentes para los desarrolladores, no deben afectar la manera en que los servicios se consumen.


Expires Expires: Fri, 19 Nov 2021 19:20:49 EST Cache-Control Cache-Control: max-age=3600 Last-Modified Last-Modified: Fri, 19 May 2021 09:17:49 EST



Las APIs suelen retornar representaciones en varios formatos, entre ellos formato plano, XML,
HTML, JSON y estos formatos pueden ser comprimidos para ahorrar ancho de banda sobre la red.
Accept-Encoding
Ejemplo de cómo el cliente informa que mecanismos soporta:
Accept-Encoding: gzip,compress
Content-Encoding
Ejemplo de cómo el server informa que mecanismo usó para encripción:
Content-Type: text/html
Content-Encoding: gzip


## URIs

* Uniform Resource Identifier
* Identificación unívoca de recursos con cadenas de caracteres
* Identifica los recursos por clase o tipo
* Uso de sustantivos en plural por convención. No verbos
* Distinción de recursos principales y subordinados

### Ejemplos URIs

Recurso: clientes 
* /clientes representa todos los clientes 
* /clientes/1 representa al cliente con id 1 
* /clientes?nombre=juan representa a los clientes con nombre juan 
* /clientes/1/compras representa a las compras del cliente 1

y si las compras son recursos primarios…?

Recurso: compras 
* /compras 
* /compras?cliente=1

## Verbos HTTP - Requests

* GET: solicita una representación de un recurso específico 
* POST: se utiliza para enviar una entidad a un recurso en específico 
* DELETE: borra un recurso en específico 
* PUT: reemplaza todas las representaciones actuales del recurso de destino con la carga útil de la petición 
* PATCH: aplica modificaciones parciales a un recurso (a diferencia de PUT) 
* OPTIONS: es utilizado para describir las opciones de comunicación para el recurso de destino

> Post es para crear, algo nuevo como respuesta. Ejemplo nueva factura para un cliente. 
> Put es mas para modificar.

## HTTP Status Codes - Responses

* 1xx: Informational 
* 2xx: Success 
* 3xx: Redirection 
* 4xx: Client Error 
* 5xx: Server Error

* 1xx: Hold on  
* 2xx: Here you go 
* 3xx: Go away 
* 4xx: You fucked up 
* 5xx: I fucked up

Ejemplos: 

200 - OK
201 - Created (con el location en el header)
400 - Bad Request
401 - Authorization Required
404 - Not Found
405 - Method Not Allowed
408 - Request Time-Out
409 - Conflict
422 - Unprocessable Entity
500 - Internal Server Error
502 - Bad Gateway
504 - Gateway Time-Out



## REST Security Design Principles

Least Privilege: Tener el menor privilegio requerido para hacer las acciones. Fail-Safe Defaults: Por defecto no tener acceso a los recursos 
Complete Mediation: El sistema debe validar los permisos de acceso a todos los recursos Keep it Simple Https Password Hashes: (PBKDF2, bcrypt, y scrypt) Never expose information on URLs: Usernames, passwords, session tokens, y API keys deberían no aparecer en la URL para evitar ser logueadas en los logs de web server logs 
Considerar agregar Timestamp en los requests. 
Validación de los parámetros de entrada


Monitorear transacciones sospechosas.
Cantidad de requests por IP o por token/JWT/user para evitar problemas de denegación de servicio,
o simplemente controlar o reducir el uso excesivo que puede bajar la performance de la API en
general.
Limitación de velocidad, o tiempos de demora agregados entre request y request para ciertos
casos, ayuda a reducir las solicitudes excesivas que ralentizarían la API, ayuda a lidiar con
llamadas / ejecuciones accidentales y monitorea e identifica de manera proactiva una posible
actividad maliciosa.
APIs pagas como las de google por ejemplo permiten configurar límites de uso, tarifa, para evitar
sorpresas ante un mal uso o bug que genere por error multiples llamadas a la API.

## Authentication y Authorization

* Basic auth 
* Api Keys 
* Bearer Authentication 
* OAuth 
* JWT

Basic Auth encodea en base 64 el usuario concatenado con : con la password

Base64 encoding 
	user: fiuba 
	pass: k@X4R$KFEbCn 
	plain-auth: fiuba:k@X4R$KFEbCn 
	Authorization: Zml1YmE6a0BYNFIkS0ZFYkNu

Base64 es fácilmente decodificable, Basic authentication solo debería usarse en conjunto con otro mecanismo de seguridad como HTTPS/SSL

## API Key

Algunas APIs usan API keys para autorización. Una API keyes un token que el cliente provee cuando hace la llamada
API keys se supone que es secreta y que solo el cliente y servidor la conocen. Sólo debería usarse en conjunto con otro mecanismo de seguridad como HTTPS/SSL.

Formas de pasarlas: 

Via queryString: 
`GET /something?api_key=abcdef12345`

Como header:
`GET /something HTTP/1.1`
`X-API-Key: abcdef12345`

Como cookie:
`GET /something HTTP/1.1`
`Cookie: X-API-KEY=abcdef12345`


## Bearer Authentication / Token Auth

Utiliza tokens de seguridad llamados Bearer (da acceso al portador del token) Se envía en un header de Authorization

## OAuth

* Es un protocolo de autenticación 
* Consiste en delegar la autenticación de usuario al servicio que gestiona las cuentas, de modo que sea éste quien otorgue el acceso para las aplicaciones de terceros. 
* OAuth 2 provee un flujo de autorización para aplicaciones web, aplicaciones móviles e incluso programas de escritorio.


![](Attachments/Pasted%20image%2020240916202243.png)


Google podría ser un OAuth Server. Un tercero para que realice la autenticacion de mis usuarios.

Mi servicio es Siebel en la foto. 

## JWT 

En las anteriores en cada petición se verifica en el server por cada API Rest para ver si estas logeado y sos quien debes ser. Costoso.

En este caso en el primer paso se genera el token tambien.

Pero devuelve un token que esta compuesto por una clave privada definida por el usuario.? 

Hay una manera para saber que el usuario firmo el token y que nadie lo modifico. 

* Las credenciales del usuario viajan solo 1 vez
* El token **no** se almacena del lado del servidor para validar. Lo guarda el cliente. 
* El uso de JWT incrementa la eficiencia en las aplicaciones evitando hace multiples llamadas a la base de datos.

Contenido del token: 

![](Attachments/Pasted%20image%2020240916205234.png)

Los JWT pueden ser mensajes solo firmados, solo encriptados, o ambos Si un token es solo firmado pero no encriptado, cualquiera puede leer su contenido, pero si no se conoce la clave privada no puede modificado. Ya que al validar la firma no coincidiría.

![](Attachments/Pasted%20image%2020240916210805.png)

El server valida la firma del JWT para saber que lo que él le envió al cliente no se modificó, y utiliza la información del mismo. De esta forma se sigue siendo stateless, y generalmente se hace más eficiente la validación del usuario, sin tener que acceder a un medio persistente a validar si el token es válido y a quién pertenece el mismo.

![](Attachments/Pasted%20image%2020240916210835.png)

Problema de lo JWT tokens -> tiempo de vida.

## Refresh tokens

Los access token deberían tener un tiempo limitado de vida, por eso aparecen los refresh token. Este es otro token que sirve para un solo uso y es utilizado para obtener un nuevo access token. Es una credencial que permite obtener nuevos tokens sin necesidad de usar las credenciales de usuario y password nuevamente.


## Respuestas 
- Mantener lo más estandarizadas a las mismas. 
- Reducir el tamaño de la respuesta a lo necesario 
- Utilizar Código de Errores HTTP


## Capa de servicios y capa de controllers 

### 1. **Capa de Controladores (Controllers)**

- **Responsabilidad**: La capa de controladores es la que se encarga de manejar las solicitudes HTTP entrantes (como `GET`, `POST`, `PUT`, `DELETE`) y devolver respuestas HTTP. Actúa como una interfaz entre el cliente (generalmente una aplicación web o móvil) y la lógica de negocio de la aplicación.
    
- **Uso en REST API**: Aquí es donde defines los **endpoints** de la API. Un controlador podría tener rutas como `/api/usuarios`, `/api/productos`, etc.
    
    En Java, usando **Spring Boot**, un controlador REST se define con la anotación `@RestController`.


### 2. **Capa de Servicios (Services)**

- **Responsabilidad**: La capa de servicios contiene la lógica de negocio de la aplicación. Aquí es donde ocurre el procesamiento de datos, las reglas de negocio, y la interacción con otras capas, como la capa de persistencia (repositorios).
    
- **Uso**: Los controladores llaman a los servicios para realizar operaciones de negocio, pero los servicios no están directamente relacionados con HTTP ni con los detalles de cómo se maneja la solicitud.

### 3. **Dónde encaja REST API**

- **En la capa de controladores** es donde se manejan todas las solicitudes relacionadas con la API REST. Los controladores son los encargados de procesar las peticiones HTTP y devolver respuestas en formato JSON o XML, según sea necesario.
- **Los servicios no están acoplados a HTTP**, por lo que podrían ser reutilizados en otros contextos (por ejemplo, en una aplicación que no sea web, sino de escritorio o microservicios).


Los **Data Transfer Objects (DTOs)** son una parte esencial de muchas aplicaciones que siguen una arquitectura basada en capas, como en Java con **Spring Boot**, y suelen ubicarse entre la **capa de controladores** y la **capa de servicios** o **lógica de negocio**. Su propósito principal es facilitar el transporte de datos entre las capas de la aplicación de una manera eficiente y segura.

### ¿Dónde encajan los DTOs?

1. **En la capa de controladores (Controllers)**:
    
    - Los DTOs se utilizan para **recibir** y **enviar** datos entre el cliente (como un frontend o una API externa) y la aplicación.
    - Los DTOs permiten que los controladores reciban datos bien estructurados, evitando exponer directamente las **entidades del dominio** (modelos persistentes) o la lógica interna de la aplicación.
    
    En el caso de recibir datos en una API, el controlador puede convertir un DTO en una entidad del dominio para pasarla al servicio.
    
2. **En la capa de servicios (Services)**:
    
    - Los servicios también pueden usar DTOs para devolver datos al controlador.
    - El servicio puede transformar las entidades del dominio en DTOs para que el controlador devuelva una representación simplificada o adaptada a las necesidades del cliente.

### ¿Por qué usar DTOs?

- **Separación de Concerns**: Los DTOs permiten una clara separación entre las entidades del dominio (que suelen ser modelos que representan la base de datos) y los datos que el cliente debe ver o enviar.
- **Seguridad**: Los DTOs ayudan a controlar qué campos se exponen al cliente, evitando exponer información sensible o interna de la aplicación.
- **Eficiencia**: Puedes crear DTOs que solo contengan los campos necesarios para una operación particular, evitando enviar datos innecesarios por la red.
- **Validación**: Los DTOs pueden incluir validaciones específicas, permitiendo asegurar que los datos que se envían a la API cumplen ciertos requisitos antes de llegar al servicio.

## Bibliografia


 Architectural Styles and the Design of Network-based Software Architectures  
 https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm 
 Wikipedia - https://es.wikipedia.org/wiki/Roy_Fielding
 http://roy.gbiv.com/ Roy Fielding personal site
 JWT: https://jwt.io/introduction/ https://jwt.io/#debugger-io 
 https://martinfowler.com/articles/richardsonMaturityModel.html
 https://hackernoon.com/restful-api-design-step-by-step-guide-2f2c9f9fcdbf
 https://www.postman.com/
