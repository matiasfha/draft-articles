Bienvenido a este nuevo microcurso de **Mycrobytes**: Fundamentos de Typescript.

El objetivo de este micro-curso es acompañarte en tu camino de aprendizaje de Typescript guiándote por las bases que te permitirán se eficiente al trabajar con Typescript.

Por medio de una serie de artículos, demos, ejemplos y desafíos aprenderás los conceptos claves que te permitirán crear aplicaciones con seguridad de tipos aprovechando todo el poder de Typescript.

Este es un microcurso orientado a quienes están comenzando con Typescript pero que si tienen experiencia previa con Javascript.
Te dará un entendimiento sólido de los fundamentos del sistema de tipo y como utilizarlo en tu día a día resolviendo problemas o implementando nuevas características en tus aplicaciones.

Comencemos con el primer desafío, mira este pequeño trozo de código:

> Los desafíos y demos de este microcurso están disponibles en el playground de Typescript. Esta web es básicamente un editor de texto tal como VSCode y te puede mostrar los errores de tipo, además de mostrarte el resultado de la compilación de Typescript (código Javascript).
> Revisa [este video para una revisión rápida del playground.](https://www.youtube.com/watch?v=Em_MltFTSTY)

```ts 
type HelloWorld = any 

// Debes hacer que esto funcione 
type test = Expect<Equal<HelloWorld, string>>
```
Haz click en [realizar el desafío](https://www.typescriptlang.org/play?#code/PQKgUABBAMCMEFoIAkD2AbAhhAsgVwDsATVSRBCysgYlogBEBTAZ0wDMBb0qMtLXQiQCEZAKLMALqmYQiLdlxl5WBCI3QRmAS0mMAttjkQJWgA7SIpzACdsElvZvHlmjHgDGW1ARYA6MmKqDoyy8pyooQBGLBDumHqRWk7qxmYRmMyonpgkENgABnyY+MSo+ZY22AAWmO6M1hAAjnghKfaSFcyM-jwgwGADoBAA+qNj42MQgDwbgNP7gPB-gLAEgEJkcxAAgs0cEABudnixAM9EWgDmERMXoxB9AxIAnqYhyOroqADqqNboRBAAvHkEO4QYDABiMaIyOIJJINFghXavBr5TCA8rmWEaEzmWKfayMdxSAZgIaXC4QAAqDhkpIm136Wj06IkxgeIQA3hBRM1MOgADScgAejwJ-IAcqgJGtARAAL4QNjWVB6CAAcgAAvdHgh3DV0OhGAQTixgHgTOhmCrbqzYhkYv8ANpiIX4iQAHnFksBrueeven2+AD4A7yncK3Vy8DzvS8-V8iPzJNYtIagyGALpgIA) para comenzar.

> No te preocupes si no entiendes lo que la última linea de código implica, la idea aquí es que puedas agregar el tipo correcto a `HelloWorld` para que el test de `true`.


> Puedes ver la solución del [desafío en este enlace](https://www.typescriptlang.org/play?#code/PQKgUABBAMCMEFoIAkD2AbAhhAsgVwDsATVSRBCysgYlogBEBTAZ0wDMBb0qMtLXQiQCEZAKLMALqmYQiLdlxl5WBCI3QRmAS0mMAttjkQJWgA7SIpzACdsElvZvHlmjHgDGW1ARYA6MmKqDoyy8pyooQBGLBDumHqRWk7qxmYRmMyonpgkENgABnyY+MSo+ZY22AAWmO6M1hAAjnghKfaSFcyM-jwgwGADoBAA+qNj42MQgKDkEOIS2DH8megeWgDPqhNboxB9AxIAnqYhyOroqADqqNboRBAAvJoS1loEAOYQA2BD21sQACoOGS-Ca7fpaPTmawSYxHEIAb1mzUw6AANLMAB7HdwSdEAOVQEgAggQDhAAL4QNjWVB6CAAcgAAodjgh3DV0OhGO8WMA8CZ0Mx6fs4bEMjFHgBtMRYxg4gA8BOJpPlp05l2utwAfFrUTLsRJ5aJkehVWcNTciOjJC93jq9QBdMBAA)

Los tópicos que revisaremos serán:

- ¿Por qué usar Typescript?
- El lenguaje de tipos: Tipos vs Valores
- unknown y never
- ¿Qué son types e interfaces?
- Objetos y Registros.
- Tipos Utilitarios
- Arreglos y Tuplas.
- `keyof`.
- `extends` restricciones y herencia.
- Genéricos
- sobre carga de funciones
- type guards y type predicates



## ¿Por qué usar Typescript?

Si estás leyendo esto es por que seguramente, ya estás convencido de usar Typescript para tus proyectos, pero, ¿por que?. ¿Qué razones mueven a la industria a adoptar este lenguaje como estándar a la hora de construir nuevos productos?

Durante los últimos años el uso de tipos en Javascript mediante Typescript ha tomado mucha fuerza yendo desde simples anotaciones a un uso complejo en diferentes librerías transformando a Typescript en un lenguaje de programación poderoso y complejo.
Es posible revisar el código de muchas librerías de código abierto y ver la impresionante cantidad de código y la masiva complejidad que este tiene, puede ser intimidante al principio. 

Por lo general las librerías tienen este tipo de código casi esotérico por que requieren ser muy genéricas para adaptarse a los diferentes usos que sus usuarios requieren, un [ejemplo es este de [tanstack-query](https://github.com/TanStack/query/blob/main/packages/react-query/src/types.ts). En este archivo puede ver un listado de tipos que apuntan a ser flexibles pero seguros utilizando diferentes características de Typescript como: *Genéricos*, *tipos condicionales*, *tipos mapeados*, etc.

¿Entonces, por que escribir algo así?

Por que el uso de tipos es genial!, genial por varias razones

- Sirven como documentación del propio código.
- Permiten que los editores de código hagan mejor uso de auto completado y sugerencias más "inteligentes", mejorando así la productividad y eficiencia.
- Pero por sobre todo, por que el uso de tipado es un tipo de *test*. Si, gracias al uso de un sistema de tipos podrás encontrar errores, bugs y "typos" mucho antes de que lleguen a tus usuarios.


Lo primero que debes hacer para ser eficiente y fluido utilizando Typescript es conocer el sistema de tipos del lenguaje y reconocer que el propio sistema de tipos puede actuar como un lenguaje. En efecto, el sistema de tipos de Typescript es [*Turing Complete*](https://es.wikipedia.org/wiki/Turing_completo)


Lo primero que comenzarás a realizar con Typescript es agregar anotaciones de tipo a tus funciones y objetos, revisemos un ejemplo y un desafío 

Imagina que tienes la siguiente función en Javascript:

> Recuerda, Typescript es un super-set de Javascript, lo que implica que todo el código Javascript es *también válido como código Typescript*

```ts
function mul(a, b) {
    return a*b
}

```

Una sencilla función que toma dos variables y las multiplica, nada complejo verdad.
Ahora recordemos que Javascript es un lenguaje de tipado débil y que permite la coerción de tipos, es decir, esta función `mul` que está pensada para multiplicar dos números, en realidad acepta cualquier valor.
```ts
mul(10,2) // 20 
mul("10",2) // 20 
mul("10","2") // 20
mul(10, "somethiing") //NaN 
mul(10,false) //0

```

Como vez, el comportamiento es inesperado, la función puede tomar cualquier valor y realizar la coerción de tipos para poder operar, algo que no es lo esperado.
¿Cómo puedes evitar esta situación?. En Javascript tendrías que escribir algunas condiciones para revisar que los argumentos cumplen su cometido.

```ts 
function mul(a,b){
  if(!isNaN(a) // !isNaN(b)) {
    return a * b 
  }
  throw "Ambos argumentos deben ser números"
}
```

Si bien esto no agrega demasiada complejidad al pequeño trozo de código, si se siente innecesario sabiendo que la multiplicación que se requiere es de números. Además que de pasar argumentos erróneos a la función sólo se sabrá en tiempo de ejecución, es decir, frente a tus usuarios.

Siguiente desafío, como implementarías esta función usando anotaciones de tipo en Typescript?
Haz [click aquí para realizar el desafío](https://www.typescriptlang.org/play?#code/PQKgUABBAMBMEFoIBUCuEAOAnAlgWwFMsBDCAM1QDsBjHAZ8omoHtGAXHDZgZ0kQQGC+AYlEQAIgW7EyAW+Z8wAUUZS2BCABMpM+RHWVNWAIfcIAR1QbiAcywEbxLBGKVmbYrVZStGjlxcIABtSbnwMII0KGnpGPFQgzCdSTwIMD2duOiDmCAA3Yhz7M0pUPABL3BZuADpFEGAwJtAIAH12js6OiEAeDcBp-cB4P8BYAkAhMgGJHTlcrpn2iAam6OoOVgh4oIAKYgAaCAAjAEoIAG8+ezZULEZSEH2wAF9m29mZlDUzF675xvDmLDZ9ABPDAaY4QJSWQq7JQADxBy12ADl3ABBSiAiD3chYZh4CAAcgAAmxgQQENQABaFSKUGxSYCoDhBbj4pokkFMYjcHwAXggAG0+LD4WwADwQ1CFUX84gALggpTweyIuz2ssVyqwAF1dgAFZKEdRYbii9kEZhkNYJAB8tu2YC1YCAA)

> Puedes [ver la solución aquí](https://www.typescriptlang.org/play?#code/PQKgUABBAMBMEFoIBUCuEAOAnAlgWwFMsBDCAM1QDsBjHAZ8omoHtGAXHDZgZ0kQQGC+AYlEQAIgW7EyAW+Z8wAUUZS2BCABMpM+RHWVNWAIfcIAR1QbiAcywEbxLBGKVmbYrVZStGjlxcIABtSbnwMII0KGnpGPFQgzCdSTwIMD2duOiDmCAA3Yhz7M0pUPABL3BZuADpFEGAwJtAIAH12js6OiEBQcggAZWYg1FoGNq6J1ogwABlQoZHYplYw7nUIAkZbe0dnV3dPHG8zUhyTrBsyzbYeX2DSaNHKABoN1TWNamJuXMoCaikOA8eFulmseAARrcnJdCJQbmZuARXBBSoQqjw6mBkABPDBSai4dIQewsNwAiDoZhkUkaAiJfy5AAGaIhRCZSRIEBw2nhODIOC+zhumlyjLulAAX+jbgAKa7MYqvMg5DzwqSvAhsagASixAAVki47A4nC43B4vH8TsFoRcrvDbmE8BECHD1ttTWYqBAmQAuDnabgYVDlMzaRJuSH2XyJGEOm4QHHBKw2XL0-ScZia97rL4-X2s9k1aZgACCAAtFaQ1A8qE8IFkci4AelSAUij40ZVBbdk5smN9ctpuer4WaMN8ze4sGKs3cO4rXti8QSiWx9BpmxWjdI2c4Bxw3QER0GnLOgs3hxoAEQsF04EJPG8bPJA1yiixWDaJIiz5yXg436kGwqDetwqBOEcZhgA0TSPBwrAQPEQSysQfqomU+6vBCGFFlgOoQAA3nw9igVgWwQCAEAQmAAC+TQobKACM0DPLAhHAMAECwNA0xMTerE3uxnHcUoWD-nc4pgAJQnPDesA3qJEDiZJI7SUxrGvDePyEGwFY4DglA2EpKkSYqUnzjJCQsWxZCFEihGqRZ6lWc01GTBMKBqGYnldFRjThIqG5sKuxEqZYhSvEoAAe+LUGwrwAHLuGWlDJnR5CzngEAAOQAAKhfiCDUDugHGVIwCoBwQTcLlTRFZ83w+AAvBAADafCxfFbAADxKJFQS9e16GYZCRA4X6+EALqvIaJB6UQ3C9Y11LIQkAB8W3PGA01gEAA), te recomiendo intentar todo lo que puedas antes de mirar 


Typescript permite agregar anotaciones de tipo a tu tradicional código Javascript para asegurarse en tiempo de desarrollo (gracias a tu editor de código) o en tiempo de compilación que los datos utilizados son correctos.

> Typescript no es ejecutado por el navegador, por lo que necesitas "compilarlo" o transformarlo a código Javascript. Es así como Typescript te entrega seguridad de tipos ya que cuenta con un compilador `tsc` que revisará tu código antes de llegar a tus usuarios.

## Como instalar Typescript

Si bien durante este microcurso usaremos principalmente el [playground de Typescript](https://www.typescriptlang.org/play), es importante que conozcas como puedes trabajar con Typescript en tu propia máquina.

A modo general lo que requieres para trabajar con Typescript de forma local es instalar el paquete `typescript` en tu proyecto desde npm.

Puedes crear un proyecto desde cero utilizando 

```bash
$ mkdir proyecto-ts 
$ cd proyecto-ts
$ npm init -y
$ npm install typescript 

```

Con esos comandos lo que habrás logrados es, 
- Crear un directorio llamado `proyecto-ts`
- Inicializar un proyecto 
- Instalar Typescript

Ahora que Typescript está instalado, puedes abrir el archivo `package.json` presente dentro de `proyecto-ts` y agregar lo siguiente 

```json 
"scripts": {
  "build": "tsc *.ts --out app.js",
  "build:watch": "tsc *.ts --out app.js --watch"
}
```

Esos scripts que agregaste a tu package.json ejecutarán el compilador de Typescript `tsc` sobre todos los archivos Typescript presentes en tu proyecto creando un archivo Javascript llamado `app.js`

Puedes probarlo copiando el código de los desafíos en un archivo llamado `app.ts` y luego en la consola ejecutar `npm run build`.



Esto ha sido todo por hoy como introducción de este micro-curso, en la siguiente entrega revisaremos que son `type` e `interface`.
