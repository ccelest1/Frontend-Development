# Multiple Compositions to the Rescue in Responsive Design
- [Article on Multiple Composition](https://kyleshevlin.com/prefer-multiple-compositions/)
- When several decisions depend on single value, prefer multiple compositions even when more verbose v conditional handling inline

- Example: If composition is preferred, VideoControls [ button that controls video state ]
    * ```js
        function VideoControls() {
            const { isPlaying, play, pause } = useVideoControls()

            isPlaying? {
                return (
                    <Button onPress={pause}>
                        <Icon name="pause" />
                        <Button.Text>Pause</Button.Text>
                    </Button>
                )
            }:{
                return (
                    <Button onPress={play}>
                    <Icon name="play" />
                    <Button.Text>Play</Button.Text>
                    </Button>
                )
            }
        }
        /*

        Can also do this

        function VideoControls() {

        const { isPlaying } = useVideoControls()
            return isPlaying ? <PauseButton /> : <PlayButton />
        }

        function PauseButton() {
            const { pause } = useVideoControls()

            return (
                <Button onPress={pause}>
                <Icon name="pause" />
                <Button.Text>Pause</Button.Text>
                </Button>
            )
        }

        function PlayButton() {
            const { play } = useVideoControls()

            return (
                <Button onPress={play}>
                <Icon name="play" />
                <Button.Text>Play</Button.Text>
                </Button>
            )
        }

        */
      ```

- Example: If we are fetching data on client, map desired ui composition to each possible state
    ```js
        function DataView() {
            const { data, error, isLoading } = useGetMyData()

            if (isLoading) {
                return <LoadingComposition />
            }

            if (error) {
                return <ErrorComposition error={error} />
            }

            if (isEmpty(data)) {
                return <EmptyComposition />
            }

            return <SuccessComposition data={data} />
        }
    ```

```ts
// example of discriminated union
type MotorBike = {
  type: 'motorbike';
  fuel: 'petrol';
}

type Car = {
  type: 'car';
  doors: number;
  bootSize: number;
}

type Vehicle = {
  make: string;
  model: string;
  fuel: 'petrol' | 'diesel',
} & (Car | MotorBike);
```
- Compositions works well with objects that share a discriminated union -> Stream can be in several phase: 'created'|'live'|'ended' (screens differ per live phase)
```js
function StreamDisplay({ stream }: { stream: Stream }) {
  switch (stream.phase) {
    case 'created':
      return <StreamCreated stream={stream} />

    case 'ended':
      return <StreamEnded stream={stream} />

    case 'live':
      return <StreamLive stream={stream} />

    default:
      return null
  }
}
```
