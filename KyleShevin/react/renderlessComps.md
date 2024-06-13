# Renderless Components
- Not all comps render DOM elements, anyting at all
- Legitimate comp use: `render() {return null}` -> helpful when req logic to exist that updates store with no associated UI
    * Making container comp that doesn't return any presentational comps

## Situation
- Required to update value in store `currentTime` with current hr, min, s -> (redux req reducers to be pure) logic for updating time must be action itself, passed to action create fxn as params
    - Req comp to set up interval to dispatch fxn that updates store
    * Comp uses lifecycle methods, no render markup -> aided in business logic composition
    ```js
    import { Component, PropTypes } from 'react'
    import { connect } from 'react-redux'

    // This is my action creator function
    // It takes three parameters: hours, minutes, seconds
    import { updateTime } from '../actions'

    class UpdateTimeContainer extends Component {
    constructor() {
        super()

        this.intervalId = null
        this.startClock = this.startClock.bind(this)
        this.stopClock = this.stopClock.bind(this)
        this.getCurrentTime = this.getCurrentTime.bind(this)
    }

    componentDidMount() {
        this.startClock()
    }

    componentWillUnmount() {
        this.stopClock()
    }

    startClock() {
        this.intervalId = setInterval(this.getCurrentTime, 5000)
    }

    stopClock() {
        clearInterval(this.intervalId)
    }

    getCurrentTime() {
        const now = new Date()
        const hours = now.getHours()
        const minutes = now.getMinutes()
        const seconds = now.getSeconds()

        this.props.updateTime(hours, minutes, seconds)
    }

    render() {
        return null
    }
    }

    UpdateTimeContainer.propTypes = {
    updateTime: PropTypes.func,
    }

    const mapDispatchToProps = {
    updateTime,
    }

    export default connect(null, mapDispatchToProps)(UpdateTimeContainer)
    ```
