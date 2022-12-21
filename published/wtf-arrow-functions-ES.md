# WTF Arrow functions?

> Lejanos son los años ya en que escribir funciones en Javascript consistía en manejar directamente los prototipos (te estoy mirando a ti Mootools) y saber con total seguridad como funciona **this**. Pero, una nueva era llegó al desarrollo web con el nacimiento de ES6 (EcmaScript 2015) que trajo nuevas luces a un aciago mundo.

Uno de los conceptos o características de Javascript que usualmente cuesta mucho manejar, tanto que se convirtió en una clásica pregunta de entrevista, es la idea de **this**. Tanto así que la comunidad empujo con fuerza para tener una mejor sintaxis para estas situaciones, de aquí nacen las **Arrow functions**.

```javascript
const holaMundo = () => 'Hola Mundo';
```

Recordemos que una función es:

- La mejor forma de encapsular y compartir lógica.
- Pueden ser ejecutadas en cualquier momento al llamarlas.
- Te permite re-utilizar código.

Las funciones en Javascript son “objetos de primera clase”, es decir, son objetos que pueden ser manipulados y transmitidos igual que cualquier otro objeto en tu programa.

Toda función es un objeto **Function.** Una función siempre retorna un valor utilizando `return`, si no se declara un `return` la función retornará  `undefined`.

Una función clásica es aquella que creamos utilizando la palabra `function`

```javascript
function holaMundo() {
	return 'Hola Mundo'
}
```

Pero esta puede ser re-escrita en forma de expresión

```javascript
const holaMundo = function() {
	return 'Hola mundo';
}
```

Lo que nos lleva al ejemplo inicial, una arrow function es una expresión, una forma simplificada de escribir nuestra función ya que:

- Puede ser escrita en una línea de código (si es lo suficientemente simple).
- no necesitas escribir la palabra clave **function**.
- no necesitas escribir **return**.
- no necesitas escribir las llaves **{}**

## ¿Cómo escribir y usar una arrow function?

Hay varias formas de escribir una arrow function, estas son básicamente variables de una expresión que permite simplificar la sentencia para diferentes casos de uso.

### No necesitas usar paréntesis.

Puedes no escribir paréntesis al rededor de los parámetros de una función, pero **solo cuando tienes un parámetro**.

```javascript
const holaMundo = name => `Hola ${name}`
```

En este caso, para ofrecer una sintaxis concisa, puedes no escribir los paréntesis al rededor de `name`, o dejarlos si gustas.

Ahora, si necesitas definir un parámetro por defecto, entonces debes forzosamente utilizar los paréntesis.

```javascript
const holaMundo = (name = "Matias") => `Hola ${name}`
```

Esta característica de evitar los paréntesis permite escribir de forma concisa técnicas como `currying`.

> **currying**: Esta es una técnica nacida de la programación funcional para convertir una función que tiene múltiples argumentos en una serie de funciones de un sólo argumento, es decir, por cada argumento se retorna una nueva función que recibe un nuevo argumento.

```javascript
function curriedSum(a) {
	return function(b) {
		return a + b
	}
}

// es lo mismo que
const curriedSum = a => b => a + b

// se puede usar

const sumar10 = curriedSum(10)

const resultado = sumar10(1) // 11
const loQueSeaMas10 = (n) => sumar10(n) // n + 10
```

En este ejemplo declaramos una función llamada `curriedSum` que simplemente realiza la suma de dos valores pero permite "almacenar" uno de esos valores para ser usado más tarde.

En la primera definición, lo hacemos con la forma clásica de funciones, no está mal pero es bastante “expresiva".

En el segundo caso, lo hacemos usando arrow functions y es mucho, mucho más concisa.

> ¿Cómo crees que funciona esta idea de `curry` y "almacenar" un valor para su uso más tarde?

### Retorno implícito

Algo que ya hemos visto en los ejemplo anteriores es el **retorno implícito**, una arrow function tiene la capacidad de retornar inmediatamente cuando el cuerpo de la función es sólo una expresión permitiéndote así mantener todo en una sola línea

```javascript
const adultUsers = users.filter(user => user.age > 18).map(user => user.name)
```

En el ejemplo podemos ver el uso del retorno implícito (junto con métodos de arreglo y concatenación de estos).

Asumiendo que tenemos un arreglo de `users` que al menos tienen los atributos `age` y `name` queremos obtener todos los nombres de los usuarios mayores de 18 años, para eso usamos `filter` y `map`   en conjunto.

Podemos ver el retorno implícito en cada llamada a la función “callback” recibida por `filter` y `map`.

Sin retorno implícito el ejemplo se vería:

```javascript
const adultUsers = users.filter(user => {
	return user.age > 18
}).map(user => {
	return user.name
})
```

Si bien el retorno implícito es genial, hay que tener cuidado con mantener la legibilidad del código, hay que mantener un balance entre expresividad y legibilidad.

Otras consideraciones que se deben tener con el retorno implícito son:

