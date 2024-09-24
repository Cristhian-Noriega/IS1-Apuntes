
Dominio: App móvil de películas

## Definición de la API Rest


### Endpoints para los usuarios (Usuario Estándar y Admin)

1. **Login**
   Descripción: El login permite que el usuario inicie seccion en la app, genera un token JWT. Para ello se utiliza un `POST` del protocolo HTTP: 
   `POST /api/v1/users/token`
   Respuesta: 
   `200 OK` con tokens `JWT`, sino `401 Unauthorized` si las credenciales son incorrectas


2. **Sign Up**
   Descripción: El sign up permite crear una nueva cuenta de usuario. Se utiliza un `POST`.
   `POST /api/v1/users/register`
   Respuesta:
   `201` created , sino `400 Bad Request` si hay errores de validación.


3. **Buscar películas sin autenticacion**
   Descripción: Permite buscar películas recomendadas, con filtros (titulo, actores, categorías). Para ello se necesita obtener un recurso de nuestra api, por lo que se usa `GET`.
   `GET /api/v1/movies`
   Si se aplican filtros, se especificaran parámetros en la URL:
   `?titulo=FightClub&genero=Thriller`
   Respuesta:
   `200 OK` con la lista paginada de películas, sino `404 Not Found` si no se encuentran.


4. **Calificar una película**
   Descripción: Un usuario logueado puede calificar una película. Se usa el verbo `POST` ya que se envía una entidad a un recurso:
   `POST /api/v1/movies/{id}/rating`
   Headers:
   `Authorization: Bearer <JWT>`
   En el cuerpo del post se indicara la calificación: `{"rating": 8}`
   Respuesta: 
   `201` created, sino `400` Bad Request en caso de una calificación invalida. 


5. **Ver perfil de un usuario**
   Descripción: Permite ver el perfil de otro usuario. Se usa el verbo `GET`
   `GET /api/v1/users/{id}/profile`
   Respuesta:
   `200` OK con todos los datos del perfil del usuario.


6. **Seguir un usuario**
   Descripción: Permite que un usuario siga a otro usuario. Se usa el verbo `POST`.
   `POST /api/v1/users/`
   Respuesta:
   `200` OK, sino `404 Not Found` si el usuario no existe.


7. **Ver las películas calificadas por un usuario**
   Descripción: Muestra las películas calificadas por un usuario particular.
   `GET /api/v1/users/{id}/rated-movies`
   Ademas en Rest se especifica la paginacion en la `URI` con parámetros:
   `?&limit=10&offset=50`
   Respuesta:
   `200` OK con la lista de películas y calificaciones, sino `404 Not Found` si el usuario no ha calificado películas.


8. **Ver solicitudes de seguimiento**
   Descripción: Muestra las solicitudes de seguimiento recibidas por el usuario.
   `GET /api/v1/users/{id}/follow-requests`
   Respuesta:
   `200` OK con la lista de solicitudes pendientes del usuario.


9. **Aceptar  y rechazar una solicitud de seguimiento**
   Descripción: Acepta o rechaza una solicitud de seguimiento de un usuario.
   `POST /api/v1/users/{id}/follow-requests/{request_id}/accept`
   `POST /api/v1/users/{id}/follow-requests/{request_id}/reject`
   Respuesta:
   `200` OK, sino `404 Not Found` si no se encuentra la solicitud
   
   
10. **Ver lista de seguidores y seguidos**
    Descripción: Muestra la lista de cuentas que siguen al usuario o que el usuario sigue.
    `GET /api/v1/users/{id}/followers`
    `GET /api/v1/users/{id}/following`
    Respuesta:
    `200` OK con la lista de seguidores o seguidos


11. **Editar perfil de usuario**
	 Descripción: Permite al usuario editar su información personal.
    `PUT /api/v1/users/{id}/profile`
    Respuesta:
    `200` OK, sino `400 Bad Request` si los datos no son validos.


12. **Eliminar cuenta de usuario**
	 Descripción: Permite al usuario eliminar su cuenta de usuario.
    `DELETE /api/v1/users/{id}`
    Respuesta:
    `204 No Content`, sino `404 Not Found` si el usuario no existe.


### Endpoints para los usuarios Admin

1. **Login de admin**
   Descripción: Permite que un administrador inicie sesion
   `POST /api/v1/admin/login`
   Headers: `Authorization: Basic {base64(email:password)}`
   Respuesta:
   `200` OK con tokens JWT, sino `401 Unauthorized` si las credenciales son incorrectas

2. **Crear usuario Admin**
   Descripción: Permite crear un nuevo usuario con rol de Admin
   `POST /api/v1/admin/users`
   Respuesta:
   `201` Created, sino `400` Bad Request si hay errores de validacion

3. **Editar y eliminar usuario Admin**
   Descripcion: Permite editar o eliminar un usuario Admin
   `PUT /api/v1/admin/users/{id}`
   `DELETE /api/v1/admin/users/{id}`
   Respuesta: 
   `200` Ok, `404 Not Found` si el usuario no existe
   
4. **Crear una categoria de pelicula**
   Descripcion: Permite a un admin crear una nueva categoria de peliculas.
   `POST /api/v1/admin/categories`
   Respuesta:
   `201` Created, `400` Bad Request si hay errores en la creacion

5.  **Editar una categoria de pelicula**
	Descripcion: Permite a un admin editar una categoria existente.
	 `PUT /api/v1/admin/categories/{id}`
	 Respuesta:
	 `200` Ok, `404 Not Found` si la categoria no existe.


