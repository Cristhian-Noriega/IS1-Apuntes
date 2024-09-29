
## Introducción

Arquitectura -> bloques fundamentales de nuestro sistema
Como estructuramos la solución.

Patrones de arquitectura -> primero me fijo en los requerimientos de la aplicación (funcionales y no funcionales) y en funcion  de eso elijo el patrón de arquitectura usar.
Son de mas alto nivel (mas que los patrones de diseno).

> Architecture represents the set of significant **desing decisions** that shape the form and function of a system, where **significant is measured by cost of change**
> - Booch

> Architecture is **the important**
> - Martin Fowler

* Lo que le da forma al sistema
* Decisiones significativas en el diseno.
* Comunicacion entre componentes

![](Attachments/Pasted%20image%2020240915111849.png)


Inicialmente la low internal quality tiene una pendiente mas pronunciada que la azul. A través del tiempo la linea entrega mas funcionalidad en menor tiempo.
Hay un punto de inflexión en el cual el azul le gana a la marrón. 
Siempre es mejor tener un sistema mejor estructurado, aunque en un principio sea mas lento.

## Patrones de arquitectura

### Layers

Propone estructurar el sistema en un conjunto de capas que se comunica entre si. Que ese conjunto de capas tenga un diseño bien definido y se comuniquen entre ellos a través de sus interfaces, y de forma lineal. Una capa se comunica con la siguiente.

![](Attachments/Pasted%20image%2020240912192920.png)

> Lección importante: La misma idea se aplica en distintos contextos.

* User Interface -> Interacción con el usuario. 
* Business logic -> incumbencias del modelo de negocios
* Data access -> esconde donde están guardados los datos. Si veo que hay, veo archivos que tengan código de acceder a una base de datos relacional. Una capa antes de la base de datos. Abstracciones que tienen que ver con el acceso a datos.

Cada capa tiene una responsabilidad, separación de incumbencias.

Dominio -> area en el que se mueve el problema. Todos los conceptos que están entorno al problema que quiero resolver. 


### Broker

Es un sistema en si mismo que abstrae al cliente del servidor.
Disminuye el acoplamiento.\

El patrón **Broker** es un intermediario que facilita la comunicación entre clientes y servidores, ocultando los detalles de la red y la ubicación de los servicios. El cliente no interactúa directamente con el servidor, sino que envía sus solicitudes al **Broker**, que se encarga de enrutar la solicitud al servidor adecuado y devolver la respuesta al cliente.

![](Attachments/Pasted%20image%2020240912200113.png)


**Ejemplos y Aplicaciones:**

1. **Balanceador de Carga**: El Broker puede distribuir las solicitudes entre varios servidores para evitar la sobrecarga en uno solo. De esta manera, se optimiza el uso de recursos y se mejora la escalabilidad del sistema.
   
2. **Middleware Distribuido**: En sistemas distribuidos, el patrón Broker es común en middleware que permite que aplicaciones en diferentes máquinas o redes se comuniquen de manera transparente, manejando la serialización de mensajes, conexiones de red, y recuperación de fallos.

### Pipe and Filters

Armar una cadena de eslabones que se comunican entre si para resolver una determinada tarea.

El patrón **Pipe and Filters** organiza el procesamiento de datos como una secuencia de pasos, donde cada paso (filtro) transforma los datos y los pasa al siguiente (tubería o pipe). Es un enfoque modular que facilita la re utilización de componentes y la integración de nuevas funcionalidades sin alterar la lógica existente.

![](Attachments/Pasted%20image%2020240912200718.png)

**Ejemplos y Aplicaciones:**

1. **ETL (Extract, Transform, Load)**: Define una serie de cajas (filtros) donde cada una realiza una tarea específica (extracción, transformación o carga) sobre los datos. Los datos fluyen de forma secuencial a través de estas cajas para completar el proceso de transformación y carga en sistemas de almacenamiento.

2. **HTTP Filters**: En un servidor web, se pueden aplicar filtros sobre un request HTTP. Estos filtros pueden realizar diversas operaciones, como autenticación, registro, o transformación de datos antes de pasar el request a la aplicación principal para ser procesado.