- **Solo funciona para expresiones sencillas**, si tu función tiene un cuerpo con más de una declaración entonces debes usar las llaves **{}** y la palabra **return.**
- **Cuidado si retornas un objeto**. Un objeto en Javascript se define utilizando llaves **{}** que son las mismas para definir el cuerpo de la función. Si lo que quieres es retornar un objeto entonces debes usar paréntesis al rededor

```javascript
// Esto fallará: Uncaught SyntaxError: unexpected token: ':'
const adultusers = users.map(user => { ...user, date: Date.now() })

// Esto si funciona
const adultusers = users.map(user => ({ ...user, date: Date.now() }))
```

### Las arrow functions son anónimas.

En Javascript existe el concepto de función anónima, el concepto viene nuevamente de la programación funcional en donde también son conocidas como: function literal, lambda function o lambda expresión). Es básicamente una función que no está “atada” a un identificador. Usualmente son utilizadas como argumentos para otras funciones (higher-order functions).

En los ejemplos anterior pudimos ver el uso de estas funciones anónimas, sobre todo en las funciones callback de los métodos de arreglo.

Todas las arrow functions son anónimas. Es posible asociar una función anónima a una variable - que no es lo mismo que nombrar la función - esto permite al engine de javascript crear cierta capacidad de inferir el nombre de la función al que puedes acceder mediante la propiedad `name`.

### No tienes que preocuparte por manejar `this`.

Quizá esta sea la mejor parte de utilizar arrow functions. No tienes que preocuparte por el uso de `this`.

En estas funciones el valor de `this` se mantiene sin cambios, es decir, siempre hace referencia al ámbito padre, esto es conocido como **lexical scoping,** esta es una forma de definir el *ámbito* de una variable para permitir que esta sea sólo accesible o llamada dentro del mismo bloque donde fue definida.

En un ejemplo

```javascript
document.querySelector('#submit').addEventListener('click', function() {
	console.log(this); //	<button id="submit">
})

document.querySelector('#submit').addEventListener('click', () => {
	console.log(this); // 	Window
})
```

Lo que hacemos es obtener un elemento del DOM identificado con el id `submit` y agregar un evento usando `addEventListener` , en este caso el evento `click`.
En el primer ejemplo usamos una función clásica, y al hacer `console.log(this)` nos muestra el botón al que `this` hace referencia, tal y como se espera.

Pero en el segundo ejemplo, el `console.log(this)` nos muestra el objeto `Window` ¿Por qué? Por que estamos usando una arrow function en donde `this` no se vuelve a "enlazar”, si no que hacer referencia al ámbito padre, en este caso, `document.window`.

> Las arrow functions no tienen sus propio *contexto this.* Lo heredan del valor de `this` que esté definido en el padre.

### No existen el argumento  `arguments` .

Recuerdas en una entrega anterior hablamos de este truco para obtener todos los argumentos de una función?. Bueno, no funciona con arrow functions.

```javascript
function withArguments() {
	console.log(arguments)
}

withArguments(1,2,3); // [1,2,3]

// usando arrow functions

const withArguments = () => {
	console.log(arguments); //Uncaught ReferenceError: arguments is not defined
}
```

Así que si de verdad necesitas el argumento `arguments`, entonces necesitas usar funciones clásicas. (Por cierto esto puede ser logrado de forma sencilla y más declarativa usando el operador spread como vimos anteriormente).

> Desafío:

> Dado el siguiente objeto: ¿Por que los métodos declarados en él no funcionan como se espera?

```javascript
const user = {
  name: 'Some Cool user', 
  age: 34
  followers: 1000,
  getFollowers() {
	return `${this.name} tiene ${this.followers} followers`
  },
  addFollower() { 
	this.followers++
	return `Gracias por unirte. Ahora somos ${this.followers}`
  } 
}
```

## ¿Cómo se relaciona con React?

Es un patrón muy común utilizar arrow functions dentro de JSX para renderizar un `map`, `filter` u otro método de arreglo.

```javascript
function AwesomeList({data}) {
  return (
	<ul>
	  {data.map(item => (
		<li key={item.id}>
		  <span>{item.name}</span>
		</li>
	  ))}
	</ul>
  )
}
```

Notar que en este ejemplo usamos el “retorno implícito” parar retornar el componente JSX `li`. No esta escrito en una sola línea pero cumple con ser una sola declaración.

## Conclusión

Usar arrow functions es la preferencia actual al escribir Javascript moderno, estas te permiten escribir métodos más concisos o simplificaciones de una linea mas legibles aprovechando las características de retorno implícito y el no uso de paréntesis.

Además te permite olvidarte de manejar el contexto `this` (algo que por cierto no vemos mucho en React).
Este tipo de funciones es muy usada como argumento para los métodos de arreglos como `.map()`, `.filter()` , `reduce()` y otros.

Y recuerda, si tienes preguntas relacionadas con este y otros temas de Javascript puedes abrir un issue en mi [repositorio AMA](https://github.com/matiasfha/ama)!

