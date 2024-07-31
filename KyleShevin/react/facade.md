# Facade Pattern
- FP: add layer of abstraction between consumer of code and implementation
    - create a wrapping function/class using package -> 2 reasons:
        1. Greater API control
            * Facade pattern used to simplify more complex class, object, function's implementation from facade consumer
        2. Provide flexibility to swap package, implementation details w/o affecting code using facade
            * Hide details and change when req w/o affecting exposed api of facade, consumed by rest of app
