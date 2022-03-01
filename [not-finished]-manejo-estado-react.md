# Manejo de estado en React

¿Qué es el estado y por que debemos "manejarlo"?

React es una librería para el construir interfaces de usuario mediante el uso de componentes que interactúan entre si y con el entorno para desplegar información y permitir interactivdad con el usuario, es en este punto donde el **estado** comienza a aparecer.

Dado que las aplicaciones web no son simplemente datos renderizados en pantalla como un sitio estático (que también pueden ser desarrollados con React) necesitamos de un medio de informar de cambios en los datos o eventos tanto de usuario como externos que modificarán lo que el usuario ve en pantalla, es decir, el **estado** son datos que cambian en el tiempo, un flujo constante de información que modifica la interfaz o las posibilidades de tu aplicación.

El **estado** también puede ser definido como una representación programática de lo que el usuario ve en un determinado momento, esto tiene un directa relación con la idea de que en React la interfaz es una función del estado `UI = f(estado)`

## ¿Por qué el manejo de estado es un problema a resolver?

Es un problema difícil, tanto que hay una rama de ciencias de la computación para esto: State Machines (o más correctamente Finite State Machines).

El mantenimiento del estado/conocimiento de una aplicación que depende de sus entradas. La complejidad subyace en la coordinación de los tipos de estado y eventos.

El estado está formado por diferentes "fuentes":

💅 UI State –> Estado usado para controlar las partes interactivas de la app

🖥 Server Cache State –> Estado del servidor - asíncrono, obsoleto.

⛽️ Resource State -> Eestado de los recursos como archivos, imágenes y otros.

⌨️ Form State –> Los diferentes estados de un formulario

🛳 URL State –> El estado manejado por el navegador.

## ¿Cómo podemos manejar el estado?

Una de las formas iniciales para manejar el estado en la gran mayoría de las aplicaciones en los primeros años de las web 2.0 fue el uso del **estado global.** Una solución super conveniente pues al tener un objeto que representa el estado de toda la aplicación accesible desde cualquier parte de la misma hacía mucho sentido y era fácil de leer.

Pero este método rápidamente toco techo y encontró sus primeros problemas: Mantenibilidad, Performance (múltiples re-renders a causa de componentes inter-dependientes), escalabilidad y complejidad.

Entonces comenzó a nacer nuevas preguntas como: **¿Qué es realmente estado global?.** La idea es que no todos los datos son realmente globales a la aplicación ya que no todos estos datos son utilizados en todas partes. Algunos ejemplos:

- Datos de usuario ✅
- Notificaciones ✅
- Formularios ❓
- Menus ❓
- Modales ❓

Es en este punto donde comienza a clarificarse que existen diferentes naturalezas del estado y diferentes formas de manejarlo y manipularlo.

## ¿Qué naturalezas tiene el estado?

Podemos dividir o categorizar el estado en dos grandes grupos

💅 UI State

🖥 Server State

¿Por qué estas dos categorías?

Estas categorías engloba la mayor parte de las fuentes del estado.
- UI State: se refiere a ese estado que sólo existe durante la sesión actual, es decir, desaparece al refrescar el navegador. Puedes incluir en esta categoría: Modales, formularios, estado de botones, etc

- Server States: Se refiere al estado que tiene persistencia, por ejemplo en una base de datos. Este estado no "le pertenece" a la aplicación web, es asíncrono y esta potencialmente desactualizado (ya que puede ser modificado en su fuente por múltiples clientes), y normalmente está relacionado con el manejo de una Cache.

Ambas categorías son fundamentalmente diferentes, lo suficiente como para ameritar diferentes estrategias para su manejo.

En el caso del estado del servidor (Server State ), ya que la podemos considerar una cache, debemos considerar muchas variables y técnicas. Algunas soluciones disponibles pueden ser Apollo Client (que contiene un sistema de normalización de cache y además está disponible para diferentes frameworks) soluciones como Relay

