
## Problemas anteriores a Docker

- Altamente repetitivo 
- Gran probabilidad de error
- Interacciones entre aplicaciones 
- Es difícil reconstruir la configuración de un servidor
  
  
## Maquinas virtuales

Emulación de una máquina real  
* Provee aislamiento total 
* Única opción para ejecutar programas de otras arquitecturas de hardware

> Esa simulacion es costosa, ralentiza la maquina

Herramientas como VirtualBox


## Infraestructura as Code (IaC)

En lugar de realizar modificaciones manualmente, se usa un lenguaje de programación específico para el dominio de configuraciones de máquinas
Algunas herramientas: 
- Chef 
- Puppet 
- Ansible 
- Terraform


## Mascota vs Ganado

Infraestructura Mascota

- Nombres específicos 
- Unicas 
- Cuidadas 
- Mantenimiento delicado 
- Presentes aunque no sean necesarios 
- Servicios legacy 
- Servicios unicos

Infraestructura Ganado

* Numero de serie 
* Practicamente identicas 
* Reemplazable 
* "Mantenimiento" creando otra instancia 
* Se crean y destruyen a necesidad 
* Servicios sin estado interno relevante

> Docker soporta ambas, pero es mas fácil usar infraestructura ganado en docker

> Rest es stateless, entonces va a ser apta a ser utilizada con infraestructura ganado. Relacionado con servicios sin estado interno relevante

## Que es docker

Docker es una herramienta que automatiza el despliegue de aplicaciones aisladas dentro de contenedores


### Containers vs Virtual Machines

![](Attachments/Pasted%20image%2020240923193018.png)

Los **containers** y las **máquinas virtuales** son tecnologías de virtualización que permiten aislar aplicaciones. Sin embargo, se diferencian en cómo logran este aislamiento y la eficiencia que ofrecen.

- **Containers**: Se ejecutan sobre el sistema operativo anfitrión (Host OS) y utilizan un **runtime de contenedores** que comparte el kernel del sistema operativo. Esto los hace mucho más ligeros que las VMs, ya que no requieren un sistema operativo completo por contenedor, solo las aplicaciones y sus dependencias.

- **Máquinas virtuales (VMs)**: Cada VM incluye su propio sistema operativo (Guest OS) y hardware emulado, que se ejecuta sobre un **hipervisor**. Este enfoque proporciona un mayor nivel de aislamiento pero a costa de mayor uso de recursos, ya que cada VM necesita una copia completa del sistema operativo.


En resumen, los **containers** son más eficientes en cuanto a recursos y ofrecen arranques más rápidos en comparación con las **máquinas virtuales**, pero las VMs ofrecen un mayor nivel de aislamiento y control.


### Conceptos de Docker

* Container: Grupo aislado de procesos
* Image: Template usado para crear contenedores  (sistema de archivos + metada)
* Dockerfile: Archivo con instrucciones para construir una imagen

### Que brinda docker

* Consistencia: Cualquier imagen se obtiene, inicia y configura de la misma manera
* Replicabilidad: Los contenedores basados en una misma imagen son inicialmente iguales
* Aislamiento: Se puede controlar la interacción entre la aplicación en un contenedor y el mundo exterior (en ambas direcciones)

### Contenedores livianos

El ecosistema Docker asume que los contenedores son livianos:

Dedicados a un único propósito 
* Ejecutan un comando 
* No incluyen dependencias innecesarias 
 
Mantienen el estado mínimo necesario 
* "Mínimo necesario" no significa "Poco". Por ejemplo, bases de datos 

Pueden encender/apagar rápidamente 
* Útil para escalar horizontalmente


### Ecosistema

![](Attachments/Pasted%20image%2020240923193624.png)


No hay forma de compartir un contenedor en sí, pero se pueden compartir: - Imágenes en repositorios como hub.docker.com - Imágenes exportadas a archivos comprimido - El Dockerfile que generó la imagen (Aunque eso no garantiza generar una imagen idéntica)


> Una imagen es una tira de bites (.tar), la gente de distintas plataformas deja preparada una imagen con el software ya preparado para ser ejecutado, facilitan el set up del software armado para el usuario. Uno puede hasta correr un sistema operativo con: 

```bash
docker run -it ubuntu:20.04
```

Pero no puedo tener un container de windows en un linux.

Otros elementos que maneja Docker:

* mounts: Acceso a directorios del host
* volumenes: Area donde se persisten datos
* networks: interfaces de red internas 
* ports: forwardear puertos del host a puertos del contenedor

