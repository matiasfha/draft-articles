Genial, ya estás en el tercer capítulo de este micro-curso sobre fundamentos de Typescript.

En este capítulo seguiremos revisando conceptos sobre los diferentes tipos que puedes utilizar en Typescript.

En la entrega anterior revisamos que existen 5 categorías de Tipos

- Primitivas
- Literales
- Estructuras de Datos
- Uniones
- Intersecciones

Pero hay dos tipos que no entran particularmente en esa categorización: **unknown** y **never**.

Lo primero que debemos aclarar es que los tipos son conjuntos, si conjuntos, tal como aquellos que revisaste en álgebra tiempo atrás.

> En realidad la teoría de conjuntos que es una rama del álgebra es la base teórica que fundamenta el uso de tipos en lenguajes de programación.

En el capítulo anterior mencionamos brevemente los tipos **uniones** e **intersecciones** y como ya debes notar, el hecho de que consideremos que los 
tipos son conjuntos está completamente relacionado con las definiciones de estos dos tipos.

En Typescript, un valor puede pertenecer a más de un sólo tipo, esta propiedad se llama **subtyping**, significa que un tipo puede incluir otros.

Pensemos en un ejemplo, imagina que tienes dos tipos literales `"Hola"` y `"Mundo"`, ambos valores literales son **strings** por lo que 
puedes decir que ambos son sub-tipos de **string**.

El hecho de que ambos literales sean sub-tipos de **string** y que **string** sea un super-tipo permite pensar en el concepto de asignabilidad de tipos.

> La idea de asignabilidad está presente en todo momento en Typescript, desde los errores mostrados - que indica que un tipo no es asignable a otro - hasta 
> las propiedades mas avanzadas como tipos condicionales, tipos mapeados, etc

Con esta idea en mente, ¿Por qué crees que no es posible asignar un valor **string** a una variable tipo **boolean**?.

**Por que `string` no es asignable a `boolean`.**

Hasta aquí ya es posible conceptualizar la idea de que existe una jerarquía de tipos, donde hay tipos que incluyen a otros, y tipos que no son asignables 

> Que no sean asignables también se puede decir que no se intersectan.

## El tipo `unknown`

¿Cuál es el tipo más alto en la jerarquía?.

![unknown hierarchy](https://res.cloudinary.com/matiasfha/image/upload/v1673575082/Screenshot_2023-01-12_at_22.57.19_ao1cwl.png)

Tal como la imagen muestra, `unknown` es el tipo que engloba o contiene a todos los otros tipos, eso implica que **todo** puede ser asignable a una variable 
anotada como tipo `unknown`.

Pero, `unknown` implica exactamente lo que su traducción dice: **desconocido**, Typescript desconoce la información de tipo por lo que nada puede 
hacer a la hora de proveer información para autocompletado o seguridad de tipos.

Además `unknown` es un tipo neutro, "se come a otros tipos". Si haces una unión de un tipo cualquiera con `unknown` obtendrás `unknown`.

Y viceversa, si haces una intersección de un tipo cualquiera con `unknown` obtendrás el tipo con que intersectaste, por que claro, dado que `unknown`
contiene a todos los tipos, la intersección de un tipo cualquier `T` con `unknown` será: `T`.

> Recuerda que una intersección tanto en teoría de conjuntos como en Typescript representa a todos los valores que simultáneamente pertenecen a ambas partes de la operación.

## El tipo `never`.

Si intersectas dos tipos que comparten propiedades, entonces tendrás una mezcla de ambos, pero que ocurre si intentas hacer al intersección de  
dos tipos que no tienen anda en común?

Usando la imagen anterior, ¿qué ocurre si intersectas `number` con `boolean`?

Recordando tus clases de teoría de conjuntos, al intersectar dos conjuntos que tienen nada en común obtendrás un conjunto vació, en el caso de los 
tipos, ese conjunto vació se llama `never`.

Este tipo representa valores que no deben existir, por ejemplo una función que no retorna valores tendrá como valor de retorno `never`.

`never` es de cierta forma el opuesto de `unknown` ya que `never` es subtipo de todos los otros tipos. `unknown` esta en el tope de la jerarquía y `never` al fondo. 

Si realizas la unión de un tipo `T` con  `never` obtendrás `T`. En cambio, si intentas intersectarlos, obtendrás `never`. 

## ¿Y que pasa con `any`?

Bueno, no tengo intenciones de mencionarlo mucho su existencia ya que es utilizar `any` es muy poco adecuado. `any` es una puerta de escape de la seguridad de tipos que  
ofrece Typescript, al utilizarlo le estás diciendo al compilador de TS que dicha variable o propiedad puede ser de cualquier valor así que no se preocupe de revisar que ocurre.

Al utilizar `any` rompes no sólo la seguridad de tipo y la inferencia de tipos provista por Typescript, si no, también el modelo mental de conjuntos .

En general te recomiendo no utilizarlo en absoluto a menos que sepas muy bien que estás haciendo y que su uso tenga una intención particular. 

## Desafíos 

No hay desafíos técnicos en esta entrega pero si te desafío a que me cuentes (respondiendo a este correo o en Twitter):
- ¿Estás usando Typescript? ¿En que proyectos?
- ¿Aún no? ¿Que te ha detenido?.
- ¿Hay algún tópico en particular que te interese revisar sobre Typescript?



