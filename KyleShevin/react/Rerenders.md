# Using useMemo() to avoid extra Rerenders
- common pattern for writing custom React Hooks: return tuple [state, handlers]
    * state: value held by hook, handlers: obj w/ methods that update state
    ```js
    // useState(), useMemo()

    //useMemo - simplest method for stable value creation, handlers - obj of methods (best API to provide hook users)

    function useSwitch(initialState = false) {
    const [state, setState] = React.useState(initialState)

    // PAY ATTENTION HERE
    const handlers = React.useMemo(
        () => ({
        on: () => {
            setState(true)
        },
        off: () => {
            setState(false)
        },
        toggle: () => {
            setState(s => !s)
        },
        reset: () => {
            setState(initialState)
        },
        }),
        [initialState]
    )

    return [state, handlers]
    }
    ```
- create stable values
    * useMemo - returns memoized value -> as long as dependencies don't change, useMemo() returns cached value when previously computed
    * allows for elimination of value changes that cause unnecessary downstream renders
- nice APIs
    * `[state, handlers]` -> simple, flexible as shown below
```js
function Door() {
  const [isOpen, { toggle }] = useSwitch(false)

  return (
    <div>
      <p>The door is {isOpen ? 'open' : 'closed'}.</p>
      <div>
        <Button onClick={toggle}>Toggle door</Button>
      </div>
    </div>
  )
}
```

# Debugging Hooks
- [Repo](https://github.com/kyleshevlin/use-debugger-hooks)
- [article](https://kyleshevlin.com/helpful-debugging-hooks/)
