La técnica de Historias de Usuario está muy asociada con el contexto de las metodologías ágiles.

Descripción de la funcionalidad del sistema, desde el punto de vista de un usuario

**Como** \<persona>
**quiero** \<funcion>
**para** \<objetivo>

Se enfocan en el valor provisto, dando pie a discutir el requisito.
No son el requisito en sí

> Da el punto de partida para describir la funcionalidad del sistema desde el punto de vista del usuario.

> El enfoque de las historias de usuario es el valor que se va a proveer y para quien va a ser provisto.

### Ejemplos

**Como** Carla (estudiante universitaria)
**quiero** consultar el listado de cursos disponibles de una materia 
**para** decidir a qué curso anotarme

**Como** Juan Carlos (jubilado)
**quiero** imprimir un comprobante de mi reserva de turno 
**para** tener recordatorio de mi turno si mis dispositivos electrónicos fallan


### Framework QUS (Quality User Story)

![](Attachments/Pasted%20image%2020240928093134.png)


### Épicas

**Épicas**: Historias de usuario que se pueden descomponer en más historias de usuario

Como estudiante, quiero inscribirme en materias para el cuatrimestre para… 
- … quiero saber cuando me corresponde inscribirme … 
- … quiero saber en qué cursos puedo inscribirme … 
	- … quiero saber qué materias hay en mi plan de estudios … 
	- … quiero saber qué materias tengo aprobadas … 
	- … quiero saber qué cursos tienen cupo disponible para cada materia … 
- … quiero inscribirme en cursos …

Deseamos subdividir las épicas hasta que llegar a tareas de tamaño razonable


### Criterios INVEST

Se recomienda que las tareas del backlog cumplan:

![](Attachments/Pasted%20image%2020240928093759.png)


### Malas historias de usuario

- Mal formada: Quiero descargar reportes mensuales
	  quien quiere esto? para que?
	   
* Orientada a la solucion: Como analista, quiero poder exportar las compras a CSV para poder abrir los datos en excel para poder calcular un promedio para poder enviarlo cada dia en un mail a los gerentes. 
	- La historia presupone detalles de implementacion (csv, excel, analista)
	- El usuario en realidad es el gerente
	- La historia de usuario no es atomica

 *  Demasiado grande


Hay casos especificos donde tamibien depende el contexto si se puede incluir detalles de implementacion. Ejemplo: 

**Como** Alice (personaje de ejemplos de seguridad informatica)
**quiero** que mis mensajes se transmiten encriptados usando \<algoritmo especifico>
**para** evitar que mis adversarios puedan leer mis mensajes

Existen situaciones en que la necesidad del usuario realmente requiere una implementación específica.



### 3 Cs (Card, Conversation, Confirmation)

**Card**: Describe la intención del usuario

**Conversation**: Los interesados se comunican para refinar las historias, descubriendo y documentando requisitos

**Confirmation**: Criterios de aceptación

#### Criterios de aceptación

Condiciones específicas que deben cumplirse para aceptar trabajo realizado

- Lista de condiciones
- Escenarios
  **Dado que** \<contexto>
  **cuando suceda** \<evento>
  **entonces** \<consecuencia>


> Es un criterio que dice que la implementacion que sea que le demos a esta historia de usuario realmente cumple con las cosas que quiere el usuario.
> Es para validar los requerimientos del usuario y si realmente le va a servir.


Deben ser: 
- Claros 
- Concisos 
- Verificables 
- Independientes 
- Orientados al Problema


### Behavior Driven Development

Una vez que se formularon los criterios de aceptación, el siguiente paso en BDD es automatizarlo y a partir de eso crear software que funcione.

Behavior Driven Development (BDD) es una metodología ágil de desarrollo de software que extiende las prácticas de **Test Driven Development (TDD)** al enfocar el desarrollo en el comportamiento deseado del sistema. En lugar de escribir pruebas basadas en la implementación, en BDD las pruebas se centran en cómo debe comportarse el software desde la perspectiva del usuario o del negocio.

