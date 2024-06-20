- CSS Selectors: [https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors]
* selectors used to target html elements -> allow for fine grained precision
    * element/elements selected by selector = subject of selector
    ```css
    h1{
        color:blue
    }
    ```

    * can also have selector list [ tag of h1, class of special i.e <h1 class='special'>]
    ```css
    h1, .special {
        color: green;
    }
    /* type, class, id */
    h1 - type selector
    .box - class
    #unique - id
    ```

    * attribute selectors - group of selectors allow for different ways to select elements based on occurrence of a specific attribute on element
        - ```css
            /* a tag with a title attribute on it */
            a[title]
            /* a tag with href value = * */
            a[href='*']
          ```

    * pseudo-classes, pseudo-elements
        - style certain element states
        ```css
        /* selects element when being hovered on */
            a:hover{

            }
        /* select certain element part -> ::first-line selects first line of text in element */
        p::first-line{
            color: yellow;
        }
        ```
    * combinators
        - combing other selectors in order to target elements in documents
            * ex: select paragraphs that are descendants of <article> elements using child combinator
                ```css
                    article > p {

                    }
                ```
    - type, class, id selectors
        - using universal selector for reading
            *
            - want to select any descendant elements of <article> element that are first child of parent + direct children and make bold, use :first-child psuedo-class
            ```css
                article *:first-child{
                    font-weight: bold
                }
            ```
            - select any <article> element that is 1st child of other element
            ```css
            article:first-child{
                font-weight: bold
            }
            ```
            - target element for >1 class applied
            ```css
            .notebox.warning{
                border-color:orange;
                font-weight:bold
            }
            /* targets
            <div class = "notebox warning">
            </div>
            */
            ```
            - id selectors
            ```css
            h1#heading{
                ...
            }
            /* targets
            <h1 id = "heading">
            </h1>
            */
            ```
    - Custom Properties (variables)
        *
        ```css
            .container {
            --main-bg-color: cornflowerblue;
            }

            /* For each class, set some colors */
            .one {
            background-color: var(--main-bg-color);
            }
            .two {
            color: black;
            background-color: aquamarine;
            }
            .three {
            background-color: var(--main-bg-color);
            }
            .four {
            background-color: var(--main-bg-color);
            }
            .five {
            background-color: var(--main-bg-color);
            }
        ```
        - using @property for controlled inheritance
            *
            ```html
            <div class="parent">
                <p>Parent element</p>
                <div class="child">
                    <p>Child element with inheritance disabled for --box-color.</p>
                </div>
            </div>
            ```
            ```css
            @property --box-color {
                syntax: "<color>";
                inherits: false;
                initial-value: cornflowerblue;
            }

            .parent {
                --box-color: green;
                background-color: var(--box-color);
            }

            .child {
                width: 80%;
                height: 40%;
                background-color: var(--box-color);
            }
            ```
