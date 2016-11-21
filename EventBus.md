Tarea
==============================

----

## Aplicaciones Moviles

#### Carlos Fernando Torres Luna

##### cf.torresluna@gmail.com 

--- 
---



# EventBus.
EventBus es es una herramienta cuya finalidad es la de lanzar y capturar eventos entre diferentes clases. Está basada en el patrón bus de eventos y su implementación consiste en que uno o varios objetos se suscriben a un canal a través del cual escuchan, y otro, en el momento en el que suceda algo que desea notificar, lanza un evento a través de este canal y todos aquellos que estaban escuchando reaccionarán haciendo aquello que tenían programado al suscribirse a éste. EventBus no solo nos facilita la tarea de notificar a los suscriptores acerca del evento si no que nos permite enviar información asociada a ese evento a través del canal y decidir en qué hilo se ejecutará la respuesta.






## Ventajas y desventajas de EventBus

Un bus de eventos es una herramienta muy útil y potente, pero el abuso de esta forma de trabajar y las malas prácticas al hacer uso de esta herramienta pueden convertirla en un arma de doble filo. Veamos a lo que nos referimos:

* **Flexibilidad:** es algo que se intuye en el momento en el que comprendemos su funcionamiento. Con EventBus se pueden realizar numerosas funcionalidades, incluso generar toda una forma de trabajar basada en estos eventos. Sin embargo, esto puede ser contraproducente, ya que EventBus te permite abstraerte de las herramientas de paso de mensajes y de notificaciones que brinda Android y el lenguaje en general; y, si se abusa, se puede generar una fuerte dependencia de esta herramienta, pudiendo incluso convertirse en el núcleo de nuestra aplicación. Esta no es una buena forma de trabajar, ya que estos eventos no son sencillos de depurar, ni es fácil ver cómo se propagan ni dónde se están capturando y cuándo, aunque posteriormente veremos algunos trucos que sí facilitan esta tarea.

* **Simplicidad:** es una ventaja clara que nos brinda en este caso EventBus como herramienta, ya que en solo tres sencillos pasos ya podemos hacer uso de esta herramienta. En cualquier parte se llama al método post de la instancia de EventBus y se propaga un evento. En cualquier parte de la app nos suscribimos en el momento que deseemos y, generando un método con una simple anotación, reaccionamos a dicho evento especificando en qué hilo se da la respuesta.

* **Agilidad:** gracias a los beneficios mencionados anteriormente, ahorramos muchas líneas de código, clases y lógica de interacción que, al fin y al cabo, se traduce en un menor tiempo de desarrollo. Pero haciendo hincapié en lo anterior, lo que parece ahorrarnos trabajo de desarrollo, si no se hace correctamente y se abusa de ello, se puede convertir en tiempo de depuración; ya que, en el momento en el que algo falla, podríamos desear haberlo hecho de la otra forma.

## Precauciones de EventBus

Aunque seguramente habrá muchas otras precauciones a mencionar sobre esta herramienta, aquí voy a exponer las que considero más importantes y las que solventan los errores más frecuentes.

* Un bus de eventos no es un canal de comunicación, sino una forma de notificar eventos entre los suscriptores y los generadores; por lo que su finalidad no es la de servir como herramienta de paso de mensajes. Aunque nos facilite mucho esta tarea, hay que ser consecuente con lo que se envía a través del canal junto con el evento y no saturarlo de datos.

* Un error muy común es el de estar suscrito a muchos eventos y controlar qué evento es el que se está recibiendo en función de la información que se nos envía. Los datos asociados al evento contienen información complementaria a este, y no para distinguirlo de otro. Es decir, la clase que encapsula los datos es la que lo define, pero no los datos que contiene dentro. La clase A define el evento A, pero los atributos de ésta no distinguen si el evento es el A, el B o el C. Como veremos más adelante, hay una clase que distingue cada uno.

* El lanzamiento incontrolado de eventos es algo muy común, hay que elegir bien cuáles se lanzan y dónde, y no llenar nuestra aplicación de suscripciones, ya que un error en estos casos puede volverse insostenible y una maraña de éstos puede convertir nuestra aplicación en un infierno.

* Hay que tener cuidado, no tanto de en qué hilo se lanzan los eventos, sino en el de la respuesta; ya que, por defecto, éstas se ejecutan en el mismo hilo en el que se lanza el evento. Además, si la acción programada es, por ejemplo, la de pintar, tendremos un error si el hilo en el que se lanzó el evento no es el principal, ya que estaremos pintando en un thread que no es el main.

* Como última precaución, es importante destacar el hecho de desuscribirse en el momento oportuno; ya que, muchas veces, es probable que se capture un evento en alguna clase que aún no lo ha hecho pero no está activa, y que capture un evento realizando sus correspondientes acciones de respuesta sin los recursos que necesitan.