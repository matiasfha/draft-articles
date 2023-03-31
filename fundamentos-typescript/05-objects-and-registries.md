En este "capítulo" de Fundamentos de Typescript revisaremos dos tipos de datos Objectos y Registros.

> En caso de que no recuerdes, te suscribiste a mi newsletter para recibir el contenido de Fundamentos de Typescript :D 

`Object` y `Record` son las estructuras de datos que más utilizarás, la gran mayoría de los datos que se manipulan en 
una aplicación web tiene la forma de un objeto, por ejemplo el resultado de consumir una API.

Es importante mantener en mente el modelo de que los tipos son conjuntos y como tal, pueden *contener* otros tipos de datos.

Esto lo revisamos en la segunda entrega en donde revisamos 5 categorías de tipos, es en una de ellas: _Estructuras de datos_ en donde nos enfocaremos.

## Object 

Un tipo "object" es tal como lo imagines, la representación de un POJO (Plain Old Javascript Object).

```ts 

type User = {
  name: string;
  age: number;
  socials: {
    twitter: string;
    tiktok?: string;
    youtube?: string;
  }
}

```
Al igual que un objeto Javascript, esta representación en el nivel de tipos puede aceptar un número indefinido de propiedades, 
siendo cada una de ellas identificadas de forma **única** por un `string` pudiendo contener cualquier otro tipo. 

El tipo creado, en el ejemplo `User` es la representación de todos los objetos posibles que contengan dicha configuración.