El objetivo principal de BDD es fomentar la colaboración entre los desarrolladores, testers, y stakeholders no técnicos (como los clientes o product owners) al proporcionar un lenguaje común y fácil de entender para describir las funcionalidades del sistema. -> se usa el lenguaje **Gherkin**

#### Estructura en BDD

Los escenarios en BDD están formulados en torno a **criterios de aceptación**, los cuales describen el comportamiento del software bajo ciertas condiciones. Estos se dividen en tres partes clave:

1. **Given (Dado)**: Establece el contexto inicial o el estado del sistema.
2. **When (Cuando)**: Describe la acción que el usuario realiza o el evento que ocurre.
3. **Then (Entonces)**: Indica el resultado esperado o el cambio en el sistema.

Una vez que los criterios de aceptación han sido definidos, el siguiente paso en BDD es automatizarlos usando herramientas como **Cucumber**.

#### Ejemplo carrito de compras en tienda online

Los **criterios de aceptación** son:

1. Como usuario, quiero agregar productos al carrito para realizar una compra.
2. El carrito debe mostrar el total actualizado con cada producto agregado.

```gherkin
Feature: Carrito de Compras
  Como usuario
  Quiero poder agregar productos al carrito y ver el total actualizado.
  Para realizar compras

  Scenario: Agregar un producto al carrito
    Given que el carrito está vacío
    When agrego un "Laptop" que cuesta 1000 dólares al carrito
    Then el carrito debería mostrar 1 artículo
    And el total debería ser 1000 dólares

  Scenario: Agregar múltiples productos al carrito
    Given que el carrito está vacío
    When agrego un "Laptop" que cuesta 1000 dólares al carrito
    And agrego un "Mouse" que cuesta 50 dólares al carrito
    Then el carrito debería mostrar 2 artículos
    And el total debería ser 1050 dólares
```


#### Ciclo de BDD

- **Discovery**: Durante esta fase, los equipos colaboran para identificar los comportamientos deseados del sistema a partir de las historias de usuario.
    
- **Formulation**: Los comportamientos descubiertos se formulan en escenarios claros y entendibles, utilizando un lenguaje compartido (como Gherkin) que puede ser comprendido tanto por técnicos como por no técnicos.
    
- **Automation**: Los escenarios formulados se automatizan mediante pruebas, asegurando que el sistema se comporta como se espera y que cualquier cambio futuro no rompa la funcionalidad.
    
- **Working Software**: Una vez que las pruebas están automatizadas y pasan correctamente, el software se considera funcional y cumple con los criterios de aceptación definidos inicialmente.

![](Attachments/Pasted%20image%2020240928103623.png)


### Casos de uso

Descripción de: 
- Una serie de acciones 
- Realizadas por un sistema 
- Que generan un resultado observable de valor para un actor en particular

Compuestos de un escenario principal y un conjunto de escenarios alternativos

![](Attachments/Pasted%20image%2020240928104644.png)


![](Attachments/Pasted%20image%2020240928104838.png)


### Como encontrar historias de usuario? 

- Establecer los límites del sistema 
- Identificar actores involucrados en el sistema 
- Buscar objetivos y actividades 
  - User Story Mapping 
  - Impact Mapping


#### User story mapping


**Story Mapping** es una técnica visual que se utiliza para descomponer y organizar historias de usuario en un producto. Ayuda a entender el flujo de trabajo desde la perspectiva del usuario, priorizando las funcionalidades de acuerdo con su valor. El proceso comienza dividiendo el trabajo en **actividades principales** (en rojo) que se descomponen en **actividades más específicas** (en amarillo). Luego, estas actividades se desglosan en **tareas concretas** (en azul) que se agrupan en versiones o entregas incrementales (v1, v2, v3), mostrando cómo evolucionará el producto a lo largo del tiempo.

![](Attachments/Pasted%20image%2020240928110800.png)

![](Attachments/Pasted%20image%2020240928110832.png)


![](Attachments/Pasted%20image%2020240928110852.png)

