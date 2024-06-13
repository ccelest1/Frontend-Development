# External Links in New Tabs
- Opening external links in new tabes
* Automated this process for turning links -> target = "_blank"
```js
    function setTargetBlankOnExternalLinks() {
  const links = document.getElementsByTagName('a')

  for (var i = 0; i < links.length; i++) {
    if (/^http/.test(links[i].getAttribute('href'))) {
      const link = links[i]
      link.setAttribute('target', '_blank')
      link.setAttribute('rel', 'noopener noreferrer')
    }
  }
}

setTargetBlankOnExternalLinks() // call this after the DOM is ready
```
* Find all links -> loop through them -> check if they start with 'http'
    - -> use test() v match() as test() returns boolean (relative links given a pass, links that contain http elsewhere in content v prefix) in order to avoid mistake mods
