# Intro to D3, Data Visualization
```json
{
  "teammates": [
    {
      "id": 1,
      "name": "Charlie",
      "birthdate": "1988-10-17",
      "position": "DevOps",
      "startdate": "2015-07-06"
    },
    {
      "id": 2,
      "name": "Eman",
      "birthdate": "1986-09-16",
      "position": "Front End",
      "startdate": "2015-05-12"
    }
    // More teammates here...
  ]
}
```
- create bar char graphing dev's age, how long they have had job positions
    * represented on x axis, radio button that toggles between the two -> then add sorting methods to y axis
    * [Reusable Charts](https://bost.ocks.org/mike/chart/)
    ```js
    // ensure d4 chart obj exists in order for custom function to be set as prop on d3 chart object
        if(!d3.chart){
            d3.chart={}
        }
        d3.chart.fine_visual = function(){
            // scoped declarations
            var data, svg

            // chart function returned when calling visual function
            function chart(){
                // initialize static elements of visualization

                /*
                would contain code for x-axis, y-axis (scale)
                adding axis to visual involves making decision about what scale is to be used
                consider linear, time scales -> refer to d3 scales

                Linear scale is declared with domain set from 0 to max age of dev team, range from 0 to full visualization width
                axis is declared -> called on svg var in appended g element

                var xScale = d3.scale
                    .linear()
                    .domain([
                        0,
                        d3.max(data, function (d) {
                        return age(d.birthdate)
                        }),
                    ])
                    .range([0, width])

                    var xAxis = d3.svg.axis().scale(xScale).orient('bottom')

                    var xAxisGroup = svg.append('g')

                    xAxis(xAxisGroup)
                */

                svg = container
                // invoke update fn -> apply data to visualization
                update()
            }

            // set property on chart fxn of update fxn -> call chart updates
            chart.update = update

            function update(){
                // visualization dynamic elements
            }

            // data setter
            chart.data = function(value){
                if(!arguments.length) return data
                data = value
                return chart
            }
            return chart
        }

        d3.json('teammates.json', function (err, teammates) {
            if (err) {
            return d3
            .select('.js-display')
            .append('p')
            .text('There was an error retrieving the data')
        }

        var data = teammates.teammates
        // req chart.property to create getters, setter for property (user customization)
        var display = d3.select('.js-display')

        var visual = d3.chart.fine_visual().data(data)
        visual(display)
        })
    ```

## D3 Pattern - Selecting, Entering, Appending, Exiting, Removing
- axes in place, bars can be added to chart -> D3 has specific pattern in rendering data driven elements to visual
    * select all elements -> enter data -> update existing elements -> add new -> destroy unused
```js
var bars = svg.selectAll('.bar')
  .data(data);
  .enter()
  .append('rect').classed('bar', true)
  .attr({
    x: 0,
    y: function(d,i) { return i * 35; },
    width: function(d) { return xScale( age(d.birthdate) ); },
    height: 30,
  })
  .exit()
  .remove();
  ```
