# Why XState is THE state management tool that you're missing out.

Developing user interfaces is hard, and we know it.

Today, developing a UI is all about building representations of a specific state and communicating the state changes using components. You could argue that user interfaces are a type of graph, or in other words, they can be connected to other things in a non-hierarchical fashion.

This opens Pandora's box, creating several possible states with different use cases and numerous edge cases.

So, the main work that web developer does is manage the state change. These changes trigger the rendering process updating the UI accordingly. And there is a lot of tools to perform this task.

# What is XState?

According to XState's documentation

> XState is a library for creating, interpreting, and executing finite state machines and statecharts, as well as managing invocations of those machines as actors.

Sounds pretty complex, right? Well, it should not be that way. Let's break that down!

In a nutshell, XState is a library that builds on top of essential concepts of Computer Science. XState helps you define - in a declarative way - all of your application's possible states or business logic and then interact with it with ease and confidence.

Those foundational Computer Science concepts are [Finite State Machines](https://academic.csuohio.edu/chu_p/rtl/chu_rtL_book/rtl_chap10_fsm.pdf) and [State Charts](https://www.sciencedirect.com/science/article/pii/0167642387900359). At its core, a finite state machine is a mathematic model that describes the **behavior** of a system that can only be in one state at any given time. It sounds similar to how a UI can only represent a single state at a time.

A State Machine defines a finite number of states, a limited number of events, and a set of transitions between states that are triggered by the defined events. This provides predictability about what state is the next one.

If you're familiar with React, you can think of this [like `useReducer`](https://reactjs.org/docs/hooks-reference.html#usereducer) on steroids.

In the other hand, we have [State Charts](https://pdf.sciencedirectassets.com/271600/1-s2.0-S0167642300X00603/1-s2.0-0167642387900359/main.pdf?X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLWVhc3QtMSJIMEYCIQCJjyWzNZvANBm6K4tLF54B67v7Ildn%2B3QiJQlg9SKmZAIhAIKVRd19gtnqxSKW%2BHDATOREM6MS6jfkQtVJMXEkilspKvoDCCsQBBoMMDU5MDAzNTQ2ODY1IgwNMkH%2FG3xOaVXUN%2Foq1wMX3%2BK9SXzIEBAi%2B6avvymJ5mDX5jp8axiRFLZy0WQv0nd%2BxqV6tKIvOJ8MDgK9BGuOgPWywI%2Bw753nFbQrKTeQjAFsh%2BvT%2Fn0BrbBBIQU%2FGwhVsAmG8jUA8FcnDJ1y%2FsRGuRmH%2F184FkEYey7w2sEZ%2FA%2B2DOYRwyBIz%2BbgQA89jxOqmnHKFifOsNCm7%2FlVR6SewXxoTXkEaZFRfh%2FJvdtNwF1c%2BAx3wajLyEM1rjlG1ScqFAauyejVRmB9%2F8hkljQK50RaxzIvO%2B9bO4oWpz0kpYuoXDDfTSZuoaTBXWFZGoxelHQ1eoeO7xztbz6jjZ0Ctd1q%2F9XNirBAynPBLmjf%2BUGzYnVLzZo9Nj1%2Foyw0VjlPPyroRnu7et6vCkYJFVm0AI3qNMLgC%2FO%2FCytq%2B%2FSDcy%2BAzR5DNrS5X3IjWiV9moVfFeK5vEZ9WxL4dJQwjpjZoA%2Bv%2BHXOjg0xFlxlfhy5AmBmPguyQyhzSR6LTV8DcAHIQ8hWDP0r25nPZPdfxm5WXPwLcX8M8Y8zUH32lkq8C9LYbrjosh%2FuGKOYLInMGF22RTqpRaLTK9zS7O%2BOip%2F5tvZIapGmSa1trnkT%2BLiSsVeZdVtvdi4w9fbkqVVjtU5RnwqLCmgw7PO4jAY6pAHzzVv5dYtoo%2FeIhEtQ38ip%2B7LpdndA1MaJ0FVrPYmdcG2Y9m%2FTDtnCJ%2F1rHgVZG6Zrz4urprAE5vzVBYlxoX%2FcBc52fycgejsxQoPw%2F40Dt%2BFXFN%2BepyeuDrEE3o92tBxHgMQkuCwueWq8JLSodApeD9TdDFgFq815vpRxwqrV8M2p5TTSM0m2sIlo4DeRQwfpwzc5reE3vc2dfeEHIbnHQEGVJA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20211112T103533Z&X-Amz-SignedHeaders=host&X-Amz-Expires=300&X-Amz-Credential=ASIAQ3PHCVTYQDUJV34F%2F20211112%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=6ed17d0662f39bd4bcb10ec12430f146d66edde7db1702c71eafbdd32976ceb2&hash=7aafd6d2fed7ac3a5cd97d5358f41c1432fad1dfbf5896b5acfb8edc77885e97&host=68042c943591013ac2b2430a89b270f6af2c76d8dfd086a07176afe7c76c2c61&pii=0167642387900359&tid=spdf-44f12906-7ab6-4339-beda-15983d1c6156&sid=288050aa8474f9443a6a178-27442b9c79cfgxrqa&type=client), these are a formalisms to model [reactive systems](https://atech.guide/what-are-reactive-systems/). They are an extension of state machines that enable new features like:

- Guarded transitions (or conditional transitions)
- Actions (functions that trigger on particular events like on entry or exit a state)
- Hierarchical states
- History, etc

So, in general words, XState is a javascript library that enables you to easily define and declare finite state machines by simple javascript object notation and visualize the state machine as a statechart, like the following image.

![Screen Shot 2021-11-12 at 16.39.15.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/97643F35-7EBC-439A-8ECA-AE3CB75ABFF4_2/Screen%20Shot%202021-11-12%20at%2016.39.15.png)

This is the state chart of a state machine that performs a fetching action.

How would you create that state machine in XState? Here's an example:

```javascript
import { createMachine, assign } from "xstate";

const machine = createMachine({
  id: "fetchingMachine",
  initial: "fetching",
  context: {
    data: [],
    error: null,
  },
  states: {
    fetching: {
      id: "fetching",
      invoke: {
        src: (context, event) => fetch("someUrl"),
        onDone: {
          target: "ready",
          actions: assign({
            data: (_, event) => event.data,
          }),
        },
        onError: {
          target: "failed",
          actions: assign({
            error: (_, event) => event.data,
          }),
        },
      },
    },
    failed: {},
    ready: {},
  },
});

const SomeComponent = () => {
  const [state] = useMachine(machine);

  if (state.match("fetching")) {
    return <h1>Loading...</h1>;
  }

  if (state.match("failed")) {
    return <h1>Some Error</h1>;
  }

  if (state.match("ready")) {
    return <h1> Data Loaded</h1>;
  }
  return null;
};
```

If you are a React developer, compare this declarative approach in XState with how it could look using the `useReducer` pattern:

```javascript
const reducer = (state, action) => {
  switch (action.type) {
    case "FETCH":
      return { ...state, data: null, loading: true, error: null };
    case "SET_DATA":
      return { ...state, data: action.payload, loading: false, error: null };
    case "FAILURE":
      return { ...state, error: action.payload, loading: false, data: null };
    default:
      return state;
  }
};

const initialState = {
  data: null,
  error: null,
  loading: true,
};

const SomeComponent = () => {
  const [state, dispatch] = React.useReducer(reducer, initialState);
  React.useEffect(() => {
    dispatch({ type: "FETCH" });
    fetch("/someUrl")
      .then((data) => dispatch({ type: "SET_DATA", payload: data }))
      .error((error) => dispatch({ type: "FAILURE", payload: error }));
  }, []);

  if (state.loading) {
    return <h1>Loading...</h1>;
  }

  if (state.error) {
    return <h1>Some Error</h1>;
  }

  if (state.data.length > 0) {
    return <h1> Data Loaded</h1>;
  }

  return null;
};
```

# So, what are you're missing out on by not using XState?

Web application development can be expressed as rendering UI and managing state. Many tools exist to simplify these processes; however, some complexities are difficult to avoid, such as ever-changing UI requirements, managing state transitions, and component communication.

XState provides a way to implement a state orchestration pattern. State Orchestration is a way to describe an [event-driven architecture ](https://docs.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven)that relies on events and side effects to transition between states.

> David Kourshid, author of XState, recently tweeted about this idea of "state orchestration":

> Been thinking about this a lot. The common principle of "separation of concerns" is often blindly applied, leading to fragile architecture. *Orchestration* is the missing part.

> [https://twitter.com/DavidKPiano/status/1243938073009889281](https://twitter.com/DavidKPiano/status/1243938073009889281)

The use of XState and the state orchestration pattern helps to avoid commonly encountered bugs when working with other "Non finite-state" solutions. These are widely called impossible states, which let's dive into now.

## What are impossible states, and how XState avoid them

States machines and statecharts are excellent communication tools about the logic you want to build since non-developer teammates generally understand them. Diagrams help a lot in the territory.

A state is a set of variables and values representing the (in this case) UI in any given point. Every new variable or value that you add causes the possible combination of states to increase. For example, you'll have 16 possible states with four independent boolean variables.

As the name implies, an impossible state is a state that should not happen in the UI. Let's see an example based on the fetch process previously shown.

Imagine a button that triggers a fetch call and three state variables to handle state changes: users, isLoading, and error.

Here you can see the handler function for this use case:

```javascript
async function fetchData() {
  try {
    setIsLoading(true);
    const newData = await fetch();
    setData(newData);
  } catch (error) {
    setError(error);
  } finally {
    setIsLoading(false);
  }
}
```

Can you spot the bug?

What happens if the user clicks the fetch button (triggering this function), the fetch fails, and the user clicks the button again? The final state will be an error and have `newData`. This shouldn't be possible!

This impossible state happens because the function does not clear the previous error. We could fix this by adding a check when the success state happens to set the error as `null`, which increases code complexity.

```javascript
async function fetchData() {
  try {
    setIsLoading(true);
    const newData = await fetch();
    setData(newData);
    // we add a check for previous errors
    if (error) {
      setError(null);
    }
  } catch (error) {
    setError(error);
    // we add a check for previous users data
    if (data) {
      setData(null);
    }
  } finally {
    setIsLoading(false);
  }
}
```

Another option is to use the reducer pattern within a React component, as in the previous example.

There is still another bug to consider. What happens if the user is too anxious and clicks the button many times? The application may move quickly between states and end up undesirable. How to fix this one? Yeah! Let's throw another boolean flag to disable the button and not trigger the fetch!.

Now the simple code looks like this:

```javascript
async function fetchData() {
  // Request in process, don't trigger the fetch
  if (isLoading) return;
  try {
    setIsLoading(true);
    const newData = await fetch();
    setData(newData);
    // Check if there were a previous error
    if (error) {
      setError(null);
    }
  } catch (error) {
    setError(error);
    // clean if the data was set before
    if (data) {
      setData(null);
    }
  } finally {
    setIsLoading(false); // Loading finished, enable the button again
  }
}
```

This code works, but the logic is becoming complex to read, and we should always follow the maxim: write code for other humans!

Now, How XState can help you in this case?

In this example, you need a way to manage the state that effortlessly defines the possible states and how these states relate to each other. Only one state should occur at a time, and it should be possible to identify the next state. In other words, we want a state machine.

Let's see how an XState state machine will look for this same case.

```javascript
import { createMachine, assign } from "xstate";

const fetchMachine = createMachine({
  id: "fetchMachine",
  initial: "idle", //Initial state: Do nothing

  context: {
    // Store the fetch result somewhere
    data: null,
    error: null,
  },
  // Declare the states of the machine
  states: {
    idle: {
      // Start in the iddle state
      on: { FETCH: "loading" }, // When the FETCH event happens, move to the `loading` state
    },
    // When enters into the loading state it will invoke a service
    // This service is just a function that return a promise
    loading: {
      invoke: {
        src: "fetchData",
        onDone: {
          // When the promise resolves
          target: "success", // Move to the success state
          actions: assign({
            // and at the same time assign the data return by the promise into the context
            users: (_, event) => event.data,
          }),
        },
        onError: {
          // If the promise is rejected
          target: "error", // Move to the error state
          actions: assign({
            error: (_, event) => event.error,
          }),
        },
      },
    },
    success: {
      // When in the success state, if the FETCH even happen, move to `loading` state again
      on: { FETCH: "loading" },
    },
    error: {
      on: { FETCH: "loading" },
    },
  },
});
```

This code can be daunting at first sight,  so let's break it down.

## How an XState machine is built

An XState machine is simple declared (yep, this is important, is declarative programming model) as a big javascript object (you can even declare it as an external JSON file), and it composes by:

- **id:** An identifier as a string is optional but helpful when you want to debug or even connect multiple machines
- **initial:** Identify the initial state of the machine. It is a simple string that declares the state before any action or transition is made.
- **context:** Is an arbitrary object that works as an "internal machine state ."It can store any data required by the application. If you're wondering the difference, the basic gist is: Anything that cannot be expressed finitely should be held as context inside the machine. Check this [article from Matt Pocock about it](https://dev.to/mpocock1/state-machines-should-this-be-a-state-or-in-context-1d7e)
- **states:** This is the meat of the machine. This is where all the states are declared, including the actions and transitions that can happen in each state. Each key of this object matches a particular state.
   - **on:** This is a state property that helps you model the actions and possible transitions that the current state can react to.

The first impression of this machine is expected that this looks like more work or that it is too cluttered, and is true, a machine definition can quickly go wild in terms of size but let's check the good side of this:

- Is simple to see all possible states of the machine
- You don't have to track any variable besides the ones you define in the context (if is required)
- You can fully decouple the state management logic (the machine) from the UI that will react to the machine.
- There are no impossible states.
- You can always get the machine diagram or draw the diagram and then declare the machine.

Copy and paste the above code [in the visualizer](https://stately.ai/viz/), and you'll see the following image

![Screen Shot 2022-01-12 at 07.57.37.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/6666304B-51E6-4401-9A30-C1B6F07ADA2B_2/8w1OjtS7xwoAzaf0Lq3p5lOIop7Du8MIe99yQxTCOAEz/Screen%20Shot%202022-01-12%20at%2007.57.37.png)

# Use the state machine with React, Vue, and Svelte

To check how to use xstate with a particular framework or library, let's review a "real world" example. Let's build a video player managed by xstate, and let's share the machine declaration among different UI implementations.

Let's first define what the requirement is: You were requested to build a video player UI, the player should:

- Load the video from an external source
- have buttons to play, pause, reset, and stop the video

Looks simple right?

Let's write the state machine!

> If you want to jump into the complete code directly, check this code sandboxes

- > [Visualizer](https://stately.ai/viz/b57eba4b-e10e-4210-9cf0-1d8ef58cf3a1)
- > [React version](https://codesandbox.io/s/react-xstate-video-player-eor2i?from-embed)
- > [Svelte version](https://codesandbox.io/s/svelte-xstate-video-player-n05hf?from-embed)
- > [Vue version](https://codesandbox.io/s/vue-xstate-video-player-m8omg?from-embed)

Let's think in what states the player will possible be:

- Loading or waiting
- Ready to play
- Playing
- Paused
- Stoped

The latest three states are only possible if the player is ready to play.

Let's set that into a machine declaration.

```javascript
const machine = createMachine({
  id: "playerMachine",
  initial: "waiting",
  states: {
    waiting: {},
    ready: {
      states: {
        playing: {},
        paused: {},
        stopped: {},
      },
    },
  },
});
```

> One crucial thing to notice here is that a machine can have nested machines inside of this. This is called [Hierarchical states](https://xstate.js.org/docs/guides/hierarchical.html). This helps a lot to declare states tied to another state like in this case. The `playing` state can only be if the `ready` state is on.

Now is time to add the logic of how the machine will move from one state to another: Transitions.

```javascript
const machine = createMachine({
  id: "playerMachine",
  initial: "waiting",
  states: {
    waiting: {
      on: {
        SUCCESS: "ready",
        FAILED: "failed",
      },
    },
    failed: {},
    ready: {
      id: "readyMachine",
      initial: "ready",
      states: {
        ready: {
          on: {
            PLAY: {},
          },
        },
        playing: {
          on: {
            PAUSE: {},
            RESET: {},
            STOP: {},
          },
        },
        paused: {
          on: {
            PLAY: {},
            RESET: {},
            STOP: {},
          },
        },
        stopped: {
          on: {
            RESET: {},
            PLAY: {},
          },
        },
      },
    },
  },
});
```

The machine looks way more extensive, but we have all the possible states and each "event" that a state can take.

The next step is to define the transitions, meaning, to what state the machine should go when a particular event happens.

```javascript
import { createMachine } from "xstate";

const machine = createMachine({
  id: "playerMachine",
  initial: "waiting",
  states: {
    waiting: {
      on: {
        SUCCESS: "ready",
        FAILED: "failed",
      },
    },
    failed: {},
    ready: {
      id: "readyMachine",
      initial: "ready",
      states: {
        ready: {
          on: {
            PLAY: {
              target: "playing",
            },
          },
        },
        playing: {
          on: {
            PAUSE: {
              target: "paused",
            },
            RESET: {
              target: "playing",
            },
            END: {
              target: "stopped",
            },
          },
        },
        paused: {
          on: {
            PLAY: {
              target: "playing",
            },
            RESET: {
              target: "playing",
            },
            END: {
              target: "stopped",
            },
          },
        },
        stopped: {
          on: {
            RESET: {
              target: "playing",
            },
            PLAY: {
              target: "playing",
            },
          },
        },
      },
    },
  },
});
```

And that can be represented by the following chart.

![Screen Shot 2022-01-12 at 08.23.58.png](https://res.craft.do/user/full/0a4fa0f6-6e08-58de-6dbc-6c56902ee35d/18427827-6084-419C-A3F9-45232FB6362C_2/xfBxmBE6yn9MoyTOdXk9fSF3bkn4n91QIXyDYTxyKP8z/Screen%20Shot%202022-01-12%20at%2008.23.58.png)

Here you can see the transition from one state to another.

So final step is to declare some actions that need to happen when a transition is fired. These actions will be defined externally based on each implementation of the UI, so you reference them (with a named string) inside each transition.

```javascript
import { createMachine } from "xstate";

const machine = createMachine({
  id: "playerMachine",
  initial: "waiting",
  states: {
    waiting: {
      on: {
        SUCCESS: "ready",
        FAILED: "failed",
      },
    },
    failed: {},
    ready: {
      id: "readyMachine",
      initial: "ready",
      states: {
        ready: {
          on: {
            PLAY: {
              target: "playing",
              actions: ["playVideo"], //fire actions when the transition happen
            },
          },
        },
        playing: {
          on: {
            PAUSE: {
              target: "paused",
              actions: ["pauseVideo"],
            },
            RESET: {
              target: "playing",
              actions: ["resetVideo"],
            },
            END: {
              target: "stopped",
              actions: ["stopVideo"],
            },
          },
        },
        paused: {
          on: {
            PLAY: {
              target: "playing",
              actions: ["playVideo"],
            },
            RESET: {
              target: "playing",
              actions: ["resetVideo"],
            },
            END: {
              target: "stopped",
              actions: ["stopVideo"],
            },
          },
        },
        stopped: {
          on: {
            RESET: {
              target: "playing",
              actions: ["resetVideo"],
            },
            PLAY: {
              target: "playing",
              actions: ["resetVideo"],
            },
          },
        },
      },
    },
  },
});
```

## Time to React

Let's bring React into the mix as an example of using the machine.

XState offers a package for each significant framework. In this case, you need to use `@xstate/react`

This will provide some hooks that will work with the UI.

In this case, you'll use the `useMachine` hook. This hook accepts a machine as an argument and also some configuration object that, in this case, you'll use to define the actions.

The UI will be a simple `<video>` tag and some buttons, so to know the state of the content, you'll need to use a `ref`. This ref is the one that you'll use inside the declared actions.

The `useMachine` hook will return a tuple (same as the `useState` hook) with the `state` of the machine and a function that will trigger transitions (`send`)

```javascript
const [state, send] = useMachine(machine, {
  //define the actions that will be triggered when buttons are click
  actions: {
    playVideo: () => videoElement?.current.play(),
    pauseVideo: () => videoElement?.current.pause(),
    resetVideo: () => {
      videoElement.current.pause();
      videoElement.current.currentTime = 0;
      videoElement.current.play();
    },
    stopVideo: () => {
      videoElement.current.pause();
      videoElement.current.currentTime = videoElement.current.duration;
    },
  },
});
```

Next, we need a way to know the machine's state so the UI can react to it, like disabling a button. For that, you'll use the `matches` method from the state. This method determines if the current state is a subset of the state passed as an argument.

> There is another hook that allows you to use selectors: `useSelector`. This hook has better performance since it will only cause a re-render if the selected value changes. Check the[ documentation here](https://xstate.js.org/docs/packages/xstate-react/#api)

So this lets you build the following "flags."

```javascript
const isPlaying = () => state.matches({ ready: "playing" });
const isPaused = () => state.matches({ ready: "paused" });
const isReady = () => state.matches("ready");
const isStoped = () => state.matches({ ready: "ended" });
```

Now is the time to add the JSX for the UI. The UI will use the above flags and the `send` method returned by the `useMachine` hook to trigger transitions.

```javascript
const VideoPlayer = () => {
  const videoElement = React.useRef(null);
  const [state, send] = useMachine(machine, {
    //define the actions that will be triggered when buttons are click
    actions: {
      playVideo: () => videoElement?.current.play(),
      pauseVideo: () => videoElement?.current.pause(),
      resetVideo: () => {
        videoElement.current.pause();
        videoElement.current.currentTime = 0;
        videoElement.current.play();
      },
      stopVideo: () => {
        videoElement.current.pause();
        videoElement.current.currentTime = videoElement.current.duration;
      },
    },
  });

  const isPlaying = () => state.matches({ ready: "playing" });
  const isPaused = () => state.matches({ ready: "paused" });
  const isReady = () => state.matches("ready");
  const isStoped = () => state.matches({ ready: "ended" });

  return (
    <>
      <video
        src="https://sample-videos.com/video123/mp4/720/big_buck_bunny_720p_30mb.mp4"
        width={360}
        onCanPlay={() => send("SUCCESS")}
        onError={() => send("FAILED")}
        ref={videoElement}
      >
        <track kind="captions" />
      </video>
      <br />

      <button
        onClick={() => {
          send("PLAY");
        }}
        disabled={isPlaying()}
      >
        Play
      </button>

      <button onClick={() => send("PAUSE")} disabled={isPaused() || isStoped()}>
        Pause
      </button>

      <button onClick={() => send("RESET")} disabled={!isReady()}>
        Reset
      </button>

      <button onClick={() => send("END")} disabled={!isReady()}>
        Stop
      </button>
    </>
  );
};
```

And there you have, a full implementation of a video player based on a machine fully decoupled of the UI itself.

## Conclusion

The main job of a web developer is all about managing the state of a dynamic web application. This ever-changing state creates complexity since it can lead to different combinations of states or even impossible states.

Many tools or libraries aim to help you with this task in very different ways. Still, just one uses solid computer science for its design and usage. XState handles user events and the resulting UI changes in a simple way.

In this article, you were able to check about:

- What is XState.
- What is a Finite State Machine and State Charts.
- What are impossible states?
- How XState helps you manage the state changes.

