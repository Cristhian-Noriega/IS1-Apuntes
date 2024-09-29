
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

> Esta info que me estas devolviendo, se que eso no cambia durante un largo tiempo.
>  El browser no vuelve a hacer una petición al server. 
>  El que cachea es un nodo intermedio en la comunicacion.

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

Least Privilege: Tener el menor privilegio requerido para hacer las acciones. Fail-Safe 

Defaults: Por defecto no tener acceso a los recursos 

Complete Mediation: El sistema debe validar los permisos de acceso a todos los recursos 

Keep it Simple Https Password Hashes: (PBKDF2, bcrypt, y scrypt) 

Never expose information on URLs: Usernames, passwords, session tokens, y API keys deberían no aparecer en la URL para evitar ser logueadas en los logs de web server logs 

Considerar agregar Timestamp en los requests. 

Validación de los parámetros de entrada


**Monitorear transacciones sospechosas.**

Cantidad de requests por IP o por token/JWT/user para evitar problemas de denegación de servicio, o simplemente controlar o reducir el uso excesivo que puede bajar la performance de la API en general.

Limitación de velocidad, o tiempos de demora agregados entre request y request para ciertos casos, ayuda a reducir las solicitudes excesivas que ralentizarían la API, ayuda a lidiar con llamadas / ejecuciones accidentales y monitorea e identifica de manera proactiva una posible actividad maliciosa.

APIs pagas como las de google por ejemplo permiten configurar límites de uso, tarifa, para  evitar sorpresas ante un mal uso o bug que genere por error multiples llamadas a la API.

## Authentication y Authorization

* Basic auth 
* Api Keys 
* Bearer Authentication 
* OAuth 
* JWT

### Authentication vs Authorization

- **Authentication**:
    
    - Se refiere al proceso de verificar la identidad de un usuario o sistema.
    - Es el paso en el que el usuario demuestra quién es, por ejemplo, proporcionando credenciales como nombre de usuario y contraseña, tokens de acceso, o datos biométricos.
    - Responde a la pregunta: _"¿Quién eres?"_
    - Ejemplo: Un usuario inicia sesión con su correo electrónico y contraseña en una aplicación.
      
- **Authorization**:
    
    - Es el proceso de determinar qué acciones o recursos está permitido que un usuario autenticado acceda o realice.
    - Ocurre después de la autenticación y define los permisos y niveles de acceso del usuario.
    - Responde a la pregunta: _"¿Qué puedes hacer?"_
    - Ejemplo: Un usuario autenticado puede acceder a ciertas páginas, pero no a la sección de administración de un sitio.
  
  
### Basic Auth

Basic Auth encodea en base 64 el usuario concatenado con : con la password

Base64 encoding 
	user: fiuba 
	pass: k@X4R$KFEbCn 
	plain-auth: fiuba:k@X4R$KFEbCn 
	Authorization: Zml1YmE6a0BYNFIkS0ZFYkNu

Lo manda en un header llamado Authorization encodeado en base 64. 

Base64 es fácilmente decodificable, Basic authentication solo debería usarse en conjunto con otro mecanismo de seguridad como HTTPS/SSL

La autenticación básica HTTP es un simple mecanismo de desafío y respuesta con el que un servidor puede solicitar información de autenticación (un ID de usuario y una contraseña) de un cliente. El cliente pasa la información de autenticación al servidor en una cabecera de autorización. La información de autenticación está en codificación base-64 .

![](Attachments/Pasted%20image%2020240929101103.png)

> **Warning:** The "Basic" authentication scheme used in the diagram above sends the credentials encoded but not encrypted. This would be completely insecure unless the exchange was over a secure connection (HTTPS/TLS).

#### Funcionamiento de Basic Auth

1. El cliente envía una solicitud HTTP al servidor.
2. En el encabezado `Authorization`, el cliente incluye el texto `"Basic "` seguido de la codificación Base64 de las credenciales, que tienen el formato `username:password`.
    - Ejemplo: `Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=`
3. El servidor descodifica el valor Base64 y verifica las credenciales.
4. Si las credenciales son correctas, el servidor concede acceso al recurso solicitado; de lo contrario, responde con un error (normalmente, un código de estado `401 Unauthorized`).

### API Key

Algunas APIs usan API keys para autorización. Una API key es un **token** que el cliente provee cuando hace la llamada

API keys se supone que es secreta y que solo el cliente y servidor la conocen. Sólo debería usarse en conjunto con otro mecanismo de seguridad como HTTPS/SSL.

