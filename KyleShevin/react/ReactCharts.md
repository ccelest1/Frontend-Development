# Making React Charts
```js
// static array containing data to be manipulated/rendered or perhaps pulled from api call from be
const data = [
  {
    name: 'kentcdodds',
    repos: 371,
  },
  {
    name: 'sindresorhus',
    repos: 909,
  },
  {
    name: 'developit',
    repos: 222,
  },
  {
    name: 'getify',
    repos: 43,
  },
  {
    name: 'btholt',
    repos: 56,
  },
  {
    name: 'kyleshevlin',
    repos: 82,
  },
]
// build comps to represent data, start w/ Chart comp + Bar comp

// chart comp creates svg based on width, height passed in
const Chart = ({ children, width, height }) => (
  <svg viewBox={`0 0 ${width} ${height}`} width={width} height={height}>
    {children}
  </svg>
)

// creates rect element that is passed as child of Chart comp
const Bar = ({ x, y, width, height }) => (
  <rect x={x} y={y} width={width} height={height} />
)

// put bar, chart comps together to create BarChart comp

/*
Creates bar chart of 6 bars -> heights correspond to # of repos pertaining to each user
BarChart comp, y propr
*/
const BarChart = ({ data }) => {
  // Width of each bar
  const itemWidth = 20

  // Distance between each bar
  const itemMargin = 5

  const dataLength = data.length

  // Normalize data, we'll reduce all sizes to 25% of their original value
  const massagedData = data.map(datum =>
    Object.assign({}, datum, { repos: datum.repos * 0.25 }),
  )

  const mostRepos = massagedData.reduce((acc, cur) => {
    const { repos } = cur
    return repos > acc ? repos : acc
  }, 0)

  const chartHeight = mostRepos

  return (
    <Chart width={dataLength * (itemWidth + itemMargin)} height={chartHeight}>
      {massagedData.map((datum, index) => (
        const itemHeight = datum.repos
        <Bar
          key={datum.name}
          x={index * (itemWidth + itemMargin)}
          y={chartHeight - itemHeight}
          width={itemWidth}
          height={itemHeight}
        />
      ))}
    </Chart>
  )
}

ReactDOM.render(<BarChart data={data} />, document.getElementById('barchart'))
```
