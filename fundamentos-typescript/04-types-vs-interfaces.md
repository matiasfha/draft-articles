Cuarta entrega del micro-curso Fundamentos de Typescript, espero que te haya sido de utilidad hasta ahora.

> En caso de que no recuerdes, te suscribiste a mi newsletter para recibir el contenido de Fundamentos de Typescript :D 

En esta ocasión revisaremos Types e Interfaces. 

Una de las dudas mas comunes al comenzar a trabajar con Typescript es saber cuando utilizar `type` y cuando `interface`. Para resolver esto revisemos que significa cada una de estas "keywords".

> TLDR; Usa siempre `type` a menos que tengas una fuerte razón para usar `interface` :P 


## Type Alias 

La palabra clave `type` define un alias, una forma conveniente de hacer referencia a un tipo. En Typescript puedes definir el tipo de una variable o argumento anotando "inline", directamente en donde se declara dicha variable o argumento

```ts 

const name: string = "Matias"
let sociales: { twitter: string, youtube: string; tiktok?: string}

function subscribe({ date, name}: { date: Date, name: string}) {
  console.log("Suscribete a mi canal en Youtube")
}

const searchFunc = function(source: string, search: string): boolean {
    let result = source.search(search)
    return result > - 1
} 

```
> El simbolo `?` usado al lado de la propiedad `tiktok` es utilizado para denotar que dicha propiedad es opcional.

Pero este método o sintaxis puede ser confusa, pero sobre todo genera duplicación de código. ¿Que ocurre si quiero usar el tipo del objeto `sociales` en otro lugar?.  
Para resolver este problema es que utiliarás los alias de tipo. 

Usando `type` el código anterior se convertirá en:

```ts 
type Name = string;
type Sociales = {
  twitter: string;
  youtube: string;
  tiktok?: string;
}

export type Args = {
  date: Date;
  name: Name
}

export type SearchFunc = (source: string, search: string) => boolean

const name: Name = "Matías"

let sociales: Sociales 

function subscribe({ date, name}: Args) {
  console.log("Suscribete a mi canal en Youtube")
}

const searchFunc: SearchFunc = function(source, search) {
    let result = source.search(search)
    return result > - 1
} 

```

Quizá te pueda parecer un poco engorroso o incluso más largo de escribir, pero al mismo tiempo podrás reconocer que es un poco más legible, pero sobre todo tendrá mejor mantenibilidad. 

> Después de todo, pasamos más tiempo leyendo y manteniendo el código que escribiendolo.

Al dar nombre a los tipos, podrás utilizarlos en otras areas de tu código e incluso exportarlo para usarlo en otro módulo. 

## Interfaces 

Otra forma de declarar o definir un nombre para un tipo objecto (que contiene varias propiedades) es utilizar la palabra clave `interface`.

Podemos escribir el código anterior utilizando `interface` para los tipos de forma de objeto.


```ts 
type Name = string;
interface Sociales {
  twitter: string;
  youtube: string;
  tiktok?: string;
}

export interface Args {
  date: Date;
  name: Name
}

export interface SearchFunc {
  (source: string, search: string) => boolean
}

const name: Name = "Matías"

let sociales: Sociales 

function subscribe({ date, name}: Args) {
  console.log("Suscribete a mi canal en Youtube")
}

const searchFunc: SearchFunc = function(source, search) {
    let result = source.search(search)
    return result > - 1
} 

```

El funcionamiento de ambos trozos de código es el mismo, Typescript realizará el análisis de tipo de la misma manera ofreciendo el mismo nivel de seguridad.


Entonces, ¿Por qué existen dos formas y cual es la diferencia o ventaja de usar una u otra? 

## Types vs Interfaces 

Lo primero es reconocer que ambos métodos son tan similares que la gran mayoría de las veces podrás usar ambas forma de manera intercambiable, pero aún así existen algounas diferencias 

#### Extendiendo `type` e `interface`

Como revisamos en la entrega anterior, los tipos de datos en Typescript son un conjunto, y como tal, es posible extender los conjuntos, pero la forma de lograrlo cuando utilizas `type` o `interface` es conceptual y sintácticamente diferente.

Al utilizar `interface`, cuando quieres **extender** su funcionalidad lo que haces es hacer uso de conceptos de POO: "herencia". Una interface puede extender a una clase padre. Esto te permite "copiar" los miembros de una interface en otra. 

```ts 
interface User {
  name: string;
  email: string;
} 

interface AdminUser extends User {
  role: 'admin' | 'internal' | 'reader';
}
```

En este ejemplo la clase `AdminUser` tendra la propiedad `role` pero también las propiedades definidas en la interface `User`. 

Una interfaz puede extender multiples otras interfaces .


```ts 
interface User {
  name: string;
  email: string;
} 

interface AdminUser extends User {
  role: 'admin' | 'internal' | 'reader';
}

interface SuperUser extends User, AdminUser  {
  super: boolean;
}

const s: SuperUser = {
    name: "",
    email: "",
    role: "admin",
    super: true
}
```

Ahora, revisemos lo mismo pero utilizando `type`. En este caso lo que harás será ejecutar una intersección de tipos

```ts 
type User = {
  name: string;
  email: string;
} 

type AdminUser = User & {
  role: 'admin' | 'internal' | 'reader';
}

type SuperUser =  User & AdminUser  & {
  super: boolean;
}

const s: SuperUser = {
    name: "",
    email: "",
    role: "admin",
    super: true
}
```