```bash
docker run -v host_dir:container_dir
```


> Anfitrión/host: maquina externa, la que corre docker
> Guests: containers

Permite montar un directorio del sistema anfitrión (`host_dir`) dentro del contenedor (`container_dir`). Esto da acceso directo a los archivos del sistema anfitrión desde el contenedor, facilitando la persistencia de datos o el acceso a recursos externos.

- **Acceso a directorios**: Los volúmenes permiten compartir archivos entre el host y el contenedor.
- **Modo de solo lectura**: Opcionalmente, se puede marcar el volumen como de solo lectura agregando `:ro` al final, para evitar modificaciones desde el contenedor.

Por ejemplo, se puede montar un directorio `/src` del host dentro de `/app` en el contenedor, asegurando acceso directo a los archivos para su manipulación o lectura.

![](Attachments/Pasted%20image%2020240923195328.png)

### Mapeo de puertos 


```bash
$ docker run -p external_port:internal_port ...
```

permite reenviar los paquetes de red recibidos en el `external_port` del host hacia el `internal_port` del contenedor. Esto facilita el acceso a servicios dentro del contenedor desde el exterior.

- **Reenvío de puertos**: Forwardea paquetes de la red desde el puerto externo del host hacia el puerto interno del contenedor.
- **Especificar dirección IP**: Puedes indicar en qué IP del host se escucharán las conexiones entrantes. Por ejemplo:
  
```bash
-p 127.0.0.1:12345:80
```

El puerto `12345` del host redirige hacia el puerto `80` del contenedor, permitiendo acceso al servicio en esa dirección IP específica del host.

--- 


Con la misma facilidad que pudimos iniciar un servidor web, podríamos haber iniciado cualquier aplicación dockerizada Conociendo solo mapeos de puertos, almacenamiento y variables de entorno ➔ Vista fisica de 4+1

![](Attachments/Pasted%20image%2020240923200636.png)

--- 

## DockerFIle

Este es un `Dockerfile` básico para una aplicación interpretada en JavaScript usando Node.js:

```Dockerfile
FROM node:16
WORKDIR /app
COPY . .
RUN npm ci
CMD ["node", "server.js"]
```

- **Base de imagen**: Comienza con una **imagen oficial de Node.js** en su versión 16 (`FROM node:16`).
- **Directorio de trabajo**: Define un **directorio dentro del contenedor** donde se ubicará la aplicación (`WORKDIR /app`).
- **Copiar archivos**: **Copia todos los archivos** desde el directorio actual del host al directorio `/app` del contenedor (`COPY . .`).
- **Instalación de dependencias**: **Instala las dependencias** de Node.js usando `npm ci` para asegurar que las versiones de las dependencias sean consistentes (`RUN npm ci`).
- **Comando de inicio**: Finalmente, **define el comando que se ejecutará** cuando se inicie el contenedor (`CMD ["node", "server.js"]`), que en este caso ejecuta el archivo `server.js`.

Serie de pasos para construir una imagen.

![](Attachments/Pasted%20image%2020240923201320.png)

docker history : Muestra cada paso que produjo una imagen


Las imágenes se almacenan como una referencia a la imagen anterior y una lista de diferencias, llamada layer
➔ Cada layer es inmutable 
➔ Las referencias son de un layer a su predecesor 
	◆ Dos builds pueden compartir layers hasta la primer diferencia 
	◆ Un archivo agregado en un layer y borrado en otro layer sigue siendo accesible 
	◆ Imágenes ya construidas sirven como una cache

Para reuso, se asume que cada layer depende solo de:
	 ● Imagen anterior 
	 ● El comando ejecutado 
	 ● Los archivos usados del directorio del Dockerfile (el contexto) 
		 ○ .dockerignore: Excluir archivos del contexto (git, node_modules, etc) 
Se ignoran otras entradas implícitas:
	 ● Comunicación con internet 
	 ● Aleatoriedad 
	 ● Pasaje del tiempo 
	 ● Etc

---

## Dockerfile para una aplicación Node.js

Un Dockerfile típico para una aplicación Node.js interpretada sigue una estructura específica para optimizar el proceso de construcción y ejecución del contenedor. Aquí se explica cada parte:

```Dockerfile
# Dockerfile
FROM node:20-slim
WORKDIR /app

COPY ./package.json .
COPY ./package-lock.json .
RUN npm ci

COPY . .
CMD ["node", "server.js"]
```

