# What are State Machines
- SMs provide reliable interface for handling systems, capable of handling problems (simple -> complex)
    * finite SM = api that enumerates all possible sys states, for each state, set of events is enumerated defining possible state transitions
    * Can only ever be in single state at a given time and is only transitioned to new state by an event
        - By defining finite possibilities of machine states, events -> create graph ds describing system -> each node represents state, each edge = possible state event

```js
const lightSwitch = {
  initial: 'off',
  states: {
    on: {
      events: {
        SWITCH: 'off',
      },
    },
    off: {
      events: {
        SWITCH: 'on',
      },
    },
  },
}
```
- given an initial state as well as possible states that will be alternated to following a particular event, we can now create a rudimentary state machine
    *
    ```js
    const interpreter = machine => {
    // We store the current state in closure
    // keeping the value in memory
    let currentState = machine.initial

    return {
        currentState() {
        return currentState
        },
        transition(event) {
        // Since we enumerated all possible events
        // for each state, our next state is either
        // the defined state for that event,
        // or it is undefined, and thus we can
        // return the currentState
        const nextState =
            machine.states[currentState].events[event] || currentState
        currentState = nextState
        return nextState
        },
    }
    }
    ```
    ```js
    // can easily add methods, properties to interface to enhance functionality
    // add method to interpreter to return all possible events for given state
    allEvents(){
        const { events = {} } = machine.states[currentState]
        return Object.keys(events)
    }
    ```
