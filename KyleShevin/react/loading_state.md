# Loading State Trick for Stateless Functional Components in React
- Concept of container, presentational components
    * container comps provide data to nested comps that will use lifecycle methods like compDidMount to fetch data
    * presentational comps display data with no use of lifecycle methods (stateless functional comp)

- Stateless functional comp returns markup
    * props passed as fxn args and utilized within markup
    ```js
    import React, { PropTypes } from 'react'
    import LoadingSpinner from './LoadingSpinner'

    // expects props object as arg
    // either render loading spinner comp or list depending on passed in items
    const DisplayItems = ({ items }) => (
        return items.length ? ( <div className="items">
            {items.map((item, index) => (
            // pass all props from item into div
            <div className="item" key={index} {...item} />
            ))}
        </div>
        ):(
            <LoadingSpinner/>
        )
    )

    // ES6 -> using items property on props object
    Display.propTypes =
    items: PropTypes.array,
    }

    export default DisplayItems
    ```
    ```js
    // also using a spinner comp showing animation

    import React, { PropTypes } from 'react'

    const LoadingSpinner = () => (
    <div className="loading_spinner-wrap">
        <svg
        className="loading_spinner"
        width="60"
        height="20"
        viewBox="0 0 60 20"
        xmlns="http://www.w3.org/2000/svg"
        >
        <circle cx="7" cy="15" r="4" />
        <circle cx="30" cy="15" r="4" />
        <circle cx="53" cy="15" r="4" />
        </svg>
    </div>
    )

    export default LoadingSpinner
    ```
    ```css
    /*
    Basic styles for spinner comp
    */
    .loading_spinner-wrap {
        width: 100%;
        padding: 50px 0;
    }

    .loading_spinner {
        display: block;
        margin: 0 auto;
        fill: #000;

        circle {
            animation-name: upAndDown;
            animation-duration: 2s;
            animation-timing-function: cubic-bezier(0.05, 0.2, 0.35, 1);
            animation-iteration-count: infinite;

            &:nth-child(2) {
            animation-delay: 0.18s;
            }

            &:nth-child(3) {
            animation-delay: 0.36s;
            }
        }
        }

        @keyframes upAndDown {
        0% {
            opacity: 0;
            transform: translateY(0);
        }
        25% {
            opacity: 1;
            transform: translateY(-10px);
        }
        75% {
            opacity: 1;
            transform: translateY(-10px);
        }
        100% {
            opacity: 0;
            transform: translateY(0);
        }
    }
    ```
