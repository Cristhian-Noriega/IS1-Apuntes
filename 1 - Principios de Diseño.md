
> Deuda técnica: deuda de dar mas tiempo para arreglar el software mal hecho por llegar a ciertos deadlines

> regla del boy-scout: Leave the campground cleaner than you found it.
>  llegar a un bosque y dejarlo mejor de lo que estaba antes de que llegaran. Aplicar esta regla al código, refactorizandolo poco a poco. 


## Objetivos en un código

- mantenibilidad
- simplicidad
- claridad
- legibilidad
- flexibilidad

## Clean Code

### Nombres significativos y pronunciables

Es preferible tener nombres claros a comentarios

```
const d = 3; // tiempo transcurrido en dias
conts tiempoTranscurridoEnDias = 3;
```

Ademas es preferible que sean pronunciables

```
const yyyymmdstr = moment().format('YYYY/MM/DD');
const fechaActual = moment().format('YYYY/MM/DD');
```

Los nombres deben **revelar la intención** de lo que hacen, deben ser explicativos.

```javascript
function obtenerCeldas() {
	let lista1 = new Array();
	for (let i = 0; i < laLista.length; i++) {
		if (laLista[i][0] == 4) {
			lista1.add(laLista[i]);
		}
	}
	return lista1;
}
```

* ¿Qué tipos de cosas se almacenan en la Lista? 
* ¿Cual es el significado del item “CERO”? 
* ¿Cuál es el significado del valor 4? 
* ¿Para que se utiliza la lista que retorna ese método?

Podría mejorarse a:

```javascript
function obtenerCeldasTransitables() {
	let celdasTransitables = new Array();
	tableroJuego.celdas.forEach(function(celda){
		if (celda.esTransitable()
			celdasTransitables.add(celda);
		)
	});
	return celdasTransitables;
}
```

o con funcional: 

```javascript
function getPassableCells() {
	return this.board.cells
		.filter(cell => cell.isPassable())
 }
```


Usar **searchable names**, es decir, usar constantes con nombres claros, no dejar valores fijos. No dejar *números mágicos*.

![](Attachments/Pasted%20image%2020240926151901.png)

Usar una **única palabra por concepto**.

> Pick one word per concept. For Class names, method names.
> Use solution Domain names

Si usamos las palabras fetch, retrieve y get, debe haber un criterio para poder diferenciarlos si aparecen en el mismo ámbito. 

Usar nombres relacionados al dominio del problema.


### Funciones

*  Funciones/Métodos pequeños
	   Deberían tener menos de 8 líneas aprox. por función 
	   
* Hacer una sola cosa 
	  Single Responsibility Principle 
	  
* Un solo nivel de abstracción por función 
	  Identificar distintos niveles de abstracción 
	  Es la clave para reducir el tamaño de funciones y hacer una sola cosa por función 
	  
* Leer de Arriba hacia abajo 
	  Como un periódico
	  
* Switch Evitarlo 
	  Rompe la regla de solamente una cosa
	  
* Argumentos 
	  Uno es bueno, Cero es mejor
	  
* Flag 
	  `show(true)`
	  Es preferible usar polimorfismo, o crear nuevas funciones.
	  No es buena practica tener código para los dos escenarios en la misma funcion.
	   
```javascript
function createFile(name, temp) {
	if (temp) {
		fs.create('./temp/${name}');
	} else {
		fs.create(name);
	}
}
```

Pasa a: 

```javascript
function createFile(name) {
	fs.create(name);
}

function createTempFile(name) {
	createFile('./temp/${name}');
}
```

> Relacionado a que los nombres deben revelar una intención, un booleano no dice nada en este caso, no pertenece al dominio. Se reemplaza con un enumerado que dice que es lo que hace cada una de las opciones. 

* Don't Repeat Yourself (DRY principle) 
  
* The Principle of Least Surprise

```
Day day = DayDate.StringToDay(String dayName);
```

* The Boy Scout Rule. 
  Mejorar las cosas a medida que uno va haciendo refactoring trabajando con el codigo.


### Comentarios

> Es una herramienta mas, según como se use es bueno o malo.

Mala practica: 
En vez de encapsular la logica de si es candidato a beneficios sociales, se esta declarativamente explicitando. Si mas adelante en el código se debe chequear de vuelta esa codicion, habra codigo duplicado. Y si la condición cambia, se debe cambiar en cada parte del codigo.

```
// verifica si el empleado es candidato a 
// obtener beneficios sociales
if ((empleado.tipoEmpleado == EMPLEADO_PLANILLAS) && (empleado.edad > 65))
```

Solución: 

```
if (empleado.esCandidatoBeneficiosSociales())
```


#### Buenos comentarios

![](Attachments/Pasted%20image%2020240927150909.png)
![](Attachments/Pasted%20image%2020240927151154.png)

> Pese a que puede haber buenos comentarios, es importante considerar que cuando se escribe un comentario, este ya es parte del código, por lo que implica mantenerlo. Es decir actualizarlo si se cambia el código.

#### Malos comentarios

![](Attachments/Pasted%20image%2020240927151236.png)
![](Attachments/Pasted%20image%2020240927151255.png)



### Excepciones

* Usar excepciones en vez de codigos de error 
  `if (deletePage(page) == E_Ok) {....}`

* En general no retornar `Null`

Hay dos grandes ramas en las excepciones. *salvables* y *no salvables*. O mejor llamadas excepciones de *negocio* y excepciones de *aplicacion*.
Las salvables están relacionadas con la lógica de negocio, como por ejemplo el usuario debe ingresar su DNI, luego esta excepción llega al usuario en forma de mensaje en pantalla.
Luego, las no salvables, que tienen que ver con el sistema, no hay espacio, se cayo la base de datos, etc.


### Test unitarios

**F.I.R.S.T.**
* Fast
* Independent 
* Self-Validating
* Timely
  
Elegir buenos nombres para los tests

* SIEMPRE ESCRIBE CASOS DE PRUEBA AISLADOS 
* PRUEBA UNA SOLA COSA EN UN SOLO CASO DE PRUEBA 
* UTILIZA UN ÚNICO MÉTODO DE ASSERT POR CASO DE PRUEBA 
* UTILIZA UNA CONVENCIÓN DE NOMBRES PARA LOS CASOS DE PRUEBA 
  
  isAdult_AgeLessThan18_False 
   testIsNotAnAdultIfAgeLessThan18 
   IsNotAnAdultIfAgeLessThan18 
   Should_ThrowException_When_AgeLessThan18 When_AgeLessThan18_Expect_isAdultAsFalse Given_UserIsAuthenticated_When_InvalidAccountNumberIsUsedToWithdrawMoney_Then_TransactionsWillFail 
   LogIn_ExistingUsernameWithIncorrectPassword_ShouldReturnMessageWrongPassword 
   
 * UTILIZA MENSAJES DESCRIPTIVOS EN LOS MÉTODOS DE ASSERT 
 * MIDE LA COBERTURA DE CÓDIGO PARA ENCONTRAR CASOS DE PRUEBA FALTANTES


> Cualquier tonto puede escribir código que una computadora puede comprender. Buenos programadores escriben código que otros humanos pueden entender
> - Martin Fowler









  
  