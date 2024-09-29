
## UML

UML es un lenguaje o forma de representar visualmente la arquitectura y diseno de sistemas de softwares complejos

![](Attachments/Pasted%20image%2020240909193209.png)

Agregacion vs Composicion

Tipos de diagramas estructurales
* Diagrama de clases
* Diagrama de paquetes -> como agrupar las clases en distintos paquetes, invoulucra acoplamiento
* Diagrama de objetos -> representación de instancias de objectos en determinado estado
* Diagrama de componentes -> componente = empaqueta un conjunto de clases  (archivos .jar en java)
*![](Attachments/Pasted%20image%2020240909194336.png)
* Diagrama de despliegue -> mostrar nodos fisicos que representan computadoras, telefonos (hardware). "Como despliego componentes en estos nodos"
*![](Attachments/Pasted%20image%2020240909194459.png)


Diagramas de comportamiento
* Diagrama de actividades -> documenta el flujo o procedimiento de una actividad
*![](Attachments/Pasted%20image%2020240909194648.png)
* Diagrama de secuencias
* Diagrama de casos de uso
*![](Attachments/Pasted%20image%2020240909194734.png)
* Diagrama de estados 
*![](Attachments/Pasted%20image%2020240909194804.png)



---
## Arquitectura de Software

La arquitectura de software representa la estructura o las estructuras del sistema, que consta de componentes de software 

> La arquitectura de software son aquellas decisiones que son importantes y dificiles de cambiar. 
> Martin Fowler.

Que debemos tener en cuenta cuando escribimos un documento, informe o queremos transmitir información? -> Hay que tener en cuenta a quien le queremos trasmitir lo que vayamos a contar. Eso es lo principal, saber a quien va dirigido.

![](Attachments/Pasted%20image%2020240909195905.png)

Muchos participantes y distintos intereses.

### Modelo de vistas 4+1 

Separar en distintas vistas las distintas decisiones que se fueron tomando en el proyecto.

#### Vista Lógica

La vista lógica apoya principalmente los requisitos funcionales lo que el sistema debe brindar en términos de servicios a sus usuarios. 

Aquí se aplican los principios de abstracción, encapsulamiento y herencia. 

Esta descomposición no sólo se hace para potenciar el análisis funcional, sino también sirve para identificar mecanismos y elementos de diseño comunes a diversas partes del sistema

Acá irían diagramas de secuencias, de clases, etc. Todo lo lógico.


#### Vista de Procesos

Procesos corriendo en memoria, acerca de que se esta ejecutando. Trata de contar la estructura de nuestro proyecto a nivel de procesos.

La vista de procesos toma en cuenta algunos requisitos no funcionales tales como el rendimiento y la disponibilidad. 

Se enfoca en asuntos de concurrencia y distribución, integridad del sistema, de tolerancia a fallas. 

Un proceso es una agrupación de tareas que forman una unidad ejecutable. Los procesos representan el nivel al que la arquitectura de procesos puede ser controlada tácticamente (i.e., comenzar, recuperar, reconfigurar, y detener). 

Además, los procesos pueden replicarse para aumentar la distribución de la carga de procesamiento, o para mejorar la disponibilidad.

> Sirve para ver cuantos procesos necesito para el modelo


#### Vista de Desarrollo (o de Componentes)

La vista de desarrollo se centra en la organización real de los módulos de software en el ambiente de desarrollo del software. 

El software se empaqueta en partes pequeñas –bibliotecas de programas o subsistemas– que pueden ser desarrollados por uno o un grupo pequeño de desarrolladores. 

La vista de desarrollo tiene en cuenta los requisitos internos relativos a la facilidad de desarrollo, administración del software, re utilización y elementos comunes, y restricciones impuestas por las herramientas o el lenguaje de programación que se use.

> Todos esas clases las empaqueto en distintos .exe o .jar ? Como los empaqueto físicamente? O varias capas lógicas en distintos paquetes físicos ? 


En un proyecto web, la vista de desarrollo podría mostrar componentes como:

- Controladores (responsables de manejar las solicitudes HTTP).
- Repositorios (que manejan el acceso a la base de datos).
- Servicios (que implementan la lógica de negocio).

#### Vista Física (o de Despliegue)

> Nodos físicos, cuantas maquinas necesito para este proyecto? 

La vista física toma en cuenta primeramente los requisitos no funcionales del sistema tales como la disponibilidad, confiabilidad (tolerancia a fallas), rendimiento (throughput), y escalabilidad.

El software se ejecuta sobre una red de computadores o nodos de procesamiento. Los variados elementos identificados –redes, procesos, tareas y objetos– requieren ser mapeados sobre los nodos.

Esperamos que diferentes configuraciones puedan usarse: algunas para desarrollo y pruebas, otras para mostrar el sistema en varios sitios para distintos usuarios. Por lo tanto, la relación del software en los nodos debe ser altamente flexible y tener un impacto mínimo sobre el código fuente.

> Se usa el diagrama de despliegue de UML.
> Balanceadores de carga. Servidores. 


#### Vista de Escenarios (negocio)

Vista +1

Los elementos de las cuatro vistas trabajan conjuntamente en forma natural mediante el uso de un conjunto pequeño de escenarios relevantes.

Los escenarios son de alguna manera una abstracción de los requisitos más importantes.

Sirve a dos propósitos principales:

• Como una guía para descubrir elementos arquitectónicos durante el diseño de arquitectura

• Como un rol de validación e ilustración después de completar el diseño de arquitectura, en el papel y como punto de partido de las pruebas de un prototipo de la arquitectura.

> En base a los escenarios se empieza a plantear la arquitectura.

![](Attachments/Pasted%20image%2020240909202039.png)


### Conclusiones

- Permite a través de diferentes vistas analizar distintas perspectivas del problema, focalizándose en el problema en cuestión 
- Concentra en un único documento las principales decisiones tomadas sobre el sistema 
- Permite a nuevos integrantes del equipo entender la arquitectura del sistema y ubicarse dentro de la solución 
- Permite discutir con todos los stakeholders las distintas decisiones y validarlas en una etapa temprana

---


## Decisiones de arquitectura

Decisiones de Arquitectura son decisiones de diseño que abordan requisitos significativos desde el punto de vista arquitectónico; se perciben como difíciles de hacer y/o costosos de cambiar

Estas decisiones son importantes de guardarlas y conocerlas. Conocer el motivo por el cual se tomó dicha decisión en la arquitectura o diseño.

Por ejemplo, para el envío de emails se usa un SMTP y realiza el envío programáticamente gestionado por nuestro backend o se delega a un servicio de email provider de terceros como ser Sendgrid, Mailgun, etc.

* Estado (propuesta, aceptada, deprecada, sustituida)
* Contexto
* Decision
* Consecuencias


### Ejemplo

![](Attachments/Pasted%20image%2020240928122845.png)


---

### Ejercicio en clase


![](Attachments/Pasted%20image%2020240928123706.png)


![](Attachments/Pasted%20image%2020240909215646.png)

![](Attachments/Pasted%20image%2020240928135932.png)

---



## Bibliografia

https://github.com/7510-tecnicas-de-disenio/material-clases/blob/master/Arquitectura/4%2B1view-architecture-Krutchen.pdf

https://www.youtube.com/watch?v=DngAZyWMGR0

https://github.com/7510-tecnicas-de-disenio/material-clases/tree/mast









