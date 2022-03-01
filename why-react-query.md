# Why we choose React query as a server state management solution.

# What is state management?

There is a certain agreement in the developer community about some topics that are commonly seen as big problems: naming things, cache, and state management.

When building UI and user experiences through web applications one of the biggest struggles is about how to deal with the application state.

**But, what is state and why does it need to be managed?**

The process of web development deals with a pretty complex environment. The browser offers the developer a set of mutable nodes in a tree form: the DOM. Several frameworks came to light across the years to handle and work with this structure, one that stands out as a pioneer of the second era of Javascript is React.

The main rationale behind React is that to manipulate the DOM we need to write imperative javascript code. React came to say that we can do better: There is a way to write **declarative code** to create the UI.

> The UI is a function of the state.

In other words, **state is a data structure that represents a snapshot of the reality in the application interface** or more simply, is a programmatic representation of what the user sees at a certain moment.

The state is the expectation that the user has for how the application should work, this is part of the constant battle that we fight when adding state to the applications. “Is what the user’s seeing exactly one to one with what the state internally represents?”.

# What problems do we need to solve?

Manage state is a known problem in computer science, there is a big branch of research that created a concept called finite state machines, a mathematical model that represents a "machine" that can be in exactly one of a finite number of states at any given time. The machine can transition to one state or another in response to some inputs. This definition is exactly what we want from our interfaces.

