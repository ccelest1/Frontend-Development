# Multiple Compositions to the Rescue in Responsive Design
- [Article on Multiple Composition](https://kyleshevlin.com/prefer-multiple-compositions/)
    - [notes](multiple_comps.md)
- When we have the same condition several times lower in component and element tree, lift it higher in order to reduce number of times it is to be checked
    - Ex: Turn `Presenter` into a [facade](https://kyleshevlin.com/facade-pattern/) rendering either `PresenterNarrow` or `PresenterWide` composition
    ```js
    /*
        guarantee which context each sub-comp renders in
    */
    function Presenter(props) {
        return (
            <div>
            <div className="sm:hidden">
                <PresenterNarrow {...props} />
            </div>

            <div className="hidden sm:block">
                <PresenterWide {...props} />
            </div>
            </div>
        )
    }
    ```
    - Shared Pieces
    ```js
        function Name({ children }) {
            return <h2 className="font-sans text-2xl font-bold">{children}</h2>
        }

        function Description({ children }) {
            return <div className="font-sans text-lg">{children}</div>
        }
    ```

    - Presenter Narrow Comp
    ```js
        function PresenterNarrow({ description, image, name, role }) {
            return (
                <div className="flex flex-col gap-4">
                <Name>{name}</Name>
                <Description>{description}</Description>

                <div className="flex flex-col gap-2">
                    <img className="h-12 w-12 rounded-xl" src={image} alt={name} />

                    <div>
                    <div className="font-sans text-sm font-bold">{name}</div>
                    <div className="font-sans text-sm">{role}</div>
                    </div>
                </div>
                </div>
            )
        }
    ```

    - PresenterWide
    ```js
        function PresenterWide({ description, image, name, role }) {
            return (
                <div className="flex items-center gap-12">
                <div className="flex w-[40%] shrink-0 flex-col gap-4">
                    <Name>{name}</Name>
                    <img className="w-full rounded-3xl" src={image} alt={name} />
                </div>

                <div className="flex grow flex-col gap-4">
                    <Description>{description}</Description>
                    <div className="font-sans font-bold">
                    {name}, {role}
                    </div>
                </div>
                </div>
            )
        }
    ```
