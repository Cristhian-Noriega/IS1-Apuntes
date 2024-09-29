> El análisis es acerca del *que*, el diseño es acerca del *como*.

Por que necesitamos un buen diseño ? 

* Para tener un delivery rápido
* Para manejar el cambio
* Para lidiar con la complejidad

Un mal diseño tiene estos síntomas: 

* RIGIDEZ
* FRAGILIDAD
* INMOBILIDAD

### Rigidez

Implica que es difícil de modificar. Un cambio en una parte del sistema implica cambiar en cascada distintas partes del sistema. 
Esto se debe a la **falta de independencia de los módulos**. Hay alto acoplamiento y baja cohesion.
El acoplamiento se refiere a como de interconectados están nuestros módulos entre ellos.

### Fragilidad

 Un sistema frágil es aquel que, al hacer un cambio en una parte, rompe otras partes inesperadamente.
 Esto sucede cuando los módulos están mal encapsulados, y los efectos de los cambios se propagan a otras áreas. 
 La falta de pruebas adecuadas o la **dependencia de implementaciones** específicas en lugar de abstracciones también contribuyen a la fragilidad.

#### Inmovilidad

La inmovilidad se refiere a la dificultad de reutilizar partes del sistema en otros contextos.
Esto ocurre cuando el código no está modularizado adecuadamente, y los componentes están tan entrelazados que es casi imposible extraer o reutilizar funcionalidades sin llevarse grandes partes del sistema.

> Los diseños se vuelven rígidos, frágiles e inmóviles a causa de incorrectas dependencias entre módulos.

> Un buen diseño tiene **ALTA COHESION** y **BAJO ACOPLAMIENTO**.

La cohesion es que por ejemplo una clase tenga un solo proposito. A mayor cohesion podemos reutilizar las clases.

Para logar un buen diseño se pueden seguir los principios SOLID.

---

## Single Responsibility Principle

La responsabilidad en un modulo/clase esta asociado a tener una razón de cambio.
Este principio se basa en que una clase debe tener una sola razón de cambio, es decir una responsabilidad

> Razón de cambio = Responsabilidad

#### Ejemplo clase Account

Supongamos una clase Account que representa una Cuenta de un banco o de alguna app de E-Commerce.

![](Attachments/Pasted%20image%2020240927171500.png)

En el diagrama se ve que la clase Account tiene 3 dimensiones distintas de cambio (por todo lo que usa) mas la dimension inherente a la entidad que se esta modelando. Es decir **4** posibles razones de cambio.

#### Ejemplo clase OrderController

```csharp
public class OrderController {
	//...
	public ActionResult  CreaterForm() {
		return View();
	}

	[HttpPost]
	public ActionResult Create(OrderCreateRequest request) {
		if (!ModelState.IsValid) {
			// View data preparation
			return View();
		}
		using (var context = new DataContext())
		{
			var order = new Order();
			// Create order from request
			context.Orders.Add(order);
		
			// Reserve ordered goods
			// .. Huge logic here ...
			
			context.SaveChanges();
			
			// send Email with order details for customer
			var smtp = new SMTP(); 
			// setting smtp.Host, UserName, Password and other parameters
			smtp.Send(client, order);
		}
	return RedirectToAction("Index");			
}
```

Enviar un correo electrónico, en realidad, no es una parte del flujo de creación del pedido principal. Incluso si la aplicación no logra enviar el correo electrónico, la orden sigue creándose correctamente. 
Además podría surgir una nueva opción en el área de configuración de usuario que les permite optar por no recibir un correo electrónico después de realizar un pedido con éxito, o incluso otros medios. 
Puede pensarse en un Helper para delegar el envío, o para desacoplar más un, un Observer o Eventos.

```csharp
public class OrderService { 
	public void Create(OrderCreateRequest request) { 
		// all actions for order creating here 
		using (var context = new DataContext()) { 
			var order = new Order(); 
			// Create order from request 
			context.Orders.Add(order); 
			// Reserve ordered goods 
			// ... Huge logic here ... 
			context.SaveChanges(); 
			
			//Send email with order details for customer 
			var smtp = new SMTP(); 
			// Setting smtp.Host, UserName, Password and other parameters
			smtp.Send(); 
		} 
	}
}

public class OrderController { 
	
	public OrderController() { 
		this.service = new OrderService(); 
	} 
	
	[HttpPost] 
	public ActionResult Create(OrderCreateRequest request) { 
		if (!ModelState.IsValid) { 
			return View(); 
		} 
		this.service.Create(request); 
		return RedirectToAction("Index"); 
	}
}
```


