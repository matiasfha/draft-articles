En este "capítulo" de Fundamentos de Typescript revisaremos dos tipos de datos Objectos y Registros.

> En caso de que no recuerdes, te suscribiste a mi newsletter para recibir el contenido de Fundamentos de Typescript :D

`Object` y `Record` son las estructuras de datos que más utilizarás, la gran mayoría de los datos que se manipulan en
una aplicación web tiene la forma de un objeto, por ejemplo el resultado de consumir una API.

Es importante mantener en mente el modelo de que los tipos son conjuntos y como tal, pueden _contener_ otros tipos de datos.

Esto lo mecionamos en la segunda entrega en donde revisamos 5 categorías de tipos, es en una de ellas: _Estructuras de datos_ en donde nos enfocaremos.

## Object

Un tipo "object" es tal como lo imagines, la representación de un POJO (Plain Old Javascript Object).

```ts
type User = {
  name: string;
  age: number;
  socials: {
    twitter: string;
    tiktok?: string; // el uso de ? denota que esta propiedad es opcional
    youtube?: string;
  };
};

// Esto es equivalente a

type Socials = {
  twitter: string;
  tiktok?: string;
  youtube?: string;
};

type User = {
  name: string;
  age: number;
  socials: Socials;
};
```

Al igual que un objeto Javascript, esta representación en el nivel de tipos puede aceptar un número indefinido de propiedades,
siendo cada una de ellas identificadas de forma **única** por un `string` pudiendo contener cualquier otro tipo.

El tipo creado, en el ejemplo `User` es la representación de todos los objetos posibles que contengan dicha configuración.

Este tipo objeto existe en el nivel de tipos. ¿Pero como lo utilizas en tu código?

Recuerda que, la idea inicial de Typescript es poder dar confianza de que tu código hace lo que dice que hace, y esto lo hace revisando en tiempo de compilación el flujo y transformaciones de datos,
para esto, el compilador se apoya de la inferencia de tipos (proviniente del proceso de análisis estático) y de las anotaciones de tipo que tu le provees.

En este caso, para utilizar el tipo `User` en tu código, bastaría con utilizarlo como una anotación.

```ts
// esto es correcto ✅
// no se incluyen las propiedades tiktok ni youtube ya que son opcionales.
const user: User = {
  name: "Matias",
  age: 36,
  socials: {
    twitter: "https://twitter.com/matiasfha",
  },
};

// Esto está errado  ☠️
// se omite una propiedad requerida, socials
const user: User = {
  name: "Matias",
  socials: {
    twitter: "https://twitter.com/matiasfha",
  },
};
```

También podrias utilizar esta otra forma

```ts
const user = {
  name: "Matias",
  age: 36,
  socials: {
    twitter: "https://twitter.com/matiasfha",
  },
} satisfies User;
```

> `satifies` fue introducido en la versión 4.9 otorgando una nueva forma de obtener inferencia de tipos.
> En resumen, `satisfies` asegura que la variable este conforme a un tipo sin perder el tipo inferido por el compilador.
> Si quieres saber más de que se trata la palabra clave `satisfies` visita el siguiente artículo en mi web @TODO

Al utilizar la anotación `: User` en la variable `user` le indicas a Typescript que dicha variable es del tipo `User` y que no debería tener propiedades diferentes a las descritas en el tipo, pero esto es solo una verdad a medias.

Para Typescript, \*los objetos con mas propiedades son asignables a objetos con menos propiedades, pero cuando dicho objeto es definido "en linea", esto no es posible".

Ejemplo

```ts
const user: User = {
  name: "Matias",
  age: 36,
  socials: {
    twitter: "https://twitter.com/matiasfha",
  },
  city: "Talca",
};
// ☠️ Object literal may only specify known properties, and 'city' does not exist in type 'User'. (2322)
```

No es posible agregar la propiedad `city` al objeto definido "en linea".

```ts
const something = {
  name: "Matias",
  age: 36,
  socials: {
    twitter: "https://twitter.com/matiasfha",
  },
  city: "Talca"

}
const user: User = something; ✅
```

En otras palabras: \*Un tipo objeto es un conjunto de objetos con al menos las propiedades definidas".

Los tipo objeto en Typescript (al igual que en Javascript) son escenciales ya que permiten realizar diferentes manipulaciones y transformaciones sobre ellos, algunas de ellas

## Puedes leer sus propiedades

Puedes acceder a sus propiedades utilizando `[]` y asi crear un nuevo tipo

```ts
type Age = User["age"];

type AgeAndSocials = User["age" | "socials"]; // => number | { twitter: string; tiktok?: string; youtube?: string }
```

## Utilizar `keyof`

`keyof` te permite obtener una union con todos los nombres de las propiedades de un objeto, en forma de una union de "string literals".

```ts
type Keys = keyof User; // => "name" | "age" | "socials "
```

