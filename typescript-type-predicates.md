# What are Type Predicates?

A variable can be many types.

You need to check what type the variable/value actually is.

One example can be a variable/value defined as union of `X` and `Y`. meaning that the variable/value can one or the other 

> Example a state object representing an async operation wich result can be loading, success or fail. 
>

The process of getting to know wich type a value/variable is is known as "narrowing".

## When narrowing is required

```ts 
type Dog = {}
type Cat = {}

type Pet = Dog | Cat 

function doesBark(pet: Pet) {
  // check if pet is a dog, then return true 
  // return false if is a cat 
}
```

To narrow this you can use different "methods" or approaches 

### Inline check 


```ts 
type Dog = {
  bark: true
}
type Cat = {}

type Pet = Dog | Cat 

function doesBark(pet: Pet) {
  if("bark" in pet) {
    return true
  }
  return false
}
```

You can use the `in` operator or the `hasOwnProperty` method to check if certain property exists on the object and decide based on that what to return.
> You can't just do `if(pet.bark)` in TS since TS will fail and show an error if you try to read a property that do not exists.

- Quick to do
- TS ensure your checks are enough to narrow the type
- No need to change the existing data/types
-
But 
- Can be messy and noisy 
- Isn't reusable 
- Some checks can go complex 

### Type Predicate 

This is a reusable piece of code, a function that returns a boolean if the argument match certain rules. But it has a catch, it use some TS only sintax to declare that this function
will be used by TS to narrow the type.


```ts 
type Dog = {
  bark: true
}
type Cat = {}

type Pet = Dog | Cat 

function isDog(pet: Pet): pet is Dog {
  return "bark" in pet
}

function doesBark(pet: Pet) {
  return isDog(pet)
}
```

The predicate function is the `isDog` and the special sintax is that part after the semicolon as a return type `pet is Dog`. You can read that as, "if this function returns true then it means that the argument `pet` is for sure of the type `Dog`"


Advantages 
- No need to change your data or types 
- Encapsulates the logic in a DRY way

Disavantages 
- It could require multiple function for each case 
- The checks can grow out of control 
-
This last point is dangerous and important, so warrants further explanation on how this approach can fail.

The first way this could fail is that we just make a mistake in our predicate:

```ts 
function isCat(pet: Pet): pet is Cat {
  return "mcirochip" in pet // 🔥 spelling mistake - no cats ever!
}

```

TypeScript will not warn us about this! Whenever we assert a type like this, we're telling the compiler that we know best and to trust us. In this case, that trust is misplaced!

This could also happen if we spelled it correctly, but then changed the name of the microchip property, perhaps to microChip. Now we have to remember to change things in two places whenever we change our types, and TypeScript isn't going to help us remember!

The second way that type predicates can go wring is even more pernicious!

Let's say we now add another type of pet to our union:

Now, our type predicate for is Catis wrong because cats don't know about the existence of hamsters. This is going to cause crashes at some point.

How can we get the benefits of keeping our code clean, modeling our distinct types correctly, and making sure TypeScript helps us with anything that changes? 


### Discriminated Unions 

Another way that typescript allows you to narrow types is by using a fancy-term around unions. Discriminated unions.

This is basically adding a property to each of the types that belongs to an union saying what type it is.

That property is called a discriminant and it's often called type or kind but that is merely a convention. There are cases where other names are more suitable (see the section on async requests, later).



```ts 
type Dog = {
  bark: true
  kind: "dog"
}
type Cat = {
  kind: "cat"
}

type Pet = Dog | Cat 

function doesBark(pet: Pet) {
  return pet.kind === 'dog'
}

```

Advantages of discriminated unions:

Our checks become dead simple – "reusability" is as simple as checking a property
If we add a new item to the union, we're forced by TypeScript to handle all the cases where we use this type downstream
We get autocomplete hints for the discriminant values
We are forced to model our domain more carefully, which leads to clearer, more robust programs
Disadvantages of discriminated unions:

We need to change our types if we are applying this to existing code

Conclusion
Discriminated unions are one of at least three ways of figuring out which specific type you have when presented with a union. They require more modeling of your domain up front, and require changing your existing types, but they confer some big advantages over other approaches.

These advantages go beyond distinguishing between types, pushing us towards more robust, maintainable solutions where we offload the job of remembering important details about our codebase to the compiler.