6.  **Editar una categoria de pelicula**
	Descripcion: Permite a un admin eliminar una categoria existente.
	 `DELETE /api/v1/admin/categories/{id}`
	 Respuesta:
	 `204` No content, `404 Not Found` si la categoria no existe.

7.  **Crear una pelicula o actor**
	Descripcion: Permite a un admin crear una nueva pelicula en la lista o un actor.
	`POST /api/v1/admin/movies` 
	`POST /api/v1/admin/actors` 
	 Respuesta:
	 `201` Created, sino`400 Bad Request`.

8.  **Editar una pelicula o actor**
	Descripcion: Permite a un admin editar los detalles de una pelicula o un actor.
	`PUT /api/v1/admin/movies{id}` 
	`PUT /api/v1/admin/actors{id}` 
	 Respuesta:
	 `200` Ok, sino`404 Not Found` si no existe.

9.  **Eliminar una pelicula o actor**
	Descripcion: Permite a un admin eliminar una pelicula en la lista o un actor.
	`DELETE /api/v1/admin/movies/{id}` 
	`DELETE /api/v1/admin/actors/{id}` 
	 Respuesta:
	 `204 No content`, sino `404 Not Found` si no existe.

   > Importante aclarar que la gestión de películas y actores por parte de los usuarios admin no modifican las calificaciones de los usuarios. Las calificaciones son gestionadas por los usuarios estándar y no son alteradas mediante los endpoints administrativos. 
   > Esto se podria lograr independizando a la *Película* de la *Calificación* mediante los endpoints mencionados, es decir, los endpoints administrativos de película y actores no incluyen ningún campo relacionado con las calificaciones, siendo estas independientes pudiéndose acceder solo con usuarios estándar.
   



---


## Modelo de Dominio Preliminar

Las entidades requeridas para representar el dominio de la aplicacion son:

* **Usuario**: Representa a cualquier tipo de usuario en la aplicacion, con atributos como email, nombre, apellido, avatar, fecha_nacimiento, género, y roles (`ADMIN` o estándar).
  Relaciones: 
	  1. Puede seguir a otros usuarios
	  2. Puede ser seguido por otros usuarios
	  3. Puede calificar varias peliculas

* **Pelicula**: Representa una pelicula con atributos como titulo, categorias, actores y calificaciones.
  Relaciones: 
	  1. Pertenece a una o mas categorias
	  2. Tiene uno o mas actores
	  3. Puede tener muchas calificaciones de distintos usuarios
	     
* **Actor**: Listado de actores con atributos como nombre, apellido y fecha de nacimiento.
  Relaciones:
	  1. Un actor pertenece a una pelicula
	     
* **Categoria**: Listado de categorias de peliculas.
  Relaciones: 
	  1. Una categoria puede contener varias peliculas
	     
* **Calificacion**: Representa la relacion entre un usuario y una pelicula atraves de la accion de califcar una pelicula de parte del usuario. 
  Relaciones: 
	  1. Pertenece a un usuario (autor)
	  2. Esta asociada a una pelicula
	     
* **Relacion de seguimiento**: Representa la relacion entre un usuario y otro usuario cuando se empiezan a seguir.


---


## Propuesta de Arquitectura preliminar

Para la arquitectura de la aplicacion, empleare una arquitectura de capas con separación de responsabilidades. La arquitectura para ello es la **Arquitectura Hexagonal**, de forma tal que se pueda separar la lógica de negocio del dominio de las implementaciones técnicas, permitiendo escalabilidad y bajo acoplamiento y alta cohesion. 

### Capa de dominio 

Para la capa de dominio,  se diseño un UML simple que represente las entidades clave de la lógica de negocio y sus interacciones en el sistema.


![](Attachments/UML%20movies%20app.jpeg)

### Capa de Aplicación 

Esta capa orquesta la interacción entre la capa de dominio y la capa de infraestructura ("el mundo exterior").
En esta capa se incluirán las siguientes tareas/lógica: 
* `GestionUsuarios`: Coordina el registro de usuarios, login. 
* `GestionPeliculas`: Coordina la busqueda, calificación de películas y gestion de categorías y actores para los admin.
* `GestionCalificaciones`: Gestiona la logica para calificar una película.

Ademas se consideran los distintos casos de uso:
* `BuscarPeliculas`: Manejar la búsqueda y paginacion de películas.
* `CalificarPelicula`: Servicio para que los usuarios califiquen una pelicula.
* `SeguirUsuario`: Logica para seguir un usuario. Desde que el usuario manda la solicitud hasta que le llega la notificación al otro usuario y acepta o rechaza.
* `RegistrarUsuario`: Se usa un metodo login con la data del usuario.
* `ObtenerCalificaciones`: Se obtienen las calificaciones enlistadas de una pelicula dado su ID.

### Capa de Infraestructura

Capa relacionada con la base de datos, autenticacion y otras interacciones externas.

* `Autenticacion JWT`: Para gestionar la autenticacion y autorización
* `Almacenamiento de imagenes para avatares`: Servicio de almacenamiento de archivos.
* `Repositorios`: Interactuan con las bases de datos para las distintas operaciones requeridas segun la entidad.
	* Usuario
	* Película
	* Actor
	* Categoría
	* Calificación
* `Base de datos`: Se almacenan las entidades usuario, película, actor, categoria y calicacion en una base de datos relacional como PostgreSql.



