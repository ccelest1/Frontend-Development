# TS preventing bad, good
- Good of TS: Prevents several bad concepts/issues: bugs, errors, bad assumptions, etc.
- Bad of TS: easier to express functionality v types , easy to build in realm of JS vs the difficulty of type-declaration in TS

## State Machines v Type Safety
- Implement SM to manage complicated UI
    * Required to make it compile-time safe via types

# Wrangle Tuple Types
```ts
function useBool(initialValue = false) {
  const [state, setState] = React.useState(initialValue)

  const handlers = React.useMemo(
    () => ({
      on: () => setState(true),
      off: () => setState(false),
      toggle: () => setState(s => !s),
      reset: () => setState(initialValue),
    }),
    [initialValue],
  )

/*
    adding `as const` after returned tuple tells TS that return value of useBool is a readonly array
        - TS correctly types state, handlers where used
*/
  return [state, handlers] as const
}

// returning tsx
function Secret({ message }: { message: string }) {
  const [isOpen, setIsOpen] = useBool()

  return (
    <div>
      <button type="button" onClick={setIsOpen.toggle}>
        {isOpen ? 'Hide' : 'Reveal'}
      </button>

      {isOpen && <div>{message}</div>}
    </div>
  )
}

```
