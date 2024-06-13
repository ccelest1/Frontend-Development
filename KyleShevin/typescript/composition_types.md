# 2 Composition Types
- Composition = act of combining, recombining existing 'elements' to create new ones
- 2 various types of composition: function composition, obj composition -> nesting composition, merging composition

### TLDR
- Composition = combining existing elements to create new ones
- Nesting = use wrapping elements to mod functionality
    * parents change child element functionality
- Merging = combine elements -> achieve new ones
    * Order is critical

## Function (nesting) composition
* When one nests functions together, result = 1 becomes input of other -> make new functions by combining in various orders for new result
```js
function MyComponent({ children }) {
  return (
    <Stack gap={4}>
      <Text>{children}</Text>
      <Row>
        <Button>Click me</Button>
      </Row>
    </Stack>
  )
}
```
## Object (merging) composition
```js
const primaryButtonStyles = {
  backgroundColor: 'blue',
  color: 'white',
  padding: '1em',
}

const dangerButtonStyles = {
  ...primaryButtonStyles,
  backgroundColor: 'red',
}

console.log(dangerButtonStyles)
// { backgroundColor: 'red', color: 'white', padding: '1em'}
```
```js
import baseConfig from 'something'

export const myConfig = {
  ...baseConfig,
  someProperty: {
    foo: 'bar',
  },
}
```
# Significance
- Composition allows for multiple ways to achieve desire result at tradeoff of resulting destructive product in process
- Function/nesting comps is superior in regards to strictness of inputs, outputs -> more difficult for devs to intuit (less room for attempting new design)