También es posible "mezclar" ambos mundos, puedes intersectar un `type` con una `interface` o extender una `intercaface` desde un `type`

```ts 

interface User  {
  name: string;
  email: string;
} 

type AdminUser = User & {
  role: 'admin' | 'internal' | 'reader';
}

interface SuperUser extends AdminUser  {
  super: boolean;
}

const s: SuperUser = {
    name: "",
    email: "",
    role: "admin",
    super: true
}

```
Extender una interfaz o intersectar un tipo parece muy similar pero su diferencia yace en como se manejan los conflictos. Al usar `extends`, si intentas sobre-escribir una de las propiedades anotándola con un tipo diferente tendrás un error. Al usar `&` con `types` podrás sobre-escribir.



#### Solo `type` puede crear alias para tipos primitivos, uniones o tuplas:

 ```ts 
type MiNumero = number;

type StringOrArray = string | any[]

type Tupla = [string, number]
```

#### Solo `interface` posee la característica de "mezclado de declaraciones" (Declaration Merging)

El 99% de las veces que estes escribiendo una aplicación no necesitarás esta característica aunque si es probable que seas usuario de ella, ya que es utilizada mayormente por autores de librerías.
Esta característica permite al compilador de Typescript mezclar dos o más declaraciones dentro de un mismo módulo. 

```ts 
interface Dog {
  name: string;
}

interface Dog {
  breed: string;
}

interface Dog {
  owner: Person
}

class MyDog implements Dog {
  name = "Rocky"
  breed = "unknown";
  owner = {
    name: "Me"
  }
}

const myDog = new MyDog();
console.log(myDog) // {name: "Rocky", breed: "unknown", owner: { name: "Me"}}

```
Dado que cada una de las interfaces utilizadas en el ejemplo tienen el mismo nombre, Typescript las reconocerá y utilizará como si de una sola interfaz se tratase.

Ahora bien, si alguna de las propiedades de las interfaces que se uniran comparte el mismo nombre pero diferente tipo, Typescript emitirá un error. Pero, si dicha propiedad es una función, entonces Typescript unirá esta propiedad creando una sobrecarga de funciones.

```ts 
interface Dog {
  bark(volume: number);
}

interface Dog {
  bark(type: 'low' | 'high');
}

const dog: Dog = {
  bark: (volumeOrType) => volumeOrType
}

console.log(dog.bark(100)) // bark(volume: number) es utilizada
console.log(dog.bark("low")) // bark(type: 'low' | 'high') es utilizada

```
> Importante: Al mezclar interfaces que contengan funciones de igual definición, la función en la última interfaz declarada aparecer como función inicial en la interfaz mezclada, es decir, la última interfaz declarada tiene precedencia.

¿para que puede ser útil esta característica de las interfaces en Typescript?

Esta funcionalidad es muy utilizada por autores de librerías para proveer a sus usuarios formas de extender las definiciones de tipos de una manera segura, por ejemplo, revisa el siguiente código.

```ts 
/* Esta es MyAwesomeLibrary */
export interface WithReturn<Data> {
  data: Promise<Data>
  returnData: Record<string, unknown>
}

/* Ahora el código del usuario de la librería */

import { WithReturn } from 'awesone-library'

declare module 'awesome-library' {
  export interface WithReturn {
    foo: { extra: string }
  } 
}


// En otra parte de tu aplicación

import { WithReturn } from 'awesome-library'

const returnData: WithReturn<typeof data> 
/*
  returnData = { 
    data: Promise<typeof data>,
    returnData: Record<string, unknown>,
    foo: { extra: string
    }
 */
```

## Conclusión 
¿Cuándo usar `interface` o `type`?

Por lo general verás el uso de `interfacep`en la construcción de librerías, en otras palabras si lo que estás desarrollando es una aplicación, lo más probable es que puedas usar cualquiera de las dos formas de definr un tipo, pero, si lo que buscas es una forma de discriminar que utilizar:

Usa `type` cuando:

- Necesitas definir alias para tipos primitivos.
- Necesitas definir una tupla.
- Necesitas definir el tipo de una función.
- Necesitas deifnir uniones, uniones disjuntas, condicionales, mapped types, etc.

Usa interface cuando:

- Cuando quieres utilizar el mecanismo de “mezcla de declaraciones” (declaration mergin)
- Cuando no existe ninguna necesidad de usar type al crear un tipo para un objeto.


## Desafíos 

- Crea una interfaz que te permita definir un objeto con una propiedad llamada `log`. Esta propiedad es una función que recibe un string y retorna void. [Ir al desafío](https://tsplay.dev/w8BjPm)
- Crea una nueva interfaz llamda `Clearable`, debe tener una propiedad llamada `clear` que será una función sin argumentos que retorna void. Extendiende `Logger` usando `Clearable` como interfaz padre. [Ir al desafío](https://tsplay.dev/wRzYnw)
- Utiliza `type` para realizar los mismos ejercicios anteriores. El resultado deben ser  dos `type` donde uno extiende al otro. [Ir al desafío](https://tsplay.dev/NV7ynm)
- Crea una interfaz y un tipo que contengan propiedaes `readonly` y opcionales. [Ir al desafío](https://tsplay.dev/mqeOQm) 



