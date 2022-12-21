# Why we choose Typescript all the way through

What can be a significant improvement for your javascript developers team? What can take them to the next level of confidence and productivity? Typescript can be that kind of change that can leverage a new whole world of improvements in developer experience, but it comes with some learning curve.

In this article, I will run you through some resources about Typescript, how this can give your team more confidence and productivity, and some examples about using Typescript with React.

Let's check some data about Typescript. In the results published in the report "State of Frontend 2020," there was a question "Have you used Typescript during the last year," and an overwhelming 77% answered, "Yes" (The survey was responded to by 45000 developers).

Another great metric is the result of the question "Do you like Typescript better than Javascript" where 54% said that they prefer Typescript the most.

![Screen Shot 2021-06-09 at 11.39.01.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/FE36839F-4382-4C24-A302-28ABD6F8A078_2/Screen%20Shot%202021-06-09%20at%2011.39.01.png)

There is another source of metric that you can check, the famous State of JS, where you can see that Typescript usage grows from 66% in 2019 to a gigantic 78%! . Similar metrics for the Satisfaction question.

![Screen Shot 2021-06-09 at 11.41.59.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/BCDF421F-DB51-42DF-82E2-45D09CBE11D4_2/Screen%20Shot%202021-06-09%20at%2011.41.59.png)

So is easy to notice that Typescript is a big player in the web development world, but what is all this buzz about?

## **What is Typescript?**

Typescript is a compiled language, can be described as "Javascript for application-scale development."

Typescript (TS) is a strongly typed language designed by Microsoft as a superset of Javascript. TS delivers a language and a set of tools. What that even means? Typescript is just javascript but with static typing.

But Typescript can't be executed in the browser. Browsers only know how to parse and execute javascript code; for this reason, typescript ships with a compiler (tsc) that reads your ts code, perform a type checking, and all went okay output performant javascript code.

So, how Typescript code looks like? (extracted of some of our own codebases)

```typescript
export type WizardState = {
    activePageIndex: number;
    steps: number;
};

type WizardProps<T extends WizardState, A extends WizardActions> = {
    reducer?: typeof wizardReducer;
    initialState?: Partial<T>;
    children: React.ReactNode;
};

type ButtonProps = {
    as?: 'button' | 'div' | 'a' | 'span';
    children: React.ReactNode;
};

export const defaultInitialState = {
    activePageIndex: 0,
    steps: 0,
};
function defaultReducer<T, A>(state: T, action: A): T {
    return state;
}

export function wizardReducer<T extends WizardState>(state: T, action: WizardActions): T {
    const { activePageIndex } = state;
    switch (action.type) {
        case ActionTypes.NEXT_PAGE:
            return { ...state, activePageIndex: activePageIndex + 1 };
        case ActionTypes.PREV_PAGE:
            return { ...state, activePageIndex: activePageIndex - 1 };
        case ActionTypes.SET_STEPS:
            return { ...state, steps: action.payload };
        default:
            return state;
    }
}
```

As you can see, it can be more verbose than plain old javascript with all of that type definitions but at the same time is kind of similar to what we were use to write right?

## **What can Typescript do for my team and me?**

In any software project, we will have bugs, which is unavoidable. Still, we all strive to find ways to minify that number or catch them before reaching the production environment and affecting the users.

These errors can be in many forms; some can just be typos or genuine implementation issues or nuances from the programing language itself.

Javascript is a really, really flexible dynamic language, where you can basically do whatever you want without any restriction. You can mutate any variable, pass any type of data to every function, etc. This can be seen as a powerful feature since the barrier to write javascript code is really low. Still, if you are crafting big applications with complex logic, javascript will be a "foot gun" by itself; no matter how experienced your team is, errors will come.

One way to avoid these errors is by testing your code: unit test, integration, and e2e test will create some confidence degree and help you not ship these bugs to production but avoid them in the first case.

### **Typescript is a Test**

Typescript can be considered part of what is known as "statical analysis tools," which means that typescript processes verify your code without actually running, checking the data flow based on the types.

Typescript compiler will parse your code, create an AST and then traverse the pieces to check how they communicate with each other.

This process can be considered some type of test. The process checks your code to avoid bugs and avoid that they are even committed to the main branch.

A very constrained example is: Imagine you have a function that calculates your company's earnings, you expect that this function accepts numbers, right?. But what happens if some unknown bug creates string data and passes that as an argument to your function? It will break, and you would even notice until the tests are run or manually tested, or worst until it hits production.

How Typescript could save you here?

```typescript
function calculateEarnings(data: number[]) {

	// perform the complex and awesome logic

	return earnings;	
}
```

