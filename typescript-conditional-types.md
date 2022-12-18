---
title: Typescript power of Conditional types and String Literals
--- 

You are already using Typescript, creating type annotations for your function arguments and return types, you can also use native utility types to perform type transformations to accomodate to your personal requirements and already have the handle of Typescript Generics. It's time to go to the next level: Use Typescript to create *type algorithms*.

### Why would you want to write algorithms in the type level?

Every programming language was built to create solutions by using different algorithms and Typescript is not different in thar regards, the only difference is that the code youwrite only affect thet type level (build time) helping you to check the safety of your code. So, creating type alogorithms can help you increase the type safety and accomodate your code to more complex requirements.

### What type of algorithms can you write?

The most basic algorithm you can write is code branching, this is, use conditional blocks to allow the data flows to one or other situation based on certain condition, you know, the old-good `if-else` block. Typescript type system is Turing Complete and therefore it also implements code branching strategies but in a slighlighly different way.

> *What Turing Complets means?
> In simple and general words: A language or system that is able to answer any computable problem given enough time and space. 
> And [Typescript is able to accomplish](https://github.com/microsoft/TypeScript/issues/14833) that sentence therefore you can implement solutions to any problem.

## Typescript Conditional Types 

The code branching strategy in Typescript is known as *Conditional Types* this is, you will define a type whos value depends on some calculation.

```ts 

type ConditionalType = SomeType extends BaseType ? true : false 

```

The sintax can be weird but at the same time very similar to a concept you already know from the Javascript world: [Ternary Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

The above example can be read as *"If **Condition** then return `true` or `false` otherwise"*

The **Condition** part is the piece of code that most often throw newcomers back since its a bit strange.

*Every* conditional type use the `extends` keyword to implement the condition side of the ternary operator. Here the `extends` keyword behaves like a type checker. So, `SomeType extends BaseType` can be read as *Is `SomeType` assignable to `BaseType`*.

> We reviewed the behavior of `extends` keyword in a [previous article of this series](https://egghead.io/blog/learn-the-key-concepts-of-typescript-s-powerful-generic-and-mapped-types), be sure to check it out.

The type system is an implementation of mathematical concepts, you can represent the idea of a type been assignable to another as a Benn diagram.

![Benn Diagram of type assignability](https://res.cloudinary.com/matiasfha/image/upload/f_auto,q_auto/v1671290442/Screenshot_2022-12-17_at_12.19.57_lukk5s.png)

So, if the type used at the left side "belongs" or is a "sub set" of the type used at the right side of the `extends` keyword then, the condition is evaluated as `true` and the first branch of the conditional type is "returned".


Let's create a simple condtional type that will check if a string have the shape of an url.

```ts 
import type { Equal, Expect } from '@type-challenges/utils'


const str1 = "https://matiashernandez.dev"
const str2 = "htpss://not-valid-url"

type IsUrl<Url extends string> = Url extends `https://${string}.${string}` ? Url : `${Url} is not a valid URL String`



```
<small>Check the [Typescript playground](https://www.typescriptlang.org/play?#code/PQKgBAqgLglgNjKBPMUD2YDGAnApgQyl1VwGcpSt9SyRgAoemAWwAc1spUlXiBvMAFEAjgFd8cADRCAHr0xcAvmABm2NMzAByAALJeAWkwALCXFwA7AOZlgo2HFJbG9TGgvkw5bAEYwAXjAAImMoKFZSAC5gYGZCGGpjXGwLfAsAE1wALwA6TIA3INd3T28AJgDg0IiomIs0KAN8iRh0g1FsOCL6fWIASVIIToAeIbgwXBkiDMpvGGsAPkqxianLdMoAA1Dw2uAAEj4560Ucw+OrRU2wAH5ITrBIsE3DseUYSnqufDBmhHTIAAlAAyYAAylBsPMrJtGL0qDRKIEANr0MDo2TyKDDETiODDXpoFReSE+aQDMYEni4Ikk3wLBmSRgYzG4BQ4sQSYYvPiE4nld6fBpgH5-VpA0EQqHWTbkwYjPl0soMhZMgC6jBUogsChg7jArHw2BolJWk2mGzp0IWAAoOnAnhSRmMFgBKMB8RiKRiG424MY27w+d0xMAAeQA0j6jSbOoHIWUQ8AwAAxfDwUhAA) to play around</small>

In this example you can see how the power of conditional types is unleashed when is used in combination with Generics and other Typescript features like string literal manipulation.

The above exapmle can be read as.

- `IsUrl` is a "type function" that accepts a Generic parameter named `Url`.
- Condition: Does `Url` "match" (is assignable ) the described string literal?
- The string literal side: The right side of the condition is a string literal "shape" that describes how a url string looks. Start with `https://` followed by *any* string, then a `dot` followed by *any* string.
- If *Condition* pass then "return" the `Url` 
- If *Condition* does not pass, then "return" an error message.

You can use this type to check if a string literal match or not the url string shape like the following example:


```ts
function parseUrl<Url extends string>(url: IsUrl<Url>) {

}

parseUrl(str1) // OK

parseUrl(str2) // Fails: Argument of type '"htpss://not-valid-url"' is not assignable to parameter of type '"htpss://not-valid-url is not a valid URL String"'.

```

Conditional types are implemented with the same syntax as Javascript ternary operator, and is the only way to implement code branching. Why? Because Typescript type system programming is a functional implementation therefore you have to write your code with expressions.

So, ternary operator is the way to go, but, is possible to implement multiple conditions at once?. Is totally possible by writing nested conditions, it can become pretty messy.

```ts
type CleanPath<T extends string> = T extends `${infer L}//${infer R}`
  ? CleanPath<`${CleanPath<L>}/${CleanPath<R>}`>
  : T extends `${infer L}//`
  ? `${CleanPath<L>}/`
  : T extends `//${infer L}`
  ? `/${CleanPath<L>}`
  : T
```

The snippet above was extracted from [tanstack/router source code](https://github.com/TanStack/router/blob/3d284a6b626229909f44c78f424d5c181364ae23/packages/router-core/src/link.ts) it use a nested conditional type that includes inference, template literal pattern matching and recursiveness. It aims to get the type of a path string.



## Pattern Matching

In the previous code sample an utility type was defined: `IsUrl` this is an a bit advanced usage of conditional types that checks if a value match certain pattern, this is known as **Pattern Matching**

The concept behind pattern matching comes from computer science and is an integral part of the functional programming paradigm. It's described as *"the act of checking a given sequence of tokens for the presence of the constituents of some pattern."*

> Check [this wikipedia](https://en.wikipedia.org/wiki/Pattern_matching) entry to read more about the computer science concept

In our case we are using it this feature to check if a type value *extends* the pattern, in the previous case an string literal pattern. But it can be any pattern.

```ts 
type IsPost<P> = P extends {title: string; author: Author } ? true : false

type t1 = IsPost<{title: "This is the title", author: { name: "Matias"}}> // true
type t2 = IsPost<{title: "This is the title"}> // false

```
It's important to know that here we are talking about type level pattern matching that allows you - in combination with conditional types - perform code branching based on how the condition is evaulated.

## Template Literals

> This will be a brief overview of template literals since we will need a full-leght article just to describe the full feature.

Another feature used in combination with conditional types are **template literals**. Coming from Javascript programming I'm sure you are used to use template literals to handle your string needs, like concatenating and interpolating strings and variables.

The syntax used here is the same as the one used in runtime code, backticks and the curly braces for interpolation. From the previous demo.

```ts

type Template = `https://${string}.${string}`
type IsUrl<Url extends string> = Url extends Template ? Url : `${Url} is not a valid URL String`

```

Here the `Template` type is the template literal that describes the shape of a string. Here we are interpolating something that can be strange at first glance since is not a value but a primitive type: `string`.

The usage of `${string}` describes **any** string so `Template` describes the combination of all possible strings that starts with `https://` and have a dot in the middle.

This is really powerful to type function arguments, you can define a template literal type that describes exactly the shape of string the function requires.

```ts 

// This accepts any string as userId 
function fetchUser(userId: string) {

}

// This accepts only strings with the correct uuid shape 

type UUID = `${string}-${string}-${string}-${string}`
function fetchUser(userId: UUID) {

}

```

> The `UUID` template literal is a simplification of the real UUID specification.

In the example the second implementation of `fetchUser` can be considered as better since the only type of argument that can be passed are the ones that ressembles an `uuid`.



## Type Inference 

Until now you learned how to create a conditional type and combine it with Generics, and template literals, but there is more. You can extract "pieces" of information from the types to used in your conditional algorith.

```ts 
type ExtractDomain<S> = S extends `https://${string}.${infer Domain}` ? Domain : never 

const str1 = 'https://matiashernadez.dev'
const str2 = 'https://matiashernandez'

type test1 = ExtractDomain<typeof str1> // 'dev'
type test2 = ExtractDomain<typeof str2> // never

```
<small>Go ahead and [check the playground link](https://www.typescriptlang.org/play?ssl=5&ssc=1&pln=6&pc=39#code/PQKgBAqgLglgNjKBPMUD2YDGAnApgQyl1VwGcpSt9SyRgAoemAWwAc1spUlXiBvMAFEAjgFd8cADRCAHr0xcAvmABm2NMzAByAALJeAWkwALCXFwA7AOZlgo2HFJbGmNBfJhy2AIxgAvNrGUFCspABcwMDMhDDUxrjYFvgAJrgAXgB0qQBuzq7uXF4ATP6BwaERUTFxCUkWqWnO9PrEgjJQ2PgKACIa+DAWADwAygB8pcNguO2WyZQABkEh4ZEAJHxeA1aKGesDKglgvdEDivNgAPxHfQNgYWAWuNmHjM08xJjUZKUA2vRgANk8iggxE4jggy0OS00jaHS6UGO-SGLTQKk8HW8o2xkkYgKBuAUoLEEkGj2e2Fh7U6PRuKPeaIx2CK2NGuIAuowgA) to play around with the implementation</small>

The example above implements a utility type that retrieves or extract the domain string from a url. Is very similar to what we previously did with the `IsUrl` example.

- The type receives a Generic named `S`.
- The condition defines is to check if `S` match with the defined template literal.
- The template literal is defined as `https://${string}.${infer Domain}`
- If `S` match with the template then it will "return" `Domain` or `never` otherwise.

The new part here is the 3rd step. The template literal definition includes a new keyword `infer`.

The `infer` keyword enabled the creation of **inline type variables**. This type variable will hold some data that is assigned by destructuring the type passed in the left side of the `extends`.

With `infer` you will create a new Generic that can be named as anything you want. Typescript will be in charge to perform the assignation of the type.

> One caveat of the `infer` keyword is that the generic variable created can only be used in the truthy side of the code branching.

Let's see another example.

```ts 
type NonEmptyArray<A extends any[]> = A extends [infer First, ...any] ? true : "The array cannot be empty"

type test1 = NonEmptyArray<[1]> // true 

type test2 = NonEmptyArray<[]> // The array cannot be empty
```
<small>As always, [check the playground here](https://www.typescriptlang.org/play?#code/PQKgBAqgLglgNjKBPMUD2YDGAnApgQyl1VwGcpSt9SyRgAoemAWwAc1spUlXiBvMAFEAjgFd8cADRCAHr0xcAvmABm2NMzAByAALJeAWkwALCXFwA7AOZlgo2HFJbG9fcQByaC4LbIAgtjY+EgAPH5guDJEFgAmlPgWSADaALoAfGAAvGDhkdFxYEkwFiq42GAAYjDY5NIAdA0JSClgAPyo2KLEAFxgAEQAKsbE+IHBVBYWaFwARsS4vkh9Lq48xJjUZFmF9GB7svJQISLicCFQnbjSnt6LAUGhSQCM6Wlpkoz7B7gKx2ISIUGwzAoweEyms3miz61y8PlY-jGj1e73oKUYQA)</small>

This example defines a type that allows you to check that an array is not empty, to do so it use a conditional type plus two other features:

- `infer` keyword to extract the first element of the array 
- and Variadic Tuples 

The type can be read as *If A is assignable to an array that contains at least one element then return `true` or an error message otherwise*


The infer keyword is also used to create some of the native utility types like `Parameters`

```ts 
type MyParameters<F> = F extends (...args: infer Args) => any ? Args : F


type fn1 = (arg1: string, arg2: number) => number 

type fn2 = (obj: { arg1: number, arg2: boolean}) => boolean 

type noFn = { arg1: number, arg2: boolean}

type t1 = MyParameters<fn1> //  [arg1: string, arg2: number]

type t2 = MyParameters<fn2> // [obj: {arg1: string, arg2: number}]

type t3 = MyParameters<noFn> // {arg1: number, arg2: number}

```
<small>[Link to playground](https://www.typescriptlang.org/play?#code/PQKgBAqgLglgNjKBPMUD2YDGAnApgQyl1VwGcpSt9SyRgAoemAWwAc1spUlXiBvMAFEAjgFd8cADRCAHr0xcAvmABm2NMzAByAALJeAWkwALCXFwA7AOZlgo2HFJbG9fcQCySAAr5s+ZrhE2KQAPABiAHxgALxgYWC4MkQWACaUABQAdNm+VqQAXGAwFiq42GAAgth5AJQxUfgWKAD8ldWUhWEubqoWAIwxYOm5fYXk2MVW0rkATIUWoswARmV10VELy2VgjD0qFjOD6WhLAFaFAiPziyvY09VzYEtoaOaNimtRz68EFjuuPGIFjQYT+sUu1VGYE2t3uVke3zeFkUu0BVBolFiAG16GA8bJ5FAQiJxHAQlirmBxpM4Y8YWUALrSTw+PwBIKhfZ9CI8ySMfEE3AKYliCTkk7nMB8XEC-GU+nYADcMtlYFmhURv2VAsUTLALN8-kCZU5Bx5ET5KsFwpJYuBoOZ3kN7JNIXtFnN9AZjCAA)</small>


The example reimplements the utility type `Parameters` that takes a Generic `F` and, if `F` is a function it will return a tuple with the parameters of the function.

## The demo 

Let's review a full demo of an utility type that relies on Conditional Types, `infer` keyword and recursiveness.

Let's build a simple query string parser, that will:

- Read an url string.
- Perform some transformations.
- Get the query string data as an object type.

```ts 

const route = "https://mysite.com?query=123&user=2&test=some string"

type ExtractQS<R extends string> = R extends `https://${string}?${infer Q}` ? Q : never


type _Merge<O,P> = {
    [K in keyof O | keyof P]: K extends keyof O ? O[K] : K extends keyof P? P[K]: never    
}

type SplitToObject<S extends string, Separator extends string> = S extends `${infer Left}${Separator}${infer Right}` ?  Record<`${Left}`, Right> : S

type ExtractQSParameters<QS extends string> = QS extends `${infer Var}&${infer Rest}` ? 
    _Merge<SplitToObject<Var,"=">, ExtractQSParameters<Rest>> : SplitToObject<QS,"=">

type RouteParameters = ExtractQSParameters<ExtractQS<typeof route>> // { query: "query"; user: "user"; test: "test"; }

```
<small>Visit [the playground to read more](https://tsplay.dev/w1Akkw) about the implementation</small>

That looks complex right? But at this point we already reviewed all the concepts required to read and understand the snippet.

- The first type defined, perform a pattern matching task using a simple template literal. At the same time will extract the query string part of the url using `infer Q`.
- Then it defines an internal utility type `_Merge` that simple takes two object types, will merge (and overwrite) the properties into a new object type (using mapped types)
- Another helper type is created that will split a string `S` based on a separator `S`  using `infer` and pattern matching to return a new Record as key/valur pair.
- Lastly it will create a new Conditional Type that takes a query string and using `infer` will extract the key/value pair, then it will merge into a new object type.

This example helps to showcase the power of the type level algorithms that can help you create complex types to handle your data requirements.

## Summary 

In this article we reviewed one of the most exicted features of typescript, the one that allows you to implement algorithms on the type level opening the pandora's box to implement solutions for your complex requirements.

- Conditional types are implemented using a ternary operator
- Here the `extends` keyword is used to create the condition part of the conditional type.
- Conditional types can be used to peform pattern matching.
- Conditional types can be used to "extract" type variables using the `infer` keyword.