## Open Closed Principle

Las entidades de software (clases, modulos, funciones) deberian estar abiertas a extensiones pero cerradas a modificaciones.

"Abierto para extension" -> El comportamiento del modulo puede ser extendido. Podemos hacer que el modulo se comporte de maneras nuevas y diferentes a medida que cambian los requisitos.

"Cerrado para modificaciones" -> El código fuente es inalterable. Nadie puede hacer modificaciones.

Solución -> Abstracción

> Esto trae a una decision que tomar, si estar abierto a la extension e implementar una solucion mas compleja y estar preparado para futuras funcionalidades, o hacerlo simple (Keep it Simple) y no estar abierto a la extensibilidad.
> Si uno se anticipo también corre el riesgo que uno se adelanto pensando en una forma diferente a lo que realmente se necesitaba.


### Ejemplo figuras

```csharp
public class Rectangle 
{ 
	public double Width { get; set; } 
	public double Height { get; set; } 
}
```

```csharp
public class AreaCalculator 
{ 
	public double Area(Rectangle[] shapes) 
	{ 
		double area = 0; 
		foreach (var shape in shapes) 
		{ 
			area += shape.Width*shape.Height;
		} return area; 
	}
}
```



```csharp
public class Rectangle : Shape 
{ 
	public double Width { get; set; } 
	public double Height { get; set; } 
	public override double Area() 
	{ 
		return Width*Height; 
	} 
} 

public class Circle : Shape 
{ 
	public double Radius { get; set; } 
	public override double Area() 
	{ return Radius*Radius*Math.PI; 
	} 
	
} 
public double Area(Shape[] shapes)
{      
	double area = 0; 
	foreach (var shape in shapes) 
	{ 
		area += shape.Area(); 
	} 
		return area; 
}
```



## Liskov Substitution Principle

> "If it looks like a duck and quacks like a duck but needs batteries, you probably have the wrong abstraction".

Problemas con herencia. Se debe cumplir con la regla  de  "es un". 

```java
Vector vectorStack = new Stack(); 
vectorStack.addElement("one"); 
vectorStack.addElement("two"); 
vectorStack.addElement("three"); 
vectorStack.removeElementAt(1); 
System.out.println(vectorStack.size()); // throws Exception si quiere ser un Stack. (o sino quizas imprime: 2)
```

Un stack NO es un vector. 

> Cuando uno hereda de una clase, hereda todo, habrán modificaciones pero no puedo quedarme con una parte si y otra parte no de la clase padre. Para evitar problemas de utilizar métodos que no corresponden a esa clase heredada de la clase padre. 

Una solución para el caso del stack y vector, es utilizar una relación de agregación / composición, en donde el stack "usa" un vector internamente, en vez de "ser un" vector, pero lo usa en la implementacion encapsulandolo.

> Hacer un buen uso de la herencia



## Interface Segregation Principle

> Los clientes no deberian ser forzados a depender de interfaces que no usan.

En lugar de tener interfaces grandes y generales, es preferible dividirlas en interfaces más pequeñas y específicas para evitar que las clases que las implementen estén forzadas a definir métodos irrelevantes para su propósito.

### Ejemplo worker

```csharp
public interface IWorker
{
    void Work();
    void Eat();
}
```

```csharp
public class HumanWorker : IWorker
{
    public void Work() { /* ... */ }
    public void Eat() { /* ... */ }
}

public class RobotWorker : IWorker
{
    public void Work() { /* ... */ }
    public void Eat() { throw new NotImplementedException(); } // Los robots no comen
}
```

El problema aquí es que `RobotWorker` está implementando un método que no le es útil (`Eat`), lo que viola el ISP.

Se deberían dividir las interfaces en partes más específicas:

```csharp
public interface IWorkable
{
    void Work();
}

public interface IEatable
{
    void Eat();
}
```

Cada clase solo implementa los métodos que necesita, cumpliendo con el **Interface Segregation Principle**


## Dependency Inversion Principle

A: Módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.

B: Abstracciones no deben depender de detalles. Detalles deben depender de abstracciones.


### Ejemplo lampara botón

![](Attachments/Pasted%20image%2020240927230624.png)


El botón es inmóvil, es decir, si se quiere reutilizar para prender y apagar otra clase (como una tele) se debe llevar consigo también la implementacion o la lógica de la lampara, ya que depende de ella. 