Una **API Key** es una cadena única generada por un servidor para identificar y autenticar a un cliente que está solicitando acceso a una API. Funciona como una clave de acceso que permite a la API saber quién está haciendo la solicitud.

Formas de pasarlas: 

Via queryString: 
`GET /something?api_key=abcdef12345`

Como header:
`GET /something HTTP/1.1`
`X-API-Key: abcdef12345`

Como cookie:
`GET /something HTTP/1.1`
`Cookie: X-API-KEY=abcdef12345`

#### Funcionamiento de API Key

1. **Generación**: Un cliente (usuario o aplicación) se registra en el servidor o servicio, y este genera una API Key única para ese cliente.
2. **Envío**: El cliente envía la API Key en cada solicitud HTTP para autenticar su identidad. Normalmente, la API Key se envía en:
    - Un encabezado HTTP, por ejemplo: `Authorization: ApiKey <tu-api-key>`.
    - En la URL como parámetro, por ejemplo: `https://api.example.com/data?api_key=<tu-api-key>`.
3. **Verificación**: El servidor recibe la API Key, la compara con las claves registradas y, si es válida, permite el acceso al recurso solicitado.
4. **Acceso**: Dependiendo de cómo esté configurado, la API Key puede tener permisos para ciertos endpoints, como solo lectura o acceso limitado a recursos específicos.


### Bearer Authentication / Token Auth

Utiliza tokens de seguridad llamados Bearer (da acceso al portador del token) Se envía en un header de Authorization

**Bearer Authentication** es un mecanismo de autenticación que utiliza **tokens** para acceder a recursos protegidos en aplicaciones web o APIs. El término "Bearer" se refiere al hecho de que quien posee (o lleva) el token tiene permiso para realizar solicitudes a la API.

1. **Generación de Token**: El usuario o cliente realiza un proceso de autenticación (como iniciar sesión), y el servidor emite un **token** de acceso (generalmente un JWT o similar).
2. **Envío del Token**: El cliente incluye el token en cada solicitud HTTP para acceder a los recursos protegidos. El token se envía en el encabezado HTTP `Authorization` con el formato:
    - `Authorization: Bearer <token>`
3. **Verificación del Token**: El servidor recibe la solicitud y verifica el token. Si es válido (firmado correctamente y no expirado), el servidor concede acceso al recurso.
4. **Acceso**: El servidor procesa la solicitud y responde con los datos solicitados si el token es válido, o devuelve un error (como `401 Unauthorized` o `403 Forbidden`) si no lo es.

### OAuth

* Es un protocolo de autenticación 
* Consiste en delegar la autenticación de usuario al servicio que gestiona las cuentas, de modo que sea éste quien otorgue el acceso para las aplicaciones de terceros. 
* OAuth 2 provee un flujo de autorización para aplicaciones web, aplicaciones móviles e incluso programas de escritorio.

**OAuth** (Open Authorization) es un estándar de autorización ampliamente utilizado para permitir que las aplicaciones accedan a recursos de un usuario en otro servicio, **sin necesidad de compartir las credenciales** del usuario (como su contraseña)


![](Attachments/Pasted%20image%2020240916202243.png)


Google podría ser un OAuth Server. Un tercero para que realice la autenticacion de mis usuarios.

Mi servicio es Siebel en la foto. 

> Se usa para que nuestro sistema no se encargue de la autenticacion. Se lo delegamos a un tercero. Luego cuando un cliente cuando ya tiene su access token quiere acceder a un recurso, nuestro sistema le pregunta al OAuth service si ese access token es valido.
#### Funcionamiento de OAuth:

OAuth define un proceso para delegar permisos, donde una aplicación (el "cliente") solicita acceso a un recurso protegido en nombre de un usuario. En lugar de proporcionar su contraseña, el usuario otorga acceso a la aplicación utilizando **tokens**.

#### Componentes clave:

1. **Resource Owner**: El usuario que posee los datos o recursos protegidos.
2. **Client**: La aplicación que solicita acceso a los recursos del usuario.
3. **Authorization Server**: El servidor que autentica al usuario y emite los tokens de acceso.
4. **Resource Server**: El servidor que tiene los recursos del usuario y valida los tokens antes de permitir el acceso.

#### Flujo típico de OAuth 2.0:

1. **Autorización del usuario**:
    - El cliente redirige al usuario al **Authorization Server** (como Google, Facebook, etc.) para que autorice el acceso. El usuario inicia sesión y permite que la aplicación acceda a ciertos recursos (por ejemplo, contactos, fotos).
