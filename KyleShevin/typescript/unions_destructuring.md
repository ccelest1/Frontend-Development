# Discriminated Unions + Destructure in TS
- DU  = type where key, value is used to determine rest of shape of type
    ```ts
    type GetPostsSuccess = {
        kind: 'success'
        context: {
            data: { posts: [] }
        }
    }

    type GetPostsFailure = {
        kind: 'failure'
        context: {
            error: { message: 'something went wrong' }
        }
    }

    // now combine w/ union
    type GetPostResults = GetPostsSuccess | GetPostsFailure

    // discriminate union by `kind` key
    function getPosts(): GetPostResults {
    // Let's assume something happens and I get a result
    }

    // works properly -> if result.king === 'success', obj = context.data.posts
    function doSomethingWithPosts() {
    const result = getPosts()

    switch (result.kind) {
            case 'success':
            return result.context.data.posts

            case 'failure':
            return result.context.error.message
        }
    }
    ```

    ```ts
    // set up state to follow progress of API call -> common pattern in web apps that benefits from use of enumerated states

    enum States {
        IDLE = 'IDLE',
        LOADING = 'LOADING',
        SUCCESS = 'SUCCESS',
        FAILURE = 'FAILURE',
    }

    type Post = {
        title: string
        body: string
    }

    type Data = { posts: Array<Post> }

    type Results =
        | [States.IDLE, undefined]
        | [States.LOADING, undefined]
        | [States.SUCCESS, Data]
        | [States.FAILURE, Error]

    // results has DU based on states enum -> create custom hook using types, fetch posts
    function useGetPosts(): Results {
        const [result, setResult] = useState<Results>([States.IDLE, undefined])

        useEffect(() => {
            setResult([States.LOADING, undefined])

            fetch('/api/posts')
            .then(res => res.json())
            .then(data => {
                setResult([States.SUCCESS, data])
            })
            .catch(error => {
                setResult([States.FAILURE, error])
            })
        }, [])

        return result
    }

    // Gained refinement -> suggestion: use object v tuple, allows for result.state + result.context -> data in SUCCESS, error in FAILURE
    function Posts() {
        const result = useGetPosts()

        switch (result[0] /* state */) {
            case States.IDLE:
            return <div>idle</div>

            case States.LOADING:
            return <div>loading...</div>

            case States.SUCCESS: {
            // We can destructure here because our `context` is refined to the
            // correct shape by the switch statement. We can also skip
            // destructuring `state` and rename `context` to `data` in one step
            const [, data] = result

            return (
                <div>
                {data.posts.map(post => (
                    <div key={post.title}>
                    {post.title} {post.body}
                    </div>
                ))}
                </div>
            )
            }

            case States.FAILURE: {
            const [, error] = result
            return <div>{error.message}</div>
            }
        }
    }
    ```