> El boton que es algo generico depende de la lampara, se deberia desacoplar entre si. 

Hay dos implementaciones concretas que dependen entre si -> alto acoplamiento

Solucion:

![](Attachments/Pasted%20image%2020240927231023.png)

Ahora hay dos clases abstractas que dependen entre si y dos clases concretas que no dependen entre si -> Se agrega un nivel de abstraccion -> Alta cohesion.


```csharp
// Violación del DIP
public class Lamp
{
    public void TurnOn() { Console.WriteLine("La lámpara está encendida"); }
    public void TurnOff() { Console.WriteLine("La lámpara está apagada"); }
}

public class Button
{
    private Lamp lamp;
    
    public Button(Lamp lamp)
    {
        this.lamp = lamp;
    }

    public void Press()
    {
        lamp.TurnOn();
    }
}
```

Solucion: se puede introducir una **abstracción** (una interfaz o clase abstracta) para que `Button` no dependa de la clase `Lamp` directamente, sino de una abstracción que cualquier dispositivo pueda implementar.

```csharp
// Abstracción
public interface ISwitchable
{
    void TurnOn();
    void TurnOff();
}

// Implementación concreta de Lamp que sigue la abstracción
public class Lamp : ISwitchable
{
    public void TurnOn() { Console.WriteLine("La lámpara está encendida"); }
    public void TurnOff() { Console.WriteLine("La lámpara está apagada"); }
}

// El botón ya no depende de la clase Lamp, sino de la abstracción ISwitchable
public class Button
{
    private ISwitchable device;

    public Button(ISwitchable device)
    {
        this.device = device;
    }

    public void Press()
    {
        device.TurnOn();
    }
}
```


### Ejemplo Copiar

La clase `Copiar` está **acoplada directamente** a implementaciones concretas (`LectorTeclado` y `EscritorImpresora`). Esto hace que cambiar la fuente de entrada o el destino de salida requiera modificar la propia clase `Copiar`, lo cual **viola el DIP**.

![](Attachments/Pasted%20image%2020240927232851.png)
![](Attachments/Pasted%20image%2020240927232936.png)



```csharp
public class Copiar {
    private LectorTeclado lector;
    private EscritorImpresora escritor;

    public Copiar() {
        lector = new LectorTeclado();
        escritor = new EscritorImpresora();
    }

    public void Ejecutar() {
        int c;
        while ((c = lector.Leer()) != -1) {
            escritor.Escribir(c);
        }
    }
}
```

- **Dependencia directa** de las clases concretas `LectorTeclado` y `EscritorImpresora`, lo que rompe el DIP.
- Si quisiéramos usar otro tipo de `Lector` o `Escritor`, tendríamos que **modificar el código de la clase `Copiar`**.

Solución: 

Ahora, la clase `Copiar` depende de **abstracciones (interfaces)** en lugar de clases concretas. Esto permite cambiar fácilmente las implementaciones de lectura/escritura sin tocar el código de `Copiar`.

![](Attachments/Pasted%20image%2020240927232954.png)

```csharp
// Interfaces abstractas
public interface ILector {
    int Leer();
}

public interface IEscritor {
    void Escribir(int c);
}

// Implementaciones concretas
public class LectorTeclado : ILector {
    public int Leer() {
        return Console.Read();
    }
}

public class EscritorImpresora : IEscritor {
    public void Escribir(int c) {
        Console.WriteLine($"Imprimiendo: {(char)c}");
    }
}
```

```csharp
// Clase Copiar ahora depende de abstracciones
public class Copiar {
    private ILector lector;
    private IEscritor escritor;

    public Copiar(ILector lector, IEscritor escritor) {
        this.lector = lector;
        this.escritor = escritor;
    }

    public void Ejecutar() {
        int c;
        while ((c = lector.Leer()) != -1) {
            escritor.Escribir(c);
        }
    }
}
```


```csharp
// Ejecución
class Program {
    static void Main() {
        ILector lector = new LectorTeclado();
        IEscritor escritor = new EscritorImpresora();  // O EscritorDisco
        var copiar = new Copiar(lector, escritor);

        copiar.Ejecutar();
    }
}
```

- **Dependencia en abstracciones** (`ILector` e `IEscritor`), permitiendo cambiar las implementaciones sin modificar la clase `Copiar`.
- Fácil **extensibilidad**: Se pueden agregar nuevos lectores/escritores simplemente implementando las interfaces, sin afectar el código principal.