> Check out [xstate.js.org](http://xstate.js.org/) to learn more about state machines and javascript.

> If you are interested in implementing this pattern with React [check out this article](https://kyleshevlin.com/how-to-use-usereducer-as-a-finite-state-machine) to learn how to work with it using the `useReducer` hook

Now, where does the state comes from? The state of our applications can be described as a mix of two types of data.

- **UI state**:  The kind of state that is derived from the user interaction with certain pieces of the UI, like Modals, alerts, even authentication. We have complete control and ownership of this state. The type of state that gets killed every time you refresh your browser. It is synchronous and easy to retrieve.
- **Server state or Cache state**: The kind of data that is remotely persisted, asynchronous with shared ownership that can be potentially Out of Date

React is in its core a **state management solution**, it offers the required tools to manage the state that belongs to the UI by itself but when we need to deal with what we call **server state** then React is not enough and we need to go out and look for a 3rd party library solution.

**Server state** presents new challenges to deal with like Caching, dedupe requests, incremental fetching, mutations, outdated requests, synchronization of the data.

In the early days of React and the Flux pattern, the solution for all this (and a few other quirks like prop-drilling) was just to mix and merge these two states and put it high on the app creating a **global state**. This is what libraries like Redux do but that was not enough to deal with the different natures of the state. A lot of developers experience the increased complexity of this type of global state solution.

So in summary:

- Server state is data that we don’t control. It is a snapshot of data that some API returned.
- UI state is predictable, easy to manage, and to reason with.

> Watch this talk from Tanner Linsley: It's Time to Break up with your "Global State” for more insights about it [https://www.youtube.com/watch?v=seU46c6Jz7E](https://www.youtube.com/watch?v=seU46c6Jz7E)

## Why do we need a 3rd party library?

React is just a tool to manage the UI so out of the box it doesn’t offer an opinionated solution for fetching or updating external data sources. The solution is to build this tooling by yourself but that usually means a lot of boilerplate, ceremony, and pain points of possible failure.

So when starting a new project we need to think about the architecture of the app and how we will deal with the problems to come, based on research and previous experience we know some of the common problems that we were about to face when dealing with an API.

As stated previously we consider that the state has two natures and that for each of that natures we need different solutions.

One concept that resonates with us is the idea of Colocation: **Place code as close to where it**’**s relevant as possible.**

The idea here is to drive the UI state with this principle and do it in a React-y way. By using the core composition model of React, the [state lifting](https://reactjs.org/docs/lifting-state-up.html) pattern and the Context API.

> React offers a very good abstraction layer: Hook. Check out this talk about custom hooks. [https://www.youtube.com/watch?v=J-g9ZJha8FE](https://www.youtube.com/watch?v=J-g9ZJha8FE)

There is a ton of good state management solutions there even some that are tailored for Graphql communication, some of the big ones are Apollo, SWR, Redux Tool Kit, Mobx-State-Tree, and React Query.

There are a few questions that you should ask before choosing a tool like:

- Knowing how my state could look: Who owns these pieces of the state? Do I own this state in the browser or doest it belong to something else?
- Can I manage all of the operations by myself? or should I not worry about this task and focus on the business logic?

We decided to go with an agnostic and strong layer to manage server state: [React Query](http://react-query.tanstack.com/).

# What is React Query?

React Query is a library created by Tanner Linsley specifically to manage server state and cache, it's defined as a thin cache layer with a simple API surface.

The principle of the architecture of this library is the encapsulation of business logic by extracting the data fetching ceremony to custom hooks.

> Check out this article about encapsulation with custom hooks: [Use Encapsulation by Kyle Shelvin](https://kyleshevlin.com/use-encapsulation)

This pattern offers a simple API surface based on a collection of a few custom hooks that can be used directly in your components accomplishing the co-location of state, but also, since they are just functions we can do better and encapsulate the usage of this to create better naming patterns and data manipulation.

One of the features of this library is that works out of the box without any specific configuration, the only thing that it needs is to receive some function that returns a Promise. This simple feature enables many possibilities: Since it is not tied to a certain technology stack you can use react-query with any type of data as far as the function to retrieve it is a Promise-based API and returns JSON structure.

### How React Query Works

Let’s review the basic example from the official documentation, which is a very simple application that retrieves the stats from a GitHub repository by using the fetch API. It is a very contrived example but is enough to show the basic out-of-the-box features.

```javascript
import { QueryClient, QueryClientProvider, useQuery } from "react-query";

const queryClient = new QueryClient();

export default function App() {
  return (
	<QueryClientProvider client={queryClient}>
	  <Example />
	</QueryClientProvider>
  );
}

function Example() {
  const { isLoading, error, data, isFetching } = useQuery("repoData", () =>
	fetch(
	  "https://api.github.com/repos/tannerlinsley/react-query"
	).then((res) => res.json())
  );

  if (isLoading) return "Loading...";

  if (error) return "An error has occurred: " + error.message;

  return (
	<div>
	  <h1>{data.name}</h1>
	  <p>{data.description}</p>
	  <strong>👀 {data.subscribers_count}</strong>{" "}
	  <strong>✨ {data.stargazers_count}</strong>{" "}
	  <strong>🍴 {data.forks_count}</strong>
	  <div>{isFetching ? "Updating..." : ""}</div>
	</div>
  );
}
```

What we can see here is that to enable the use of react-query we just need to add the corresponding client provider high in the component tree and then just use the provided hook `useQuery` to perform the task.

In the `Example` component we can see the use of this hook. `useQuery` accept a function that returns a promise, in this case, the `fetch` API has been used. And returns a set of values that helps you to know the different states of the fetching process.

There are two important things to notice here:

1. react-query handles the whole state of the fetching process as a state machine, meaning that is not possible to fall into an impossible state.
2. And, the first argument of the `useQuery` hook is a string (can be an array too). This represents a **cache key** that is used to uniquely represent the data inside the cache layer to be referenced later.

> Check out this talk from Richard Feldman : [Making Impossible States Impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8) and[ this article from Kent C Dodds](https://kentcdodds.com/blog/make-impossible-states-impossible) about the same topic but React focused.

How the same example could look without react-query?

```javascript
import React from 'react';
export default function App() {
  return (
	  <Example />
  );
}

const initialState = {
	data: undefined,
	isLoading: false,
	isError: false,
	isFetching: false,
}

const reducer = (state, action) => {
	switch(action.type) {
		case 'TRIGGER_FETCH':
			return {...state, isFetching: true, isLoading: true }
		case 'PARSING':
			return {...state, isFetching: false, isLoading: true }
		case 'DONE':
			return {...state, isLoading: false, data: action.payload }
		case 'ERROR':
			return {...state, isLoading: false, isError: true, error: action.payload }
		default:
			return state
	}
}

const useGetData = () => {
 const [ state, dispatch ] = React.useReducer(reducer, initialState)
 React.useEffect(function fetchEffect(){
    dispatch({ type: 'TRIGGER_FETCH' })
    fetch(
	  "https://api.github.com/repos/tannerlinsley/react-query"
	).then((res) => {
        dispatch({ type: 'PARSING' })
        return res.json()
    }).then(data => {
        dispatch({ type: 'DONE', payload: data })
    }).catch(error => {
		dispatch({ type: 'ERROR', payload: error })
    })
     
 },[dispatch]}
 
 return state
}

function Example() {
 
  const { isLoading, error, data, isFetching } = useGetData()

  if (isLoading) return "Loading...";

  if (error) return "An error has occurred: " + error.message;

  return (
	<div>
	  <h1>{data.name}</h1>
	  <p>{data.description}</p>
	  <strong>👀 {data.subscribers_count}</strong>{" "}
	  <strong>✨ {data.stargazers_count}</strong>{" "}
	  <strong>🍴 {data.forks_count}</strong>
	  <div>{isFetching ? "Updating..." : ""}</div>
	</div>
  );
}
```

there is a lot more boilerplate and points of failure and this needs to be replicated every time you want to fetch, obviously you can extract this to a custom hook and reuse that but this does not offer a cache layer, meaning that you will always be failing into loading states and possible errors without fallbacks.

react-query will solve those problems by managing the cache giving the app instant refresh and background process to update the data in place.

# How we use React Query

In our particular project, we use a graphql layer for our data needs, to manage that we created a set of custom hooks that wrap the calls to react-query hooks for each “feature” of the application, and for the fetching requirement, we use the simple client `graphql-request` since all of the cache requirements are met by react-query.

We also do heavy use of **Optimistic Updates** this is we update our state/cache even before some mutation is done.

The idea behind this decision is to create a fast and smooth user experience. Since in most of the cases we know what the data will look like after the mutation we can just update the UI to represent that change.

This is possible because of the cache layer implemented by react-query. Since the data is cached and also associated with some unique key we can tap into it to:

1. Cancel ongoing queries that can affect the optimistic update.
2. Write the new data into the cache in the exact place we need it.
3. Rollback the mutation in case of error.
4. Silently update the data when the mutation succeeds.

### What does that look like?

This is again a contrived example that you can find in this [github repository](https://github.com/matiasfha/react-query-example) there are multiple branches to showcase the different step to move from a simple `fetch` approach to use-query. Check **main** branch for the last step and [check the PRs](https://github.com/matiasfha/react-query-example/pulls) to see the different steps to go from just using the base hooks to acommplish the optimistic update behavior.

This is a very simple nextjs application that uses airtable as the backend to enable a TODO list (yes another TODO list app). The idea is very simple. An application that fetches a list of items that can be toggled between different states (UI state). Each state change is also saved back into the database.

It also allows the user to create a TODO item that is also saved in the backend, finally, the user can also navigate to some of the items to check more information about them.

Now, back to the optimistic update example.

This is the implementation of a custom hook to wrap the mutation process from react-query, the very same approach we use in our project.

```javascript
import React from 'react'
import axios from 'axios'
import { useMutation, useQueryClient } from 'react-query';

export default function useCreateTodo() {
	const queryClient = useQueryClient();
	return useMutation(todo => axios.post('/api/todos', todo).then((res) => res.data),
		{
			onMutate: async newTodo => {
				await queryClient.cancelQueries('todos');
				const prev = queryClient.getQueryData('todos');
				queryClient.setQueryData('todos', old => [...old, { newTodo, id: Date.now() }]) //add the new item to the cache
				return { prev } // return the previous data to be used later inside the context
			},
			onError: (err, newTodo, context) => {
				queryClient.setQueryData('todos', context.prev); // set the cache to the previous data
			},
			onSettled: () => {
				queryClient.invalidateQueries('todos') // refetch
			}
		})
}
```

This is the create todo action, it uses the `useMutation` hook from react-query to call the corresponding API endpoint. The **magic** happens with the lifecycle hooks passed as options to `useMutation`.

- `onMutate` is triggered before the mutation function is fired. It receives the same variables that the mutation function receives. It can return a value that can then be passed to the `onError` and  `onSettle` function as `context`.
- `onError` is fired when the mutation call fails.
- `onSettled` is fired when the mutation process ends not matter if it fails or succeeds.

With these options we can orchestrate the optimistic update in a very simple way:

- `onMutate` we cancel any query related to the cache piece that we are about to updated using the query `key.`
   - Then, we read the data from the cache using the query key.
   - Finally, we update the data by pushing the new `todo` into the array and faking the `id` since we still don't know it.
- `onError` if some error happens we just roll back the update by accessing the `context` value of `prev` returned from `onMutate`
- `onSettled` at the end of the process we just “refresh" the corresponding piece of the cache to show the saved data into it.

# Conclusion

In summary, the state management problem is still something that needs to be handled project by project but certain common points enable us to create a model and look for solutions that enable us to better handle the different pieces of the state.

We consider that the state is compromised of two pieces: UI State and Server State.

- UI State: can be handled by React itself by using the powerful hooks API: `useState`, `useReducer` and `useContext`.
- Server State: It has a lot of moving parts that can lead to multiple pain points so we decided to use a battle-tested and top-notch library to handle it. The library offers a lot of options and features but requests zero-config and almost zero maintenance.

## **Resources**

If you are interested in learning more about state management check out these resources:

- Video: [Let's learn state machines with David K. Piano](https://www.youtube.com/watch?v=czi24DqUfSA)
- Playlist: [React State Management in 2021](https://egghead.io/courses/react-state-management-in-2021-6732) a series of interviews and chat with different industry experts conducted by Joel Hooks from [Egghead.io](http://Egghead.io)
- A course: [Build Advanced Component with React Hooks](https://egghead.io/courses/build-advanced-component-with-react-hooks-cd6a) by Matías Hernández that showcase how to use custom hooks to manage component state.
- Articles:
   - [Application State Management with React](https://kentcdodds.com/blog/application-state-management-with-react)
   - [State Colocation will make your React app Faster](https://kentcdodds.com/blog/state-colocation-will-make-your-react-app-faster)