> Si quieres saber más sobre `keyof` te invito a leer [este artículo dedicado a su uso en mi web](https://matiashernandez.dev/blog/post/typescript-el-operador-keyof)

## Marcar propiedades como opcionales

Ya lo pudiste ver en el ejemplo que hemos usado hasta ahora, es posible marcar algunas propiedades como opcionales utilizando el modificador `?`.

```ts
type Socials = {
  twitter: string;
  tiktok?: string;
  youtube?: string;
};
```

Al utilizar este modificador le indicas a Typescript que es permitido que dicha propiedad sea obviada.

# Records

Son similares a los objetos, ya que también representan un conjunto de objetos. La principal diferencia es que un Record (Registro) no define el nombre de sus propiades de forma explicita, ni tampoco permite que el objeto definido tenga diferentes tipos de claves.
Al utilizar `Record`, todas las claves serán del mismo tipo.

```ts
type UserRecord = { [key: string]: string | number };

// Esto es equivalente

type UserRecord = Record<string, string | number>;
```

Esto se puede leer como, `UserRecord` es un objeto donde todas sus propiedades son `string` y pueden tener un valor tipo `string` o `number`

Ahora, el registro creado, es un objeto que puede tener múltiples propiedades con cualquier nombre, por lo que no es exactamente buena idea utilizar para definir a nuestro usuario.  
Si lo que quieres es restringir los posibles nombres de propiedades que se pueden utilizar en el Record, lo puedes hacer utilizando una union de "string literals"

```ts
type UserRecord = Record<"name" | "email" | "age", string | number>; // => { name: string | number; age: string | number; email: string | number }
```

En resumen, objetos y registros en Typescript son muy similares a su contraparte en el mundo de los "valores" (los objetos javascript). Ambos son fundamentales para construir conceptos y herramientas mas avanzadas como tipos utilitarios, tipos mapeados y otros. 

# Desafíos 

- Crea un objeto que tengoa como propiedades:  una función que reciba un registro de strings y retorne void, llamada `callback`, además una propiedad de tipo `Stack`. Debes definir el tipo `Stack` que contenga las propiedades length, pop, y push todas funciones.[Ir al desafío](https://www.typescriptlang.org/play?#code/PQKgUABBDyBGBWBTAxgFwM4QJ4QEooHsAnAE3UggoFobaqKBiJiAEUXQEMAzAW4IoDCRRBwgBXAHYQCCRKgIQANoo4BbDiQVx4EAI5jEEVIgkBzUcgKqFAByIEbAS0QkN7AFxQKEKuImiuSWRHAGepfUNhYNhRSQhhU0d0VHsIEkNkokczTBxheSIJQwA3AkcSABolFXVXCAADZA5lGOQAa3qq7183VQBDzElROwdnVxJqtQ1RZI52tMNURxsFeoBlVDmO928oVkRYdgWubMciCERFI2XVja36vQMISwljM1EVTBGnFzdMRRMplQAAsqisbFUcDYxOhgUYCK5MIEJMECEV0AA6ADaAElzs0FpxeAQALqUPZ7XwAswghaHeJyYj+c5xCRiVSIVK7KC+cEMgrMgCPonqABUHgAKGGiS5GRCqGwqYwLK5LFYQO7tACU3J8EGhsIug38EGRwTCj0iKEcsDOfT8og4RFM7JM8kmtVWjmMqgeEQg6E59vS13VYoeYBAwDAMdAEAA+omk8mkxBADwbgGn9wDwf4BYAkAQmRZ-ZEvgJlNl+MQKOx4AQQC8G4BCnf2h0w6ROEjOFyuMiQ7ug2A1m3m1YHWyNflDClMJgAl1lLFVLAqAZsjcrFXNEMCCIp0ucwMAawAeUUQAC8EDZqkO5wAfAHHKZ2ycmpbV4sbgzooY4mrJyZOY4ljPGIWAKMUzTEPqkGtig7pHPUl7XvU+41jiEgkIBHDoQoHDoDwr6BkQ9pxJqbQLBe7LXpQYCoFgNiGKRx5nhRV6cne54AN4UNSQLAp4ADcFDggJQkwnxED8QAvjGtH0TACDMVxXjSTG+4gKW5bJhAorsBgGmaYmlbRo4CrEKgRh0YYHEQAAovozRVDZAAe9FoFUAByBCoAAghIOCSaa9iqBAADkAACsmIFQyDAs0PHsMAYhLIo6AhapkXPLhRznliFDOa5qCHnZYjNIepFYgAROCFUklUEpamed6IWxN4VHlLmwUV9mKGVg5tJVPEgjVdUNaeTWUS1bVQPlnXFaV5VVWJw0QBK3ryp4zVEKNd6lOUN6te1BVdSVPXaJVTQtFsy0SkQnj4JYpCHpk2SmFUz1mDe20QLtEz7VNtkdWgx2lWdFWzO0y2kX9h2zd1h6g+DbQ1QNgJDbVI7tCjNLAjV+1gCSsYgOpNkogYrxEB8MzbmI5pSBw+iOBAwKoKgNjoO4B4YOuWAYukxTAAA6gACqYNgADLBVWQA) 

