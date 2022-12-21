The keyof operator is a powerful typescript building block that unlocks you to build more complex data shapes like using mapped types or complex condiational types.

So, how to use it.

Imagine that you have a type that list a set of colors for your app. Like this one

You want to build components that accepts a prop named color that is typed in a way that allow you to just write the name of the colors as a string literal. But keeping all typesafe

Youi can do it by manually writing all of the optiosn in a union of literals, tthat works, But As I said in previous videos I'm lazy and want to automatically get the union of literal.

That can be done using the `keyof` operator that will returns all of the properties of the type as a union.

You can also use it with an enum, but you need to also use the `typeof` operator to bring the union into the Type world
Or use it with `const` doing the same operation.


## The keyof operator
A few primary concepts around Typescript help you build complex data shapes. One of that building blocks is the keyof operator.


This operator or keyword is the Typescript's anwser to the javascript `Object.keys` operator.


`Object.keys` returns a list of the properties (the keys) of an object. `keyof` do something similar but in the typed world only. It will return a literal union type listing the "properties" of an object-like type.


This operator is the base building block for advanced typing like mapped and conditional types.

> The keyof operator takes an object type and produces a string or numeric literal union of its keys. - [Typescript Handbook](https://www.typescriptlang.org/docs/handbook/2/keyof-types.html)

```ts
type Colors = {
    primary: '#eee',
    primaryBorder: '#444',
    secondary: '#007bff',
    black: '#000',
    white: '#fff',
    whiteBorder: '#f2f2f7',
    green: '#53C497',
    darkGreen: '#43A17C',
    infoGreen: '#23AEB7',
    pastelLightGreen: '#F3FEFF',
}

type ColorKeys = keyof Colors; // "primary" | "primaryBorder" | "secondary" ....

function SomeComponent({ color }: { color: ColorKeys }) {
  return "Something"
}

SomeComponent({ color: "WhateverColor"})

SomeComponent({ color: "primary"})
```

The previous code sample is an snippet from a real webapp, the `Colors` type describes a set of colors that can be used across the application.

The `keyof` operator is apply to the `Colors` type to retrieve a literal union of all the possible colors.

> Literal union means that is an Union type made up by literal values like "primary" | "primaryBorder"

The union is then used to type the props of `SomeComponent` allowing the `color` argument to be one of the colors defined in the type.

You can also use the `keyof` operator to build up more complex types or constratins along side Generics and tempalte literals, but that will be for another post.

There you go, the `keyof` operator can be small but is an important piece in the big scheme that unlocks powerful operations when used in the right place.