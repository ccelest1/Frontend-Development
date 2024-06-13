# React Project Structure
## Do have a folder named features where each sub-folder is a specific feature containing all the files related to that feature.
- Take all comps related to single ft to give ft a name, contain all files for ft in folder
    * Conscious thinking about interfaces, define useful boundaries
    * Pick a feature, explore sub-tree of system

# Break out Comp Logic w/ Hooks
- One pattern: break out of component logic into custom hook -> destructuring parts of hook's API my component needs
## Examples
```js
// created comp with is selected? (boolean alt between true and false)
// toggle comp that changes word depending on current state
function Item() {
  const [isSelected, setSelected] = React.useState(false)

  const toggleSelection = () => {
    setSelected(x => !x)
  }

  return (
    <div>
      <div>Is selected? {String(isSelected)}</div>
      <button type="button" onClick={toggleSelection}>
        Toggle
      </button>
    </div>
  )
}
```
```js
// generalize abstraction for hook that (1) uses parts of interface critical to component, (2) rename APIs to provide increased context to comp's use case

// We parameterize the initialState and provide a default value of false
function useBooleanAPI(initialState = false) {
  // Using the Boolean constructor ensures that the hook cannot be
  // given an initial state of the wrong type
  const [state, setState] = React.useState(Boolean(initialState))

  const setTrue = () => {
    setState(true)
  }

  const setFalse = () => {
    setState(false)
  }

  const toggle = () => {
    setState(x => !x)
  }

  return {
    setFalse,
    setTrue,
    state,
    toggle,
  }
}

/*
both comps (1) consume same interface, (2) have freedom to use interface parts required + rename functionality parts to reflect context they are used in
*/
function Item() {
  const { state: isSelected, toggle: toggleSelection } = useBooleanAPI()

  return (
    <div>
      <div>Is selected? {String(isSelected)}</div>
      <button type="button" onClick={toggleSelection}>
        Toggle
      </button>
    </div>
  )
}

function SimilarItem() {
  const {
    state: isSelected,
    setFalse: unselect,
    setTrue: select,
  } = useBooleanAPI()

  return (
    <div>
      <div>Is selected? {String(isSelected)}</div>
      <div>
        <button type="button" onClick={select}>
          On
        </button>
        <button type="button" onClick={unselect}>
          Off
        </button>
      </div>
    </div>
  )
}
```
