
![](Attachments/Pasted%20image%2020240912205402.png)

> Agarrar un monolito (un solo bloque) y partirlo en cosas mas chicas (micro-servicios)


Ventajas: 
* Escalar independientemente. 
* Si tengo servicios chicos, puedo implementar cada servicio en distintos lenguajes. Habra distintos problemas entonces necesitare distintos paradigmas, entonces es cómodo tener servicios en distintos lenguajes con distintos lenguajes específicos para resolver cada problemática. 
* Cada servicio ataca un problema con un scope mas chico. -> si lo quiero reemplazar no tengo que romper todo el sistema.

Desventajas: 
*  Difícil de debuggear -> mas pedazos
* Complejiza al establecer una comunicación entre los servicios. Lidiar con un sistema distribuido. 


![](Attachments/Pasted%20image%2020240928204857.png)


API Gateway -> controla el acceso. Punto de partida. Conoce donde están todos los servicios y delega los requests.

Nadie puede acceder al sistema sin antes pasar por el API Gateway. Controla el acceso.

El API Gateway conoce donde están todos los servicios y delega el request que recibió al servicio correspondiente.



### Micro Fronteds

Misma idea a micro services pero orientado al front-end. Parto una capa de presentación y la parto en slices, en frontends mas chicos.

![](Attachments/Pasted%20image%2020240912210502.png)

