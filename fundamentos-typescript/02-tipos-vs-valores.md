En la primera entrega de este micro-curso revisamos en breve el por qué utilizar Typescript y como instalarlo, hoy seguiremos ahondando en los conceptos que necesitas para ser todo un mago de Typescript.

# El lenguaje de tipos: Tipos vs Valores

Typescript es un lenguaje algo extraño, existe en diferentes mundos que se entrelazan mientras escribes código: Valores y tipos.

Estos mundos - que incluso puedes considerar diferentes lenguajes - tienen diferentes objetivos y diferentes formas de "funcionar".

El lenguaje de los valores es aquel que escribes para que sea ejecutado en producción, en general estamos hablando de Javascript válido ya que todo código Typescript (sin las anotaciones de tipo) es código Javascript valido.

 
Por el otro lado, el lenguaje de los tipos es el que te permite agregar anotaciones al código Javascript para definir, por ejemplo, que tipo de valor son aceptados como argumentos de una función

```ts 
function mul(a: number, b: number) {
  return a * b
}
```
Estas anotaciones son las que te permitirán utilizar el poder de la inferencia de tipos de Typescript para obtener mejor sistema de auto-completado, documentación y "seguridad" en que tú código hace lo que crees que hace (recuerda que los tipos son también una forma de tests).

## El lenguaje de tipos 

Es en este mundo en donde el verdadero poder de Typescript reside. El sistema de tipos es poderoso cuando comienzas a utilizar los diferentes conceptos disponibles para poder realizar "Programación al nivel de tipos".

Typescript te permite por ejemplo crear funciones en el nivel de tipos usando el concepto de **generics**, crear condiciones, variables, etc.

Es cierto que la sintaxis puede ser un poco extraña pero, es sólo eso, sintaxis, una forma de expresar conceptos y funcionalidad por medio de palabras claves y signos.

Este lenguaje de tipos es también "extraño" por que es puramente funcional (cómo Haskell), es decir, es totalmente inmutable y sólo funciona utilizando funciones y recursión. Esto implica que en este nivel no existen muchas características pero tiene todo lo necesario para considerarse un lenguaje "Turing Completo".

- Bifurcación de código: La posibilidad de ejecutar diferentes "caminos" de código, es decir un bloque condicional.
- Asignación de variables: La posibilidad de declarar variables para ser utilizadas en una expresión.
- Funciones: Trozos de código reutilizables, que en este caso son 100% puras.
- Ciclos: Puedes iterar sobre los datos utilizando recursión.


Pero, al ser un lenguaje 100% funcional también existen restricciones

> Las restricciones de los lenguajes funcionales pueden sonar arbitrarias y complejas si vienes de un lenguaje imperativo y procedimental pero están ahí para ayudarte y mejorar tu código.


- No puedes mutar el estado: Las variables son inmutables.
- No hay "side effects": En este caso implica que no hay entradas ni salidas. Typescript (en el nivel de tipos) no puede escribir en la consola o ejecutar una llamada a una API.


## Categorías de Tipos

Typescript, como todo otro lenguaje de programación, ofrece cierto número de tipos por defecto con los cuáles podrás construir la lógica que requieres para tus soluciones. 
Pero vale mantener en mente que, cuando trabajas en Typescript tus datos y variables son **tipos**, es decir, crearás funciones/lógica/programa cuyos datos de entradas serán tipos y generaran nuevos tipos.

¿Cuáles son los tipos disponibles?

- Primitivas
- Literales
- Estructuras de Datos
- Uniones
- Intersecciones

Revisemos, en resumen, cada uno de estos tipos. 

### Tipos primitivos

Estos tipos han estado presentes en tu día a día desde siempre, y no son nada nuevo ni inventado por Typescript: `number`,`string`,`boolean`,`symbol`, `bigint`, `undefined`,`null`.

Como vez, los tipos primitivos de Typescript son a la vez "casi" todos los tipos primitivos existentes en Javascript, casi, por que objetos y funciones no están en esta lista ya que pertenecen a otra categoría.

