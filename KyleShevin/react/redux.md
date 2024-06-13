# Redux State Snapshots
- Redux = predictable state container for js apps
## Problem
- UI for one platform depended on state to determine correct layout + color scheme
    - There are several elements (logo, heading, footer) that have colors based on state booleans
    - When certain actions are taken such as opening nav, change state of those items
    - If user closes nav w/o link selection, elements return back to initial state

- Most obvious solution is to trigger actions changing color of each individual piece of UI one had when opening nav
    * Confident about UI state when opening nav -> (problematic) unsure about UI state when nav was closed

## Solution
- Create snapshot of any part of state object -> store in same state object -> use values to replace current state at a given time ( first level inception of state )
    * creation and application of state snapshot was significant ft of app -> return to previous state easily

    - Redux app req: (1) initial state, (2) actions to dispatch + change state, (3) reducers handle actions + return new state object

    ### Code
    #### Initial State
    - Defined in reducer file it belongs to
    ```js
        const initialState = {
            headingIsLight: false,
            logoIsLight: false,
            footerIsLight: true,
            navIsOpen: false,
            // use this key to store state pieces -> set to null for initial instantiation
            uiSnapshot: null,
        }
    ```
    #### Actions
    ```js
        // actions to dispatch to reducers -> number of strats for handling action creators, action types
        // for ex, using actionTypes.js for all types -> create action creators under actions dir

        // actionTypes.js
        export const OPEN_NAV = 'OPEN_NAV'
        export const CLOSE_NAV = 'CLOSE_NAV'
        export const TAKE_UI_SNAPSHOT = 'TAKE_UI_SNAPSHOT'
        export const APPLY_UI_SNAPSHOT = 'APPLY_UI_SNAPSHOT'

        // actions/ui.js
        import {
        OPEN_NAV,
        CLOSE_NAV,
        TAKE_UI_SNAPSHOT,
        APPLY_UI_SNAPSHOT,
        } from './actionTypes'

        export function openNav() {
        return { type: OPEN_NAV }
        }

        export function closeNav() {
        return { type: CLOSE_NAV }
        }

        export function takeUISnapshot() {
        return { type: TAKE_UI_SNAPSHOT }
        }

        export function applyUISnapshot() {
        return { type: APPLY_UI_SNAPSHOT }
        }
    ```
    #### Reducers
    - Require reducers to create store to dispatch actions + Create reducers to create store to dispatch actions, handle action types
    ```js
    import {
        OPEN_NAV,
        CLOSE_NAV,
        TAKE_UI_SNAPSHOT,
        APPLY_UI_SNAPSHOT
    } from './actionTypes'

    // Our initial state from before
    const initialState = {
        headingIsLight: false,
        logoIsLight: false,
        footerIsLight: true,
        navIsOpen: false,
        uiSnapshot: null
    }

    const reducer = (state = initialState, action) {
        // handle each action.type w/ different case -> each case is designed to return new obj with desired state
        switch (action.type) {
            case OPEN_NAV:
                // Object.assign() -> copies all enumerable props from 1 or + source objs to target obj -> return mod target obj so a {} with all props in it (deep merge into target)
                // handle every case w `Object.assign({}, state) -> pass other object w/ state to change
                return Object.assign({}, state, {
                    navIsOpen: true,
                    headingIsLight: true,
                    logoIsLight: true,
                    footerIsLight: false
                })

            case CLOSE_NAV:
                return Object.assign({}, state, { navIsOpen: false })

            case TAKE_UI_SNAPSHOT:
                return Object.assign({}, state, { uiSnapshot: state })

            case APPLY_UI_SNAPSHOT:
                return Object.assign({}, state, state.uiSnapshot)

            default:
                return state
        }
    }

    export default reducer
    ```

    #### Implementation Example
    ```js
    // understanding how Object.assign() allows for understanding of op of Snapshots
    // OPEN_NAV action is preceded by TAKE_UI_SNAPSHOT
    import store from './store'
    import { takeUISnapshot, openNav } from './actions/ui'

    store.dispatch(takeUISnapshot())
    store.dispatch(openNav())
    ```
    ```js
    // now we have a snapshot with uiSnapshot prop having entire previous state w/ initial state of null
    {
        headingIsLight: false,
        logoIsLight: false,
        footerIsLight: true,
        navIsOpen: false,
        uiSnapshot: {
            headingIsLight: false,
            logoIsLight: false,
            footerIsLight: true,
            navIsOpen: false,
            uiSnapshot: null
        }
    }
    // when we apply snapshot, merge value under uiSnapshot at tree root level -> reset uiSnapshot to null and if user closes nav w/o choosing link, close nav and revert to previous state with applied snapshot
    ```
    ```js
    import store from './store'
    import { closeNav, applyUISnapshot } from './actions/ui'

    store.dispatch(applyUISnapshot())
    // navOpen: false is store in snapshot -> nav closes when state is applied
    // likely would dispatch closeNav
    store.dispatch(closeNav())
    ```
- Potential use case -> error occurs in app, log out snapshot for entire state tree and use snapshot for debugging purposes (good idea! -> redux-testing) or toggle between states while dev'ing comps

## Normalizing Redux State
- Allows for lookup of item's id property via object key + maintain array of ids in correct order -> in same reducer logic `map()` out keys in array and define as property as state object
```js
const state = {
  stories: {
    // an object that looks like the one logged out from above
  },
  allStoryIds: [
    // an array of each id to use as keys for lookup
  ],
}
```
* sames as rec Redux Normalization [normalize state](http://redux.js.org/docs/recipes/reducers/NormalizingStateShape.html)
