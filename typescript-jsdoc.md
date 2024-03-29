Did you know that you can use comments instead of code to write the Typescript types for your app?

You can use JSDoc to describe the methods and variables and your editor of choice will pick up the type information from there.

## But, what is JSDoc?

JSDoc is a markup language used to describe Javascript code. In the era before Typescript comments were the unique way to define information about, not only usage and description,
but also type information.

JSDoc is a standardization of that practice of adding comments to describe and document your code, it's a spinof the [Javadoc ](https://en.wikipedia.org/wiki/Javadoc) scheme,
but specialized for Javascript code.

```js 
/**
 * Renders a new component with props
 *
 * Used by external plugins
 *
 * @param  {Object} options
 * @param  {String} options.action          an action to execute
 * @param  {String} [options.errorMessage]  an optional error message to show 
 * @return {Promise<string>}                a string with the content 
 */
static async renderComponent({ action, errorMessage }) {
  return template({ action, errorMessage })
}
```

Most of current code editor supports this syntax to read and display documentation about the code, go ahead and try it out.

## Typescript into the game 

The Typescript team took JSDoc and added supports for the type information. Is it possible [to use a variation of the JSDoc annotation syntax](https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html) to provide 
information about the types you will use.
Let's take the previous example but this time with type annotations 
```js 
/**
 * Renders a new component with props
 *
 * Used by external plugins
 *
 * @param  {String} action          an action to execute
 * @param  {String} [errorMessage="default message"]  an optional error message to show (with default value)
 * @return {Promise<string>}                a string with the content 
 */
static async renderComponent({ action, errorMessage }) {
  return template({ action, errorMessage })
}
```

Looks  similar right? That's why TS "hijacked" some of the JSDoc syntax and added new meaning, but the real power comes when you define new types.

```js 
/**
 * Renders a new component with props
 *
 * Used by external plugins
 * 
 * @typedef {Object} Props 
 * @property  {String} action          an action to execute
 * @property  {String} [errorMessage="default message"]  an optional error message to show (with default value)
 *
 * @param {Props} prop 
 * @return {Promise<string>}                a string with the content 
 */
static async renderComponent(props) {
  return template(props)
}

```

In this case, the `@typedef` annotation was used to create a type named `Props` and then the `@property` annotation is there to declare each of the properties that belongs to that object type.
After that, you can  use the new type inside the **same file** were it was defined (unless you export the declaration).

The result of the previous snippet is the same as 

```ts 
type Props = {
  action: string;
  errorMessage?: string
}
static async renderComponents(props: Props): Promise<string> {
  return template(props)
}
```

## Why use JSDoc instead of Typescript?

As everything in tech (and in life) **it's depends**. There's a range of possible use cases.

One possible use case (and the one I consider the most) is: You're writing simple node scripts and you want some type safety while coding. Since node doesn't support Typescript out of the box (yet),
you need to go through some setup to add it, and maybe, the script is little enough and it doesn't worth the effort (and the compile time).

Other option is you may want to start getting your hands dirty with type safety but without fully committing or without fully migrating your project, you can add this JSDoc comments 
and start enjoying a better life.


Or, maybe you have a similar thinking as webpack team (a large Javascript codebase), they decided to have type-safety but without a compile step.

Lastly, one good reason to use JSDoc is for documentation generation, there are packages that use JSDoc comments to generate documentation sites that can be simple deployed.

## Downsides?

OK, I convince you to use type annotation through JSDoc, is there any downsides to this approach. And the answer is yes, there is. If you are a library author, then this approach is not for you.

JSDoc type have a strong TS support but is not yet on part with Typescript, not all Typescript syntax it is supported.

