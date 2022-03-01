# Advanced Typescript for your project

> Note: Need a better title since this is more like "intermediate" TS to get you started with complex data types

You may already know that Typescript can be a significant improvement for your Javascript developer team but, as any new language there is a learning curve to harness the full power of it.

In this article I will drive you through some advanced usages of Typescript that can help you achieve those data requirements or features.

> If you are looking for an in depth guide to help you understand the Typescript reasoning and why you should choose it for your project check this previous article: [Why we choose Typescript all the way through](https://clevertech.biz/insights/why-we-choose-typescript-all-the-way-through)

## What will be covered

We're going to explore several "advanced" building blocks  that allow you to harness the power of the Typescript's type system. The use of this types and features allow you to enforce complex compile time invariants.

- Generics
- Union and Intersection Types
- Typescript native utility types
- Mapped Types
- Template Literal Types
- Conditional Types

The main value proposition of Typescript is to add some rules and order to a very loose language as Javascript leveraging the power of a compiler to act as line of defense to catch data types related bugs. Static typing enforces contracts, which prevent certain classes of defects.

Let's dive directly into this

## Generics

The main idea of the concept of **generics** is to allow you to create reusable pieces of code that can work with a variety  of types rather that be restricted to a single one.

You can consider **generics** as code templates that you can define and reuse through your codebase. The use of it will provide you with a way to tell functions, classes or interfaces what type they should use when you call it.

You can even think in the **generic** type as an argument of a function.

You should strive to use and create a **generic** function when you need things like:

- A function that can work with different data types
- Use the same data type in different places

Let's see an example use case to when and why use **generics**

Suppose you need a function that randomly return an element from an array

```typescript
function getRandomElement(array: number[]): number {
    let index = Math.floor(Math.random() * array.length);
    return array[index];
}
```

This function, accepts an `array` of type `number` and returns a `number`. All good here right?.

Now, let's assume that you also need to get a random string from an array of strings, how that function would look?

```typescript
function getRandomElement(array: string[]): string {
    let index = Math.floor(Math.random() * array.length);
    return array[index];
}
```

Exactly, the same, the only different is the type of the argument, in the first case you used `number[]` and now `string[]` .

How can you consolidate both function that in fact have the same logic?

One option is to change the type of the argument to be `any[]` but, by doing this you will loose the typechecking powers that Typescript gives you, so, is not a good solution at all.

The other option is to use generics, here you'll define a "type variable" that will capture the type provided at the time of calling the function, like:

```typescript
function getRandomElement<T>(array: T[]): T {
    let index = Math.floor(Math.random() * array.length);
    return array[index];
}
```

> By convention typescript community use uppercase single letters to declare the type variable, in this case `T`. However, you can use any word to define it.

Now, let's call this generic function with different types

```typescript
let randomNumber = getRandomElement<number>(numbers); 


let randomString = getRandomElement<string>(string);



// More complex
interface Profile {
	id: number;
	name: string;
	email: string;
	avatar?: string;
}

let randomObject = getRandomElement<Profile>(profiles)
```

In this examples, you explicitly passes the type of the array that the function will use using the `<>` sintaxis.
In practice, the compiler will use type inference based on the argument, meaning that Typescript will guess the type of the generic based on the information from the argument passed to the function.

You can also use multiple generic parameters with **generics** by using different letters/names to denote each type.

```typescript
function someAwesomeFunction<T,U,V>(arg1: T, arg2:U, arg3: V): V {
}
```

## Using generic constraints to limit types

It is possible that you want to create a generic function (or interface or class) that can accept different types but only from a subset of types, in other words, you want to constrain the types that can be used for the generic.

To accomplish this constraint requirement you'll use the keyword **extends**  that in effect allows to inherit functionally from a base type or class.

Let's use the same function as before, but let's say that you only want to accept arrays of strings and numbers.

Using generic constraints to limit types

```typescript
function getRandomElement<T extends number | string >(array: T[]): T {
    let index = Math.floor(Math.random() * array.length);
    return array[index];
}

const numbers = [1,2,3,4,5]

const randomNumber = getRandomElement(numbers)

const strings = ["one","two","three","four","five"]

const randomString = getRandomElement(strings)

const booleans = [true, false]

const randomChar = getRandomElement(booleans)
//Argument of type 'boolean[]' is not assignable to parameter of type '(string | number)[]'.
//  Type 'boolean' is not assignable to type 'string | number'.
```

## Union Types

A **union type** (or just "union" or "disjunction") is a way to define a mutually exclusive type. Is a way to create a value that can be of more that one type at a time.

This type provides information to the compiler that help it to prove that the code is safe for all the possible data types passed to it.

This type is a perfect fit when you know what are all the possible states will be but do not know for sure which one will be used at certain point. One common use case for this type is to define the actions of a dispatcher like in the reducer pattern (like Reduce or `useReducer` in React).

Let's think in a promise, a promise can be in 3 states, `pending`, `fullfilled` or `reject` you can model this states like

```typescript
interface MyData {
    id: string;
    name: string;
}


interface FullFilled<T> {
    state: 'success';
    data: T[];
    error?: never;
}

interface Rejected {
    state: 'rejected',
    data?: never;
    error: Error
}


interface Pending {
    state: 'pending',
    data?: never;
    error?: never;
}

type RequestState<T> = FullFilled<T> | Rejected | Pending


// utility function used to update an state
function setState<T>(initial: RequestState<T>) {
    let state = initial
    function updateState(newState: RequestState<T>) {
        state = newState
    } 
    return {
        state,
        updateState
    }
}


async function doRequest() {
    const {state, updateState} = setState<MyData>({ state: 'pending'})
    try {
        const result = await fetch('/some-url')
        updateState({
            state: 'success',
            data: await result.json()
        })
    }catch(error) {
        updateState({
            state: 'rejected',
            error: error as Error
        })
    }

    return state

}

async function main() {
    const result = await doRequest()
    switch(result.state) {
        case 'pending':
            alert('Request is Pending');
            break;
        case 'success':
            alert('All good');
            break;
        case 'rejected':
            alert(`It failed: ${result.error.message}`)
            break;
    }
}
```

In this example there are 3 interfaces: `FullFilled`, `Rejected` and `Pending` that model the 3 states of the promise, then there is a type declared named `RequestState` that states that it can be any of the 3 possible states (one at a time).

Finally in the function `doRequest` the request is perform and an state is stored in a variable named `state`.  This `state` is then return by the function so in effect `doRequest` return type is `Promise<RequestState<MyData>>`.

Then you can use this into another function and just `switch` over the state of the request by accessing the `state` value, here typescript will ensure that you cover all the possible cases.

[Here is the demo](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgLIE8Aicx2QbwChkTlgATALmQGcwpQBzAbmNJDgFsJq6GQWhAL6FRoSLEQoAYgFcANvOnBFEcgB4AKgD4CbEnRw9kAchqyESGjROtSycjjjVNAbQC6d0tCgB7KAD81CAQAG7QrCKE4tDwSMgAShAAVhAIkOR69oaQ1CZQKWkZJgA0+g5OQcgh4VBeJD7+1ACiUH5QwqLR4LFSyAAKECDkTFmkOcYmAA5DIwKl5Y64VTUR5Y2BwWFrUWDoM4kQAI6yEHQAyriQWroAvMhyisqqGjrIAD6HqelqHwOzTFEMFkIHSwF8IFoEDAlyMNwAFKBgGBgHB5NQkiczjCrhAbgBKMYkeTQ2i45D3JEotHlYGglEQ5CyKZLCCwyDwkIAd3ZxkxpwuuIJRPsZKMFOqEB5uPKQmQ5QKYFkUEhRFF41xZXVJGZrN5ss6ojgNHQoOQdLBjPIvn52PhhLVpAQELoBAmJSZLKMvLl9xo0N56gw2Fw2nh+DFuVMM2GTBMQnx5Xo6BF9mdIFdBXM8jAErgXLgyPN0IQAAt4SYAPQ0XzcAC0yvkJkT2s9etx4fKrYmeXMljONi1rdIS2cyHzhdzWYUYAAdMkayB7V3SAnZQgcGX4RsHSudV7ILzO8ONUY8gVvsUhyeNtQNuOaMhWu098g1-YovZFcrIRNRFFjVNBBzRBS1IU4Qsl13NMXSnM4ZzzAsi2tW06GXbIuWRLdpxzWcJmg7UN39aMAXmShX3sNFoDACtUNzYBH0GWN5nxephwAIwKOAAGs2LTY0UDMCwrBsciTxIKioBokwAEFFGQRhfF8chmz47VOIgHi1JIIjBIvIo1BMMTxMkmiAAMAElc3gFQ1GoAASfAcLnDZZ24aw4EYCAhDMltxI0rSDSEIA), go ahead and try to delete one of the switch cases. What happen?

Other common use case of an **union type** is found in the DOM itself, DOM events always happen independently of each other. So there is a finite list of possible events that can be processed:

```typescript
type Event = MouseEvent | KeyboardEvent; /* and so on */
```

One particular case of union type are **discriminated unions** (or tagged union), this special case allows you to easily differentiate between the types with it.

Why would you want this? Because an union type can also host different types and sometimes you will want to know what type is in use, you can always use type guards for this purpose but there is a shortcut: **discriminated unions.**

To create a **discriminated union** you need to add certain field to each type in the union which can be used to differentiate between each type.

One common use case for this type of union is to check that one piece of the data is present while other part is not. For example: A user account should have `username` and `email`  and also include an `avatar` OR an `emoji.

To model this  you need to combine different types to define each case and use optional attributes along with the `never` type like this:

```typescript
type WithUsername = {
    username: string;
    email?: never
}

type WithEmail = {
    username?: never;
    email: string;
}

type WithAvatar = {
    emoji?: never;
    avatar: string
}

type WithEmoji = {
    emoji: string;
    avatar?: never
}



type User = { id: number } & (WithUsername | WithEmail) & (WithAvatar | WithEmoji)

const userWithNameAndAvatar: User = { id: 1, username: 'username', avatar: 'avatar' }

const userWithEmailAndAvatar: User = { id: 2, email: 'email', avatar: 'avatar'}

const userWithNameAndEmoji: User = { id: 3, username: 'username', emoji: 'emoji' }

const userWithEmailAndEmoji: User = { id: 4, email: 'email', emoji: 'emoji'}

const wrongUser: User = { id:5, username: 'username', email: 'email', emoji: 'emoji'}

const wrongUser2: User = { id:5, username: 'username', emoji: 'emoji', avatar: 'avatar' }
```

[Here you can find the demo of this code](https://www.typescriptlang.org/play?#code/C4TwDgpgBA6glsAFgVQM4QE4DsCGBbaAXigG8AoKSqAV3W3wgC4pVgM4sBzAbgqojw44AGwD8zLBABumMgF8yZUJFgJEAUUEioxclRp1cBcVEkyMvfQKHDmrdl14Kl4aPCQBBKTmA4MO0j5KAQB7ACs4EzNMSyocb18MOzYOTnlFZTc1TXC4AL1+PFzkhx4gqHifPyjpWWcM1yg0THyoOAATCWo8ACMWuSgAMigACncUQwYoAB9VJE0bAEoh0fGvKv9Z8ZyIxcUAYxCsVgNMcYA5Bg8sdvXE5mb-XTbOqABGABpT+gJmAHJaJgjBA-l9KvcoH9wX4-lBnIdjsBvtstMJrrcEn4HnRWh1mAAmL7WET-YnCUEVTFJSHQjB-eFHE6AjAXK43HZwbEtZ54qAAZi+zOB-yFDApoQipKKEVhDMRyOyqPRHK5T1IL2YABYiaipTZxdLOZCJXB6QdGUiAO4YI6cR6q3GdACsgsmv0hooIBpsepEBuKxsNZrICJO1ttj3xDp5ztdQIYIrdIJ1Ab+JoptP+tL+AAK5EA)

To implement this restriction there are 4 types that work in pairs to describe what attribute should be present and how the other attribute should be.

Then a new type is created as the **union** of the other ones.

Then the new type is used and typescript will tell you if you are using the wrong combination of attributes.

Union types are an excellent modeling tool, there are good reasons to not use them:

- When the types are known at compile-time: You can use generics instead to provide further type safety and flexibility.
- When we need to enumerate all possibilities at run-time: Use an enum instead for this).

Union types are an ergonomic way to model a finite number of mutually exclusive cases, allow you to add new cases with ease.

> By combining this feature with mapped types, you can even dynamically create types as shown in [this great demo by Matt Pocock](https://twitter.com/mpocock1/status/1498284926621396992?s=20&t=w7tdOotYWM4IEic_NdqGOQ) that transform an union type to another union type with different shape.

## Intersection types

**Intersection** types can be considered the opposite of the previous **union** type. In creates a new type by **combining**  multiple existing types. The new type will have all the properties and features of the existing ones.

```typescript
interface Student {
    name: string;
    age: number;
}

interface WithJob {
    company: string;
}

type WorkingStudent = Student & WithJob 

const workingStudent: WorkingStudent = {
    name: 'Student',
    age: 20,
    company: 'Ccompany'
}
```

When you are intersecting type the order doesn't matter.

In summary an **intersection** type combines two or more types to create a new type that includes all the properties of the existing types.

## Typescript native utility types

Typescript provides you a handfull of handy utility types that helps to manipute types with ease. This utility types are in fact generic types.

### Partial

This utility type will enables you to create a type that will set all the properties of the exiting type to be optional.

```typescript
interface Person {
	firstName: string;:
	name: string;
	country: Country; //asumme the existance of this type
	age: number;
}


const person: Partial<Person> = { firstName: 'Matias' }

const person2: Partial<Person> = { age: 36 }
```

### Required

Is in fact the opposite of **Partial**, marking all the properties of a type as required.

```typescript
type Person = {
	firstName: string;:
	lastNname?: string;
	country?: Country; //asumme the existance of this type
	age?: number;
}

type RequiredPerson = Required<Person>

const person: RequiredPerson = { firstName: 'Matias', lastName: 'Hernández', age: 36, coutry: 'Chile' }

const person2: Required<Person> = { firstName: 'Person 1', lastName: 'Last Name' } // error cause' is missing age and country
```

### Readonly

This type will mark all the properties of the type argument to be not-reassignable or **readonly**

```typescript
type FuncArgs = {
	id: number;
	name: string;
}

function someFunc(args: FuncArgs ) {
	args.id = 10; 
    return args;
}

function readonlyFunc(args: Readonly<FuncArgs>) {
    args.id = 10; //Cannot assign to 'id' because it is a read-only property.
    return args;
}
```

### Pick

WIth this you can create a new type by "picking" properties from an existing type.

```typescript
interface Student {
    age: number;
    firstName: string;
    lastName: string;
    school: string;
    grade: string;
}


type Person = Pick<Student, 'age' | 'firstName' | 'lastName'>

const person: Person = {
    age: 36,
    firstName: 'Matias',
    lastName: 'Hernández'
}
```

This type allows you to create new types from a previous one by describing what properties you want as the second argument of the generic parameters.

### Omit

In the other hand, is there **Omit** that works as the opposite of **Pick** by allowing you to selected what elements to remove or not consider when creating the new type.

```typescript
interface Student {
    age: number;
    firstName: string;
    lastName: string;
    school: string;
    grade: string;
}


type Person = Omit<Student, 'school' | 'grade'>

const person: Person = {
    age: 36,
    firstName: 'Matias',
    lastName: 'Hernández'
}
```

### Extract

This type will create a new type based on the properties present in two different types. It will **extract** from the type **T**  all properties assignable to type **U.**

Let's see an example. Here the goal is to extract all the arguments that are function types.

```typescript
type MixedArgs = string | number | (() => string) | (() => number)
type FunctionArgs = Extract<MixedArgs, Function>
```

The first argument we pass to Extract is our union type and the second argument is the type that we will use when comparing each member of the union. If a member is assignable to our second argument, it will be included in the resulting type.

But, how this will look like in real life?. A good use case will be creating a filtering function

Say you have an array of users with different types of users that are tagged by the `type` property, so in fact you have a **discriminated union**  as a type.

```typescript
type Users =
  | { type: "admin"; role: string }
  | { type: "customer"; company: string }
  | { type: "affiliate"; program: string };
```

Now, you want to create a function that takes an array of `Users` and return only the ones that match with certain `type`.

You can easily get away with something like this

```typescript
function filterUsers(users: Users[], type: Users["type"]) {
  return users.filter((item) => item.type === type);
}
```

But, there is a little issue with this function (even tho, the result will be correct at runtime), the return type of this function will be always `Users[]` even tho you know for sure that the result will be just one of the types.

Let's use `Extract` to create a better return type.

```typescript
function filterUsers<T extends Users, U extends T["type"]>(
  users: T[],
  type: U
) {
  return users.filter(
    (item): item is Extract<T, Record<"type", U>> => item.type === type
  );
}

const products: Users[] = []

const affiliates = filterUsers(products, "affiliate") //Result will be { type: "affiliate", program: string }
```

These seems a bit more complex so let's dissect it.
Now the function accepts two generics and use a type predicate inside the filter function allowing you to instruct Typescript about the specific type of the argument passed to the function when the function returns `true`.

The type that the filter function will narrow is `Extract<T, Record<"type", U>>` where `T` will be the `Users` array and `Record<"type", U>` will be an object like `{type: 'affiliate' }`

So the end result of the filter will be `{type: 'affiliate', program: string}`

### Exclude

This type works opposite to **Extract** by excluding properties that are already present in two different types. It excludes from `T` all fields that are assignable to `U` .

```typescript
interface UserBase {
  email: string
  image: string | null
  username: string
}

interface UserProfile {
  id: string
  email: string
  image: string | null
  isAdmin: boolean
  username: string
  reviews: string[]
}

type ProfileSpecificKeys = Exclude<keyof UserProfile, keyof UserBase>
// 'id' | 'isAdmin' | 'reviews'

type ExcludedTypes<T, U> = {
  [K in Exclude<keyof T, keyof U>]: T[K]
}

const user: ExcludedTypes<UserProfile, UserBase> = {
  id: '1234',
  isAdmin: false,
  reviews: [],
}
```

### NonNullable

This utility type will take a Type `T` and remove `null` and `undefined` from the allowed type values.

useful for instances where a type may allow for nullable values farther up a chain, but later functions have been guarded and cannot be called with null

```typescript
type NullableType = string | number | null | undefined

type NonNullableType = NonNullable<NullableType> // string | number

let numberOrString: NonNullableType = "string"

numberOrString = 10

numberOrString = undefined // error
```

It is important to note that this utility is not recursive, if you have a type with a nullable property, the use of the higher `NonNullable` will not affect the property, to do so, you'll need to overwrite the base type/interface

```typescript
interface UserBase {
  email: string
  image: string | null
  username: string
}


type UserBaseWithImage = NonNullable<UserBase>

const userWithImage: UserBaseWithImage = {
    email: 'email@email.com',
    image: null, // This is still nullable
    username: 'username'
}

/*
Extends and override the image property to make it non nullable
interface RequiredImage extends UserBase {
  image: NonNullable<UserBase['image']>
}
Same as 
*/
type RequiredImage = UserBase & {
  image: NonNullable<UserBase['image']>
}
```

There are a few more utility types that allows you to perform different tasks like:

- Parameters: Create a tuple type from the types used in the parameters of a function
- ReturnType: Creates a type from the return type of a function.
- String manipulation types: Uppercase, Lowercase, Capitalize and Uncapitalize. This utility types allow you to manipulate strings transforming string types.

### Mapped types

Is time to review a feature a bit more complex. **Mapped types** allows you to take an existing model and transform each of its properties into a new type, this handy feature allows you to keep your types DRY.

First step to understand this type of [metaprogramming](https://en.wikipedia.org/wiki/Metaprogramming) is to know why you want to use mapped types.

The main use case is when you need to create a type that is derived from another type, and to [remain in sync](https://en.wikipedia.org/wiki/Metaprogramming) with the original one.

For example, a permissions schema that defines an object that store the permissions that the user have to change configurations in the app.

What happen if you add new configurations to the schema? You'll need to also update the permissions type to acommodate the new configuration on it.

In this case would it be way better to have types that rely on each other to keep them in sync decreasing the maintainability effort.

How this could look?

```typescript
type Configuration = {
    layout: string;
    widthraw: string;
  	deposit: string;
	sell: string;
	buy: string;
}

type Access = {
	[Property in keyof Configuration as `change${Capitalize<Property>}`]: boolean
}

/* It creates
type Access = {
	changeLayout: boolean;
	changeWidthraw: boolean;
	changeDeposit; boolean
...
...
}
*/
```

This example combines several features of typescript, it uses:

- Indexed access types: You can access the type of a property by looking it up by name.
- Index signatures: Allow you to access a property even if you don't know the name of it by using the `[]` syntax (like you normally do with to dynamically access an object property in javascript)
- The **keyof** operator: This operator returns a union of the keys of the type passed to it. In this case the use of `keyof Configuration` resolves to `"layout" | "widthraw" | "deposit"....`
- Capitalize: An utility type that transform a string to the capitalized version of it, in this case it transform the `Property` name: `layout` → `Layout`
- And finally it use [template literal types](https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html) to create the name of the new property. This is a feature name as [key remapping](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#key-remapping-via-as)

### Template Literal Types

This types are build on the [string literal types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#literal-types) adding the ability dynamically creates a string.

It use the same syntax as javascript template literals.

```typescript
type MessagesID = "welcome_msg" | "logout_msg";
type AccountStrID = "account_tittle" | "menu_title";
 
type AllLocaleIDs = `${MessagesID | AccountStrID}_id`; 
// "welcome_msg_id" | "logout_msg_id" | "account_title_id" | "menu_title_id"
```

But the real power of this types comes when you need to define a new string type by deriving its value from information inside the type.

For example:

- A JSON parser with type safety

[Jamie Kyle 🏳️‍🌈 on Twitter](https://twitter.com/buildsghost/status/1301976526603206657)

- A dot notation getter with type safety

[GitHub - ghoullier/awesome-template-literal-types: Curated list of awesome Template Literal Types examples](https://github.com/ghoullier/awesome-template-literal-types#dot-notation-string-type-safe)

- A CSS Parser that you can toy around [in the typescript playground.](https://www.typescriptlang.org/play?#code/C4TwDgpgBAKgTgSwLYHUAWDgQMpgIYDGEAPNsHFBAB5YB2AJgM5SPkK0DmAfFALxQAoKCzadKNCA2Zk4QqAH4oAIgCicOAHs4SuQC4oM8XSZQABlAAkAb3YAzCBRkBfcwB8zAHVrW7Dg+Rc5RXhkdEwcfCJSch49f1kBUEgoAHEIYBhwEkNqY2ZWRE4efjkC9g4jSRMZIOU1TW04nIkpMx9aewoANTwAGwBXCCcwKlNa2n6kACM-OLLOAG4BRKyoAAU8OEYIAGFsbDXNMEZSStb57j5BYWwzk1N2zvWjp11Hvx6BoYX3igAlCCsQLCYSKKxyEFQADaAGsoOxYIhUBgsLhCCRDhowFwALr6NIZLLEELI8JoqKfQZcLgQqBOKAAMnWm22ewORxOANYNMh+n6tBhtA0AHdaMsVskCVzgGziNK7vlRJcSsISWFUZESNKeLkqswHjYOn4dhpaHRgE4nL8oH9pS5rgobfK4tKlhLoBsthBpbL5brzkrinI1SiIui5YDgDqWvdrSazZILVbDU9bZHgY7Paz9pjjsR4+aeVB9Bc3UkPSzdvtTv6TBcg8ILgqDLVVOotDphPoQ2TNaRo3k2injb08IxGAA5PBIIZQKzWu3mWrgyGw+G0RGhUPkkg7Ufjqcz3H6LPeyO+yNF+nuU+yqXn6vanglpVl1ZszKQZj8W-VsbCKYNHoEA5wdYQkDwKgAFphQQehgDQfQACYAAYRiWSEpkIGEOE0fl6CgggNF6LR9F6BAODQYApi+DC6WWYQADo0AARjnWkiJIuB9GFUM6KcBioAAYjAdjIVsU1gCg2xpwQXoQH0AA3Bx6DwWg8Do4QJLNKDGAQAAvCBkLQqh+IEUwaWWIjaFYKA+l6P4KKo-QPyyb8xKgQDgP0KxlCwggcLwhhCOIjt9CUOAIHoJQABplAg6DYPgtAlH0FiUJQukYrkJQRNS0ClG0qSZKQOSQHypRlLgVT1Ni5Qit0gyIHy9Kspy5iWPy3zONI5RePCJQ2oEgRrNs2Del6epetcr8rl8gB6ebnixBxQCgAByJQEpguCEKUdb4WYUrx3Kdc5C8hSCv8wKNHwkKuIqyLora4RcrALr6sk6TZPkiqqpqvA6sKr69MMlrMqcbLXo6j6eu4vrQ0GyGBCcIA)
- And many more that you can find in this [awesome repository.](https://github.com/ghoullier/awesome-template-literal-types)

### Conditional Types

This is another powerful typescript feature, the ability to create a type based on a condition. This is in fact the description of a type relationship test that select one of two possible types based on the outcome of that test.

It looks like this

```typescript
`T extends U ? X : Y `
```

That looks similar to the javascript ternary operator, you can also read this like this pseudo code

```typescript
if(T extends U) {
	return X
}else{
	return Y
}
```

Here the letters `T`, `U`, `X` and `Y` stands for arbitrary types.

The piece of code show in the conditional part `T extends U` describes the type relationship that is set to test.

Typescript use this feature to create the utility types like `NonNullable` , the code for that type looks like this

```typescript
type NonNullable<T> = T extends null | undefined ? never: T;

// That can be read if a rough pseud-code like

typefunction NonNullable(T) {
	if(T === null || T === undefined) {
		return never;
	}
	return T;
}
```

This type selects the `never` branch if the type `T` is assignable to either `null` or `undefined`, otherwise it keeps the original type `T`.

Let's check another example use case for conditional type. Let's image that you want a way to figure it out if a string is trimmed, meaning it doesn't have empty spaces at the end nor the beginning. How can that be done?

Now let's see how to implement this type, let's start by checking if the passed string have empty spaces at the end. You'll need a way to say:
"Check if the string contains a   at the end. This can be done by using the keyword `infer`

```typescript
T extends `${infer R} `
```

The `infer` keyword is a compliment to conditional types. It cannot be used outside of an `extends` clause. `infer` allows you to define a variable within the constraint to be used or referenced later.

So in the previous code snippet, the type `infer R`  is extracting the type of the `T` generic and since is inside of a template literal, is constructing a new string with an empty space at the right side.
It can be read as "Is T the string with empty space at the right"?

With that, let's create the type `TrimEnd`

```typescript
type TrimEnd<T extends string> = T extends `${infer R} ` ? TrimEnd<R> : T;

// pseudo code

typefunction TrimEnd<T extends string> {
	if(T extends `${infer R} `) {
		return TrimEnd<R>
	}
	return T;
}

// more pseudo code

typefunction TrimEnd<T extends string> {
	if(T.endsWith(' ')) {
		return TrimEnd<R>
	}
	return T;
}
```

So, this type accepts a generic that `extends` (is constrained to) a `string` . Is a conditional type that check if `T` is the string with empty spaces at the right side (this is done dynamically thanks to the template literal type).

If the condition is true, then it pass the type again to recursively finds a string that doesn't ends with a space.

Now, you need to check if the string starts with an empty space. That type is the same but opposite of `TrimEnd`, let's call it `TrimStart`

```typescript
type TrimEnd<V extends string> = V extends ` ${infer R}` ? TrimEnd<R> : V;
```

And with this two types, you can finally create the T`rimmed` type required

```typescript
type TrimStart<V extends string> = V extends ` ${infer R}` ? TrimStart<R> : V;
type TrimEnd<V extends string> = V extends ` ${infer R}` ? TrimEnd<R> : V;
type Trimmed<V extends string> = TrimStart<TrimEnd<V>>;


const password = " this is the password with empty spaces at the end and begin "


function collectPassword<T extends string>(str: Trimmed<T>) {
    return str;
}

collectPassword("this is the password")

collectPassword(" invalid password ") // This triggers an error
```

[Demo for this code](https://www.typescriptlang.org/play?ssl=15&ssc=38&pln=1&pc=1#code/C4TwDgpgBAKgTgSwLYGVgEM7ADwDUoQAewEAdgCYDOUlwipA5gHxQC8U+RJF1ABlABIA3glIAzCHCgAlAL78A-LESoMWbNJYAuDgG4AUKEjLkAUQp4CxMlRp1RzNhyvdb-YaIlS5ik0nPkGtp6huDQ8MhIEIGc1jx29I7sEaqYOCkBeExMBvr6AMYA9qS0UGDolJQA7oVw5E4ARFDAABYI1O3NLdDllTV1UFUIrQRIYKA05fkQ1OjAXdA2UOgUUABGEAyiUA15+mIArqT5wAjFUEUANpcQJwAKFdW1gTAuNtS0iUwAFJ86KVEXkwAJRQIT6KCQqBwCDAA5wUgJAyyPJXG73R79cjfBqtTqdVo9THPBrA1GFa63YAPPrPHFQUQAN3QlwQ9V6TwGpKAA)

This type use a combination of powerful typescript features: Template Literal Types and Conditional Types. You can mix and match as many typescript features you need and want to create your own utilities and complex types.

Typescript is a fully fledged language that is not just to add some types into the loosy Javascript, but can help you model complex data requirements and avoid several types of bugs.