- `FROM node:20-slim`: Usa una imagen base de Node.js ligera (versión 20).
- `WORKDIR /app`: Establece el directorio de trabajo dentro del contenedor.
- `COPY ./package.json .` y `COPY ./package-lock.json .`:
    - Copia solo los archivos necesarios para instalar dependencias.
    - Estos archivos cambian con poca frecuencia, lo que ayuda a optimizar el caché de capas de Docker.
- `RUN npm ci`:
    - Instala las dependencias del proyecto.
    - Usa `npm ci` en lugar de `npm install` para garantizar instalaciones consistentes basadas en package-lock.json.
- `COPY . .`: Copia el resto del código de la aplicación al contenedor.
- `CMD ["node", "server.js"]`: Define el comando para ejecutar la aplicación.

## Notas importantes:

- Esta estructura aprovecha el caché de capas de Docker. Al copiar primero solo los archivos de dependencias y ejecutar `npm ci`, se crea una capa cacheada que no necesita reconstruirse si el código de la aplicación cambia pero las dependencias no.
- Usar `npm ci` en lugar de `npm install` puede parecer un cambio menor, pero es importante para la consistencia y puede ahorrar tiempo en builds repetidos, especialmente en entornos de desarrollo con múltiples desarrolladores.
- La optimización de la estructura del Dockerfile puede parecer innecesaria para proyectos pequeños, pero se vuelve crucial cuando se multiplica por todos los builds que realiza un equipo de desarrollo.

Esta estructura de Dockerfile es un ejemplo de buenas prácticas para aplicaciones Node.js, optimizando tanto el proceso de construcción como el rendimiento del contenedor resultante.


---

## Orquestadores y Docker Compose

Un orquestador coordina el uso de **múltiples** contenedores para implementar una única aplicación, brindando: 
- Configuración de contenedores heterogéneos 
- Distribución 
- Balanceo de carga
- Tolerancia a fallos 

Algunos comunes: docker-compose, kubernetes

Docker Compose es una herramienta para definir y ejecutar aplicaciones Docker multi-contenedor. Permite describir la configuración de servicios, redes y volúmenes en un archivo YAML, simplificando la gestión de aplicaciones complejas.

### Ejemplo: De Docker Run a Docker Compose

#### Comando Docker Run

Anteriormente, se utilizaba directamente el comando `docker run`:
```bash
$ docker run -v "./nginx_html:/usr/share/nginx/html:ro" -p 12345:80 nginx:1.23
```

Este comando inicia un contenedor de Nginx, montando un volumen local y exponiendo un puerto.

#### Equivalente en Docker Compose

El mismo despliegue se puede definir en un archivo `docker-compose.yml`:

```yaml
version: "3"

services:
  webserver:
    image: nginx:1.23
    volumes:
      - "./nginx_html:/usr/share/nginx/html:ro"
    ports:
      - "12345:80"
```

Este archivo describe:

- La versión de Docker Compose (3)
- Un servicio llamado "webserver"
- La imagen de Docker a utilizar (nginx:1.23)
- El volumen montado
- El mapeo de puertos

### Uso de docker compose

Para iniciar los servicios:

```bash
docker-compose up -d
```

El flag `-d` ejecuta los contenedores en segundo plano.

Para detener y eliminar los servicios:

```bash
docker-compose down
```

### Ventajas de Docker Compose

1. **Configuración declarativa**: Describe toda la aplicación en un solo archivo.
2. **Reproducibilidad**: Facilita replicar el entorno en diferentes máquinas.
3. **Gestión simplificada**: Maneja múltiples contenedores como una unidad.
4. **Desarrollo local**: Ideal para entornos de desarrollo y pruebas.

### Orquestadores

Docker Compose es excelente para entornos de desarrollo y aplicaciones pequeñas, pero para despliegues más complejos y a gran escala, se utilizan orquestadores como Kubernetes o Docker Swarm. Estos ofrecen:

- Escalabilidad automática
- Balanceo de carga
- Recuperación ante fallos
- Actualizaciones continuas
- Gestión de configuración y secretos

Docker Compose puede ser un buen punto de partida para familiarizarse con la definición de servicios antes de migrar a orquestadores más avanzados.

## Configuraciones y buenas practicas

#### Docker Compose: Configuración de Base de Datos

El archivo `docker-compose.yml` es fundamental para definir servicios en Docker Compose. Aquí un ejemplo para una base de datos MySQL:

