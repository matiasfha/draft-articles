# Basic Type Transformations with Typescript

<!--toc:start-->
- [Basic Type Transformations with Typescript](#basic-type-transformations-with-typescript)
  - [Why type transformations](#why-type-transformations)
  - [Create a Union from Object keys](#create-an-union-from-an-object-keys)
  - [Extract](#extract)
  - [Exclude](#exclude)
  - [UnWrap](#unwrap)
  - [Summary](#summary)
<!--toc:end-->

If you're reading this, it is safe to assume that you can write type annotations for your application code. You can create object types, unions, and intersections and use the basic types available.

In this article, we'll review how to perform type transformations using some of the native utility types. 

## Why type transformations 

Most of our tasks are to take some data to transform that into another shape to be displayed or consumed by other functions, services, or UI. In other words, our day-to-day work is about data transformation. Typescript's goal is to model your data needs. Therefore, Typescript can also model or adapt to the data transformation needs by transforming one type into another.

Most type transformation tasks can be performed using built-in utility types like the ones we'll review here.

One important note to remember. Types in Typescript are just data. 
We know that Typescript is a superset of Javascript, meaning that every Javascript code is also valid Typescript code. Still, this article focuses on **type level programming**, so we will review how to use the type system language to transform the data.


## Create a Union from Object keys 


This is the process of taking all the names of the properties of an object to create a union, like the following snippet.

```ts 

type State = {
  openModal: boolean;
  entity: {
    id: string;
    name: string;
  }
}

type Properties = 'openModal' | 'entity' 

```

How can you "read" the properties' names from the object?

This is where the keyof operator comes into play.

```ts 
type State = {
  openModal: boolean;
  entity: {
    id: string;
    name: string;
  }
}

type Properties = keyof State

```

The `keyof` operator is an essential concept for data transformations, is somehow a reflection of the runtime `Object.keys` method from Javascript into the type world.

> The keyof operator takes an object type and produces a string or literal numeric union of its keys. - [Typescript Handbook](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

The goal of `keyof` is to retrieve all property names from an object type. This retrieval will create a union of all the possible values in a "literal string" fashion.

## Parameters and ReturnType 

Sometimes you use a library that provides the types for all functions but only the return type.

You want to know the arguments of that function to, for example, re-use them or re-create them in another place of your code. How can you extract the function arguments?

```ts 
function someFunction(arg1, boolean, arg2: Record<string, unknown>) {

}


type SomeFunctionArgs = Parameters<typeof someFunction> // [arg1: boolean, arg2: Record<string, unknown>]

```
To accomplish your goal, you will use the `Parameters` type utility provided by Typescript itself. This utility type accepts a function type and returns a tuple with all of the arguments of that function, in this case, `[arg1: boolean, arg2: Record<string, unknown>]`.

With that in hand, you can re-use the `SomeFunctionArgs` type to perform your operations, like accessing only the second argument. 

```ts 

type TheObj = SomeFunctionArgs[1] // Record<string, unknown>

```

This utility type is beneficial to work with code you don't own, like a 3rd party library.

In a similar place but working in the other part of the spectrum, you have `ReturnType` that, as the name implies, will retrieve the type of the return value of a function. 

```ts 
function someFn() {
    return [1,2,3]
}

type ReturnSomeFn = ReturnType<typeof someFn> // number[]

function someFn2() {
    return {
        a: true,
        b: 123,
        c: {
            s: "some string"
        }
    }
}

type ReturnSomeFn2 = ReturnType<typeof someFn2> // { a:boolean, b: number, c: { s: string }}
```


## Omit and Exclude 

`Omit` and `Exclude` are kind of similar. They accomplish the goal of removing data from a type but in different ways.

`Omit` will allow you to declare what **key** you don't want to get from a type.

`Exclude` will allow you to remove an element from a union.


```
type State = {
  openModal: boolean;
  entity: {
    id: string;
    name: string;
  },
  moreData: number
}

type NoMoreData = Omit<State, 'moreData'> // { openModal: boolean, entity: { id: string, name: string}}

type Properties = keyof State // "openModal" | "entity" | "moreData"

type NoEntityUnion = Exclude<Properties, "entity"> // "openModal" | "moreData"

```


TLDR; Use `Omit` to work with object-like type and use `Exclude` to operate over unions.

## Pick and Extract

On the other hand, you can find these two utility types.

`Pick` can be seen as the opposite of `Omit`. It lets you choose what key of an object-like type you want and remove the rest.

`Extract` will allow you to retrieve the chosen element from a union. It is similar to the Javascript `Array.filter`, where you pass a condition, and the ones that pass the condition are chosen to be in the final type.


```ts 
type State = {
  openModal: boolean;
  entity: {
    id: string;
    name: string;
  },
  moreData: number
}

type MoreDataT = Pick<State, 'moreData'> // { moreData: number}

type Letters = { letter: 'a', value: 3} | { letter: 'b', value: 100 }

type LetterA = Extract<Letters, { letter: 'a'}> // { letter: 'a', value: 3}

```



## Summary

Typescripts ship with a good set of utility types that will help you with almost all of the data transformation requirements of a standard application development process. Learning how and when to use them will help you move faster and better develop type-safe code.