3. **Unix Commands**: Los comandos Unix se pueden encadenar usando pipes (`|`). Cada comando toma la salida del comando anterior como entrada y realiza una operación sobre los datos. Por ejemplo:

```bash
cat archivo.txt | grep "texto" | sort | uniq
```

4. **Compilador**: Un compilador procesa código fuente a través de varias etapas o filtros: análisis léxico, análisis sintáctico, análisis semántico, optimización, y generación de código. Cada etapa toma los resultados de la anterior, los transforma, y los pasa a la siguiente.


### Arquitectura Hexagonal

Lo que importa de una app es los casos de uso, encapsulan el core/logica del sistema. A partir de ahi, el resto de las cosas son cosas colaterales llamadas "adaptadores". 
La UI es una forma de adaptar mi aplicacion a una cierta interfaz grafica.

La **Arquitectura Hexagonal**, también conocida como **Arquitectura de Puertos y Adaptadores**, es un enfoque de diseño que busca aislar el núcleo de la aplicación de sus dependencias externas (bases de datos, interfaces de usuario, servicios web, etc.), creando una separación clara entre la lógica de negocio y la infraestructura.

![](Attachments/Pasted%20image%2020240912201733.png)

Core -> dominio, lógica de negocio
Application -> casos de uso
Infraestructure -> todo el resto

La dependency rule indica que siempre se va de afuera hacia adentro. No al revés. 

Es parecido al de Layers (capas). Pero el data access y user interface también es infraestructura.

Concepto de Hexagonal -> Nació porque las apps eran bastante monótonas. Hoy en día hay muchas variantes. En data access por ejemplo tengo muchas alternativas de como implementarlo / usarlo. Por ahi ya no se accede a los datos, sino a otro sistema. 
Entonces se definió que lo importante sea lo del centro, el dominio.


**Ejemplos y Aplicaciones:**

1. **Desarrollo de Aplicaciones Modulares**: Una aplicación modular con capas bien definidas permite que los detalles de infraestructura (como la base de datos) se cambien fácilmente. Por ejemplo, podrías cambiar de una base de datos SQL a una NoSQL sin modificar la lógica de negocio, solo cambiando el adaptador.

2. **Sistemas Multicanal**: En aplicaciones que interactúan con múltiples interfaces (web, móvil, APIs), la arquitectura hexagonal permite que las mismas reglas de negocio se utilicen sin duplicación. Cada canal tiene su adaptador que conecta con el núcleo de la aplicación.

3. **Pruebas Aisladas**: Al desacoplar el núcleo de la aplicación de los detalles externos, es fácil probar la lógica de negocio de forma aislada, sin preocuparse por la infraestructura subyacente (como bases de datos o APIs externas).


![](Attachments/Pasted%20image%2020240912202405.png)

> La regla del dominio es que todo depende del dominio, el dominio no depende de nada, el dominio no puede cambiar y no puede ser afectado por capas superiores.




- **Independencia del Entorno**: La lógica central de la aplicación no depende de frameworks, bases de datos ni interfaces. Esto facilita cambios en la infraestructura sin afectar el núcleo del sistema.
  
- **Adaptadores**: Los detalles de la infraestructura (bases de datos, APIs externas, interfaces gráficas) interactúan con el núcleo de la aplicación a través de adaptadores. Estos adaptadores traducen las solicitudes externas a un formato comprensible por el núcleo.
  
- **Puertos**: El núcleo de la aplicación define interfaces (puertos) que representan los puntos de interacción con el mundo externo. Los adaptadores implementan estas interfaces para conectar con tecnologías específicas.                                                            Es una interfaz que vive en la capa de application. La implementacion de esa interfaz vive en la infraestructura. 

**PORT** -> interfaz que vive en la capa de aplicacion
**ADAPTER** -> implementacion de la interfaz del port, que vive en la infraestructura


En la **Application Layer** están los Casos de uso -> una lógica asociada asociada a resolver un problema en particular de la aplicacion.

 Ejemplo: Crear un Producto, buscar un Producto, firmar un Documento. 

