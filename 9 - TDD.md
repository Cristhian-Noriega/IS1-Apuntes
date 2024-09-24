
## Leyes de Robert C Martin

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a  unit test than is sufficient to fail; and compilation failures are failures. -> Trabajar de a un test fallando, centrarse en uno y hacerlo pasar.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test. 

> Hacer baby steps para obtener una cobertura al 100%

## Katas

El objetivo de una kata para nosotros no es llegar a una respuesta correcta sino lo que se aprende en el camino, el objetivo es la práctica no la solución.

links: 

Kata iniciación 
* https://github.com/ferpega/ohceKata 
* https://github.com/trikitrok/ohce-kata-java-using-outside-in-tdd 
* https://github.com/francho/kata-ohce


Kata refactor 
* https://github.com/DoDevJutsu/incomprehensible-finder-refactoring-java 
* https://github.com/celtric/incomprehensible-finder-refactoring-java 
* https://www.meetup.com/es-ES/Barcelona-Software-Craftsmanship/events/233107734/?eventId=233107734&chapter_analytics_code=UA-46511806-1 
* http://codely.tv/screencasts/finder-kata-refactoring/

--- 


## Refactoring

> A technique for restructuring an existing body of code, altering its internal structure without changing its external behavior.


* Ensure all tests pass
* Find code that smells
* Find refactoring
* Apply refactoring

### Code smells

* Duplicated Code -> no solo da a nivel de una clase, sino en varias clases, tambien en una herencia.
* Long Method 
* Large Class
* Long parameter list 
* Divergent Change -> muchos metodos con un criterio y otros con un criterio distinto. 
* Shotgun Surgery -> Alto acoplamiento, muchas clases dependen/conoce de una clase
* Feature Envy -> Una clase envidia a otra. Una clase quisiera ser otra clase, porque hace mucho de sus metodos/atributos. 
* Data Clumps -> metodos siempre reciben las mismas clases/cosas. Habia que modelar en una clase y encapsular esas cosas.
  ![](Attachments/Pasted%20image%2020240919195300.png)
* Primitive Obsession -> falta modelar en un concepto, en una clase para cada uno. Deberían estar encapsulados en un concepto.
```
double money; 
String phone; 
String zipCode; 
String password;
```

* Switch Statements -> Rompe Single Responsibility.  
* Lazy Class -> Clase con un solo método.  Comportamiento que puede estar en otro lado y deberia estar aca, o esta clase no es necesaria.
* Message Chains -> `metodo1().metodo2().metodo3().metodo4()`
* Data Class -> muchos getters y setters. Se puede usar para un DTO, pero sino, debería estar eso en una clase Manager o Controller. 


---

### Bibliografia

* Refactoring - Martin Fowler
* Refactoring to patterns

