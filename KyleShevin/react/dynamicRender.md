# Dynamic Render React Components
- System of reusable admin components -> 'blocks' architected by utilizing block in various orders, patterns
    * In order to render comps when we dont have (1) quantity, (2) which blocks we will be given

- Handle a dynamic array of objects using a switch statement ->
    0. each obj has type property and need to make indv comps for each
    1. check each type and require proper comp w/ each one

```js
export default class BlocksLoop extends Component {
  constructor() {
    super()
    this.getBlockComponent = this.getBlockComponent.bind(this)
  }

  getBlockComponent(block) {
    switch (block.type) {
      case 'heading':
        return <HeadingBlock key={block.id} {...block} />

      case 'text':
        return <TextBlock key={block.id} {...block} />

      case 'image':
        return <ImageBlock key={block.id} {...block} />

      case 'list':
        return <ListBlock key={block.id} {...block} />

      default:
        return <div className="no_block_type" />
    }
  }

  render() {
    return (
      <div className="blocks_loop">
        {this.props.blocks.map(block => this.getBlockComponent(block))}
      </div>
    )
  }
}
```
* Blocks array is mapped over -> each item is passed into method -> correct comp is returned dynamically
    - Type was added in back end prior to make a new comp -> method returns empty `div` w/ class to be styled
    - can reduce boilerplate with var + `let dyanmicComponent = null` -> override assignment in each case w/ correct comp -> add `key` + `{...block}` props to current `dynamicComponent` var
