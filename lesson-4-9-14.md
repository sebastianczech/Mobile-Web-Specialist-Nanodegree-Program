# Browser Rendering Optimization

## The Critical Rendering Path

* Avoid _juddering_

* Frames - 60 Hz = 60 fps (frames per second)

* __DOM + CSS = Render Tree__

* Only visible elements are in render tree e.g. elements that are ```display: none``` don't show up in the render tree.

* [Performance Analysis Reference ](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference)

* __JavaScript__ or CSS anims or Web animation API) > __Style__ > __Layout__ > __Paint__ > __Composite__

* [Introducing 'layout boundaries'](http://wilsonpage.co.uk/introducing-layout-boundaries/)

* [CSS triggers](https://csstriggers.com/)

## App Lifecycles
## Weapons of Jank Destruction
## JavaScript
## Styles and Layout
## Compositing and Painting
