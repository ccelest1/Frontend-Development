# Facade Pattern
- FP: add layer of abstraction between consumer of code and implementation
    - create a wrapping function/class using package -> 2 reasons:
        1. Greater API control
            * Facade pattern used to simplify more complex class, object, function's implementation from facade consumer
        2. Provide flexibility to swap package, implementation details w/o affecting code using facade
            * Hide details and change when req w/o affecting exposed api of facade, consumed by rest of app
- __WHEN TO USE FACADE PATTERN__
    * Using libs for data fetching, data formatting, etc. can be good candidates for facade pattern
        ```js
        // API.js - facade
        import axios from 'axios'
        export default class APIA {
            get(url){
                return axios.get(url)
            }
            post(url, options){
                return axios.post(url, options)
            }
            // other methods, etc.
        }

        // can also replace axios lib with traditional fetch in API.js
        export default class API {
            get(url){
                return fetch(url)
            }
             post(url, options) {
                return fetch(url, {
                    method: 'POST',
                    ...options,
                })
            }
        }


        // DetailPage.js
        import React, { useState, useEffect } from 'react';
        import API from './API'

        function DetailPage({ id }){
            const [data, setData] = useState( null )
            useEffect(()=>{
                API.get(`your/api/url/pages/${id}`)
                    .then(res => res.json())
                    .then(json => {
                        setData(json)
                    })
            }, [id])
            return data !== null && <div>{JSON.stringify(data, null, 2)}</div>
        }
        ```

- PROS, CONS
    * Pros: easy to make switches as necessary
    * Cons: Adding a layer of abstraction that reqs documentation
        - need to update facade if there are fts of underlying library to be used