2. **Generación de tokens**:
    - El Authorization Server genera un **token de acceso** (y a veces un **token de refresco**) y lo envía al cliente.
3. **Acceso a recursos**:
    - El cliente utiliza el **token de acceso** para hacer solicitudes al **Resource Server** en nombre del usuario. Este token es una prueba de que la aplicación ha sido autorizada.
    - Ejemplo: `Authorization: Bearer <access-token>`
4. **Renovación del token** (opcional):
    - Si el token de acceso expira, el cliente puede usar el **token de refresco** para obtener uno nuevo sin necesidad de que el usuario vuelva a autenticarse.

## JWT 

> En los anteriores metodos de autenticacion, en cada petición se verifica en el server por cada peticion para ver si estas logeado y sos quien debes ser. Costoso.

En este caso en el primer paso se genera el token también.

Pero devuelve un token que esta compuesto por un json con una estructura con información adentro que esta firmado con una clave privada que solo el servidor conoce. 

Hay una manera para saber que el usuario firmo el token y que nadie lo modifico. 

* Las credenciales del usuario viajan solo 1 vez
* El token **no** se almacena del lado del servidor para validar. Lo guarda el cliente. 
* El uso de JWT incrementa la eficiencia en las aplicaciones evitando hace multiples llamadas a la base de datos.

> La parte inicial es la misma que los anteriores, hay que identificarse de alguna manera (usario y password). Eso va a la base, chequea que existis y te valida. 
> Pero ahora en vez de darte un token random que no dice nada, da un token random con información.

Contenido del token: 

![](Attachments/Pasted%20image%2020240916205234.png)

* Header: Contiene el tipo de token a utilizar y el algoritmo de firma a usar.
* Payload: Contiene la información que se quiere transmitir, como los claims (datos sobre el usuario) y cualquier otra información adicional. Los claims están codificados en JSON.
* Signature: Se crea mendiante la combinacion del header + la carga util (codificados en base 64) + una clave secreta. Se usa para verificar que el token no ha sido alterado durante su transferencia.


Los JWT pueden ser mensajes solo firmados, solo encriptados, o ambos Si un token es solo firmado pero no encriptado, cualquiera puede leer su contenido, pero si no se conoce la clave privada no puede modificado. Ya que al validar la firma no coincidiría.

![](Attachments/Pasted%20image%2020240916210805.png)

El server valida la firma del JWT para saber que lo que él le envió al cliente no se modificó, y utiliza la información del mismo. De esta forma se sigue siendo stateless, y generalmente se hace más eficiente la validación del usuario, sin tener que acceder a un medio persistente a validar si el token es válido y a quién pertenece el mismo.

![](Attachments/Pasted%20image%2020240916210835.png)

Problema de lo JWT tokens -> tiempo de vida.

### Características de JWT:

- **Stateless (sin estado)**: JWT es **autocontenido**, lo que significa que el servidor no necesita almacenar información de sesión en la base de datos o en la memoria. Toda la información necesaria está dentro del token. -> **VENTAJA**
- **Seguridad**: Aunque el **payload** está codificado en Base64URL, no está cifrado, por lo que **no debe incluirse información sensible** como contraseñas. La seguridad se basa en la firma que garantiza la integridad, no en la confidencialidad. -> **VENTAJA**
- **Expiración**: Los JWT suelen tener un campo de expiración (`exp`) que define cuándo el token ya no es válido, lo que mejora la seguridad al limitar la ventana de uso. -> **DESVENTAJA**

> La diferencia con otros tokens, es que el servidor para chequear que el token es valido y de quien pertenece  tiene que hacer una query a la base de datos.
> El mecanismo de JWT lo valida en memoria, porque sabe si es el quien lo firmo. 

#### Diferencias

##### **Token Tradicion**:

- **Almacenamiento en el servidor**: Los tokens tradicionales son **simples identificadores** que el servidor emite después de autenticar a un usuario. Este identificador se asocia con una sesión en el servidor, y el servidor mantiene el estado y la información completa de la sesión en su base de datos o memoria.
- **Verificación**: Cada vez que el cliente realiza una solicitud, envía el token. El servidor usa este token para buscar la sesión correspondiente y verificar la autenticación y los permisos del usuario.

##### **JWT (JSON Web Token)**:

- **Autocontenido (stateless)**: Los tokens JWT contienen toda la información necesaria para autenticar y autorizar al usuario dentro del propio token, como el ID de usuario, roles, y permisos. No es necesario que el servidor mantenga un estado o sesión vinculada a este token, ya que todo está codificado en el token.
- **Verificación**: Al recibir el token, el servidor solo tiene que verificar la firma del JWT para confirmar su autenticidad y validez, sin necesidad de buscar información adicional en una base de datos.