Si bien estos tipos pueden expresar gran parte del código de una aplicación, no son suficientes ya que normalmente necesitaras tipos más complejos para representar tus soluciones.

### Tipos literales

Aquí **literal** significa exactamente eso, el **tipo** es es exactamente el valor que ves.

```ts 

const diez: 10 = 10;
const hola: "hola" = "hola";
```

Es decir, la variable `diez` tiene como tipo al número `10`, entonces sólo podrá contener como valor el número `10`. Lo mismo con la variable `hola`, el único valor que puede tener es el string  `hola` dado que la anotación de tipo indica que es el string literal `hola`.

### Estructuras de datos 

Aquí es donde comenzamos a encontrar mayor utilidad en los tipos ofrecidos nativamente por Typescript, ya que permiten modelar de mejor manera tus requerimientos. En esta categoría encontrarás objectos, registros, arreglos y tuplas.

- **Objetos** Es un tipo que describe la "forma" de un objeto como un conjunto finito de pares clave:valor.
- **Registros** Muy similar a un objeto, pero describe la forma de un objeto con un número desconocido de propiedes en donde todas ellas son del mismo tipo.
- **Tuplas** Permiten describir un arreglo de un tamaño definido.
- **Arreglos** Tal como su nombre lo indica, describe un arreglo de tamaño desconocido pero cuyos valores son del mismo tipo.

```ts

type Objeto = {
  name: string;
  age: number;
}

type Registro = { [key: string]: unknown} // Un objeto cuyos nombres de propiedades son strings pero su valor es desconocido
type Registro2 = Record<string, unknown> // Resultado identico a la linea anterior

type Tupla = [string, number, boolean] // Un conjunto finitio de 3 elementos 

type Arr = Array<string> // Un arreglo infinito de sólo strings, también puede escribirse como string[]


```


### Uniones e Intersecciones

Estos dos tipos ofrecidos por Typescript son similares y opuestos. Ambos conceptos provienen de la teoría de conjuntos y sólo existen en el nivel de tipos. Son importantes ya que permiten expresar diferentes patrones y modelos.

Por ahora sólo revisaremos su sintaxis y brevemente su significado, pero ahondaremos más en una siguiente entrega.

```ts 

type Union = X | Y 

type Intersection = X & Y 

```

La primera linea de este trozo de código puede leerse cómo: _"El tipo Union tendrá el tipo de X o el de Y"_. La segunda linea es lo opuesto, _"El tipo de Intersection será simultaneamente X e Y"_.

## Desafíos

Por ahora llegaremos hasta aquí revisando los tipos de datos para saltar a algunos desafíos en donde utilizarás lo que has aprendido hasta ahora y te forzarás a pensar un poco más allá.

Todos los desafíos de esta sección tienen la misma instrucción. **Agrega las anotaciones necesarias a los argumentos de la función para satisfacer los requerimientos descritos**.


1. La función `saltar` puede recibir un argumento que solo puede tener los valores _"arriba"_ O _"adelante"_. [Ir al desafío](https://tsplay.dev/N72kGw)
2. La función `getKeys` acepta cualquier objeto cuyas "claves" (o nombres de las propiedades) sean strings, y no le importan el valor de ellas. [Ir al desafío](https://tsplay.dev/NlLGrN)
3. La función `parseUser` es utilizada para obtener cierta información de un usuario, recibe como argumento un objeto que se compone de `id`,`name`, `age` y `location`. A su vez, `location` es un objeto que describe `country` y `city`. [Ir al desafío](https://tsplay.dev/WPzPYN)
4. La función `sumArr` recibe un arreglo de números. [Ir al desafío](https://tsplay.dev/NV7dvm)


> El enlace a las soluciones de cada desafío lo encontrarás directamente en el desafío. Te recomiendo revisar su contenido sólo después de intentar resolverlo.