Interacciones donde participan varias entidades del dominio. Esas interacciones representan una operación de mi dominio que resuelve mi sistema.

![](Attachments/Pasted%20image%2020240928152001.png)

**Diagrama UML de casos de uso**:

![](Attachments/Pasted%20image%2020240928152321.png)


En la **Infrastructure Layer** están los adaptadores de los ports. 


![](Attachments/Pasted%20image%2020240912203039.png)

La infrastructure layer se caracteriza por: 

1. Es la capa más externa de la arquitectura.
2. Contiene todos los detalles técnicos y específicos de la implementación.
3. Se encarga de la comunicación con elementos externos como bases de datos, interfaces de usuario, servicios web, etc.
4. Utiliza adaptadores para conectar estos componentes externos con la lógica de negocio central.
5. Implementa las interfaces definidas por la capa de dominio, permitiendo que los detalles técnicos se mantengan separados de la lógica de negocio.
6. Proporciona flexibilidad para cambiar o actualizar componentes tecnológicos sin afectar el núcleo de la aplicación.
7. Incluye elementos como frameworks, bibliotecas externas, configuraciones y código de infraestructura.

Esta capa permite que la aplicación se comunique con el mundo exterior mientras mantiene el dominio de negocio aislado y protegido de los detalles de implementación



---




Cada caso de uso en una clase en OOP. Si es un paradigma funcional, los casos de usos son las funciones.

![](Attachments/Pasted%20image%2020240912203248.png)


Misma implementacion pero de forma distinta

![](Attachments/Pasted%20image%2020240912203415.png)

Testing??? Esta acoplado a HTTP 
Me debo abstraer. Crear una abstraccion.

### Repository Pattern

Es una abstraccion de donde guardar datos

![](Attachments/Pasted%20image%2020240912203554.png)

Ahora el caso de uso (`createUser`) instancio el repositorio y llamo al repositorio para guardar. Ahora el caso de uso ya no sabe como se guardan las cosas. 

Quiero sacar la linea `const repository = new UserRepository();` ya que quiero que el caso de uso no sepa que implementacion concreta de repository este usando.

Puedo componer el `createUser` con el `repository`. Que reciba por parametro.

-> Aplico el principio de inversion de dependencias.

![](Attachments/Pasted%20image%2020240912203747.png)

`CreateUser` depende de `fetch` y `database`. Hay una abstraccion con `UseRepository` pero quiero invertir la dependencia.

![](Attachments/Pasted%20image%2020240928200937.png)

`UserAPIRepository` y `UserDBRepository` implementan la interfaz de `UserRepository`.
A esas dos implementaciones concretas las debo meter como parametro al `CreateUser`.


![](Attachments/Pasted%20image%2020240928201233.png)


![](Attachments/Pasted%20image%2020240928201328.png)


Todavía hay que resolver el problema de sacar el `new` -> Usamos Dependency Injection

> Dependency Injection is a software design technique in which the creation and binding of dependencies are done outside of the dependent class.



![](Attachments/Pasted%20image%2020240912203908.png)

![](Attachments/Pasted%20image%2020240928201647.png)

Puedo llamar a `createUser` con distintos repositories. Encapsula lo que varia que es el repositorio que quiero usar.

Por lo tanto la idea es ...

> Tener casos de uso que dependan de abstracciones (PORTS), tener implementaciones de esas abstracciones (ADAPTERS) que viven en la infraestructura para cada caso (acceder a la base, llamar a una API, etc) y en algún momento inyectar esas dependencias.
> El momento de hacer esa inyeccion es cuando arranca el sistema. 


---

### Workflow

![](Attachments/Pasted%20image%2020240928202932.png)

* Capa de infraestructura -> Adapters
* Capa de aplicacion -> Casos de uso y dominio
* Capa de dominio -> Ports (interfaces de los adapters) y Entities

Cuando arranca el sistema, se inyecta las implementaciones concretas de las abstracciones. Los adapters se inyectan adentro de los ports. 

--- 

### Folder structure

![](Attachments/Pasted%20image%2020240928203439.png)

> El de la derecha es todo infraestructura. La UI.