## Refresh tokens

Los access token deberían tener un tiempo limitado de vida, por eso aparecen los refresh token. Este es otro token que sirve para un solo uso y es utilizado para obtener un nuevo access token. Es una credencial que permite obtener nuevos tokens sin necesidad de usar las credenciales de usuario y password nuevamente.

> Viene a solucionar el problema de la expiracion y revocacion. 

Un **Refresh Token** es un token adicional que se emite junto con el **Access Token** (por ejemplo, un JWT). A diferencia del Access Token, el Refresh Token tiene un tiempo de vida mucho más largo y se utiliza exclusivamente para obtener nuevos Access Tokens sin requerir que el usuario vuelva a autenticarse.

- **Access Token**: Es el token que se envía junto con las solicitudes al servidor para acceder a recursos protegidos. Tiene un tiempo de vida relativamente corto (por ejemplo, 15 minutos o 1 hora) y contiene la información de autenticación del usuario.
- **Refresh Token**: Se utiliza para obtener un nuevo Access Token cuando este expira. No se envía en cada solicitud; solo se usa cuando el Access Token ya no es válido.

#### **Ventajas de usar Refresh Token**:

1. **Sesiones más largas sin comprometer la seguridad**:
    
    - **Access Token de corta duración**: Al usar tokens de corta duración, se reduce el riesgo de que un atacante use un Access Token comprometido por mucho tiempo.
    - **Refresh Token para renovar Access Tokens**: El Refresh Token permite renovar el Access Token automáticamente, manteniendo la experiencia del usuario fluida sin tener que solicitar constantemente las credenciales.
      
2. **Revocación más fácil**:
    
    - Si un Access Token se compromete, no hay tanto riesgo porque tiene un tiempo de vida corto. Si un Refresh Token se compromete, el servidor puede invalidarlo o revocarlo de forma centralizada.
    - Esto es particularmente útil en aplicaciones donde es más difícil implementar la revocación de Access Tokens, como ocurre con **JWT**, ya que el servidor puede invalidar el Refresh Token y, por lo tanto, evitar que se emitan nuevos Access Tokens.


## Respuestas 
- Mantener lo más estandarizadas a las mismas. 
- Reducir el tamaño de la respuesta a lo necesario 
- Utilizar Código de Errores HTTP


## Versionado

Rest no provee un mecanismo definido para versionado pero se suelen ver estas estrategias.
El versionado en APIs REST es esencial para gestionar cambios sin romper la compatibilidad con versiones anteriores.

- Usando la URI: 
  \https://api.fi.uba.ar/v1 
  \https://apiv1.fi.uba.ar 
  \https://api.fi.uba.ar/20211101/ 
- Usando un Custom Header:
   Accept-version: v1 
   Accept-version: v2 
- Usando el Header Accept:
   Accept: application/vnd.example.v1+json 
   Accept: application/vnd.example+json;version=1.0


## Paginado, filtros y ordenamientos

Puede ir metadata en la respuesta:

```bash
{
	"success": true,
	"metadata": {
		"page": 5,
		"per_page": 20,
		"page_count": 20,
		"total_count": 521,
	}
	"data": {...}
}
```

O puede ir en los headers de la respuesta:

HTTP/1.1 200 
Pagination-Count: 100 
Pagination-Page: 5 
Pagination-Limit: 20 
Content-Type: application/json

Filtros y ordenamiento suele usarse como parámetros del query string o del body

Los **filtros** permiten a los clientes obtener solo los datos que son relevantes para ellos. Normalmente, se implementan como **parámetros de consulta** (query string) en la URL o dentro del cuerpo de la solicitud en algunas APIs más avanzadas.

Este es el enfoque más común: los filtros se pasan en el query string como pares clave-valor.

```bash
GET /api/usuarios?role=admin&status=active
```

Esto podría devolver solo usuarios que sean administradores y que tengan un estado "activo".


En algunos casos, las APIs permiten enviar filtros como parte del cuerpo de la solicitud, especialmente en solicitudes POST que involucran búsquedas complejas.


```json
{
  "filters": {
    "role": "admin",
    "status": "active"
  }
}
```

**Desventajas**:

- No sigue el estilo de RESTful tan estrictamente, ya que en una solicitud GET, idealmente el cuerpo no debería ser usado.
- Menos intuitivo que los query strings.
  
Que pasa con los formatos, booleanos, fechas? Con la notación: camelCase o snake_case?

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