```yaml
# docker-compose.yml
version: "3"

volumes:
  db_persist:

services:
  db:
    image: mysql:8.0
    ports:
      - "12345:3306"
    volumes:
      - db_persist:/var/lib/mysql
    environment:
      MYSQL_USER: "${DB_USER}"
      MYSQL_PASSWORD: "${DB_PASSWORD}"
```

### Características clave:

1. **Volumen persistente**: `db_persist` para almacenar datos de MySQL.
2. **Servicio de base de datos**: Usa la imagen `mysql:8.0`.
3. **Mapeo de puertos**: Expone MySQL en el puerto 12345 del host.
4. **Variables de entorno**: Utiliza variables (`${DB_USER}`, `${DB_PASSWORD}`) para credenciales.

### Notas importantes:

- El cliente generalmente es parte del sistema, mientras la base de datos está oculta.
- Docker Compose gestiona el volumen `db_persist`.
- Las variables de entorno se completan con valores de:
    - Variables de entorno del sistema
    - Un archivo `.env` (si no se especifica otro con `--env-file`)

## Uso de archivos .env en Orquestadores

Los archivos `.env` son cruciales para separar la configuración en orquestadores:

### 1. Configuración del contexto de despliegue

- Incluye: Puertos, URLs, API keys
- Se definen como variables en `.env`

### 2. Configuración interna del sistema

- Configura contenedores internos no visibles desde fuera
- Ejemplos: Puertos/hosts para comunicación interna
- Generalmente hardcodeado en `docker-compose.yml`
- Puede usar YAML Anchors para evitar duplicación

### Mejores prácticas:

- Usar `.env` para separar configuración sensible o específica del entorno.
- Mantener la configuración interna en `docker-compose.yml` para mejor control y visibilidad.
- Utilizar YAML Anchors para reducir repetición en configuraciones complejas.

Esta estructura permite una clara separación entre la configuración específica del entorno (que puede variar entre desarrollo, pruebas y producción) y la configuración interna del sistema, facilitando el mantenimiento y la seguridad de las aplicaciones orquestadas con Docker Compose.


## Importancia de los volúmenes 

En el ejemplo del `docker-compose.yml` para MySQL, vemos esta línea:

```yaml
volumes:
  - db_persist:/var/lib/mysql
```

Este volumen, `db_persist`, es crucial por varias razones:

### 1. Persistencia de Datos

### Problema sin volúmenes:

Sin un volumen, todos los datos almacenados en la base de datos MySQL se perderían si el contenedor se destruyera o se reiniciara. Esto se debe a que, por defecto, los datos en un contenedor son efímeros.

### Solución con volúmenes:

El volumen `db_persist` permite que los datos persistan independientemente del ciclo de vida del contenedor. Esto significa que:

- Si el contenedor se detiene y se reinicia, los datos permanecen intactos.
- Si el contenedor se destruye y se crea uno nuevo, los datos aún estarán disponibles.

### 2. Backup y Migración

- **Facilita backups**: Los datos en el volumen pueden ser respaldados independientemente del contenedor.
- **Migración simplificada**: Los datos pueden moverse fácilmente entre diferentes entornos o máquinas.

### 3. Rendimiento

- Los volúmenes pueden ofrecer mejor rendimiento que escribir en el sistema de archivos del contenedor, especialmente en sistemas con controladores de almacenamiento optimizados.

### 4. Seguridad

- Los volúmenes pueden configurarse con permisos específicos, mejorando la seguridad de los datos almacenados.

### Ejemplo: 

Imaginemos un escenario con nuestra base de datos MySQL:

1. El contenedor se ejecuta y la base de datos se llena con datos importantes.
2. Por alguna razón, el contenedor se destruye (error, actualización, etc.).
3. Sin el volumen `db_persist`, todos esos datos se perderían.
4. Con el volumen, al crear un nuevo contenedor y vincularlo al mismo volumen, todos los datos estarán disponibles inmediatamente.

```yaml
services:
  db:
    image: mysql:8.0
    volumes:
      - db_persist:/var/lib/mysql
```

Incluso si `docker-compose down` se ejecuta (lo que normalmente destruiría los contenedores), los datos en `db_persist` se mantendrán seguros.

El uso de volúmenes, como `db_persist` en nuestro ejemplo, es fundamental para garantizar la integridad y disponibilidad de los datos en aplicaciones basadas en contenedores, especialmente para servicios críticos como bases de datos.