Typescript will yell to you that the arguments you are passing are strings and can't be used by your wonderful function by just giving the type of the expected data. And this will happen while you are writing the code!

So you can consider that using Typescript is like running an ever-ending test that oversees what you write to not ship low-quality code.

### **Confidence**

What you gain when running tests on your code? Confidence.

Confidence that your code does what it suppose to do. Since Typescript is "testing" (or checking) your data flow, you can focus on design the correct solution for your problem and be sure that the algorithm that you crafted will do what it suppose to do without fiddling with checking the valid arguments or the right data is returned.,

This confidence will actually lead to a better maintenance and improvement experience. Since Typescript will check the data types, you can rely on the type system for big refactors without errors (only the ones related to the business logic). The use of types will invalidate almost every "silly error" that can sneak into Javascript codebases (undefined is not a function sounds familiar? )

### **Better coding experience**

Another win of Typescript for developing a "Javascript" application is the significant improvement in developer experience (well, there are a few trade-offs that we will check later in the article).

As a developer, you spend most of your time sitting in front of your screen staring at your code editor. The idea of using a code editor is to make your life easier and be more productive by leveraging some tasks to the dev environment. Some of these tasks are related to the language itself, and as was mentioned before, Typescript allows static code analysis tools to run. This tooling around Typescript offers immense features for your "developer experience," like:

- **Code auto-completion:** Most code editor will auto-complete what you are typing, allowing you to write faster and confidently, but if we are talking about javascript, this auto-completion will be just some pieces of the language and maybe, a list of some of the current files used keywords like variables names and other. But with Typescript, since it is a static type language, the code auto-completion goes one level up, showing you documentation of every piece of code you write.
- **Real-time type checking:** Most IDE and code editors will run a type-checked in the background reviewing the code you write and marking the type errors even before you save or run the code. This will help you to debug your code and be more confident about what was written.
- **Better refactoring experience**: Refactor is one of those tasks that as a developer you expect to come, but at the same time, kind of hate it because of the lack of confidence that you get when updating code with vanilla javascript, but again, since Typescript allows static analysis the code editor can easily rename variables or functions, move files and the related imports, etc., helping a lot in the process. Also, if the algorithm you wrote didn't change the logic, you can be confident that the code works since Typescript will show you if some data type (and, therefore, data flow) is incorrect.

### **Better project quality**

Most software projects involve more than one developer, and the coordination between this team in terms of coding styles and good practices can be cumbersome. Still, by leveraging the power of Typescript, most of these tasks can be forgotten.

The project maintenance and code quality can be trusted to the type analysis allowing the project to grow to keep the risk low.

Another big win in this area is that types can be seen as code self-documentation. Not every developer is good at commenting on the code, and using JSDoc (that is actually good) is not always a reality for most teams. Typescript types help in this case by "documenting" the expected arguments and output of a function or method, describing the shape of an object, helping to keep some sanity and a clear mind when working with unknown code.

Choosing typescript forces and helps developers have a data-driven mindset; this means that the implemented solutions often start by describing the data types that will be used (input and output) and then the corresponding algorithm to perform that data transformation. This approach will help spot and solve problems much earlier than vanilla javascript.

## **Typescript PitFalls**

Like everything in life, "Not all that glitters is gold," and there are some pitfalls where maybe the biggest one is a stuttering learning curve.

Most typescript developers started as javascript developers, and switching from the full dynamism of vanilla javascript to the strictness of (strict) Typescript can be difficult. When starting the project, you can think that you are writing more code than the one required before, or feel that the types are in your way of being productive, or even be surprised or scare by the TS errors, but trust me, it gets easier.

There are many nuances in Typescript, and that can be a step to learn. There are different techniques to define data structures, and some of them can become really complex quickly, but it will pay.

Another pitfall is that Typescript allows you to slowly and increasingly add types to your project; this can be good when you are starting, but at same type can be a foot gun. This lack of strictness allows you to just make mistakes, use wrong type definitions, or simply don't define types for particular pieces of the code. If you are doing this with your own code, you can work around this lack of confidence by knowing what you are doing and using a good test suite, but what happens when using a third-party code? You can get wrong types or lack of it from external sources, which will end in Typescript not being 100% reliable.

But, be easy!. Most popular open-source libraries are fully typed and have a strong community keeping an eye on the types; you can always check the source code or the types source code to be sure.

## **Conclusion**

As you can see, Typescript has some cons but at the same time big and robust pros. Using Typescript in your next project can help a lot by improving the confidence and code quality. Learning Typescript will be really worth it for your career and projects.

The few pitfalls or disadvantages can be considered minors when you look at the overwhelming powers offered by its usage. This is why we choose Typescript for our web development projects, both in our applications' front-end and back-end.

