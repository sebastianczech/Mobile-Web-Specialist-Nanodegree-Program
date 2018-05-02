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

* _RAIL_ - Responsive, Animations, Idle, Load.
   * Responsive: 100 ms
   * Animations: 16 ms 
   * Idle: 50 ms 
   * Load: 1 s
   
* After load app is idle.

*  Animations: [irst Last Invert Play](https://github.com/udacity/devsummit/blob/master/src/static/scripts/components/card.js)

* ```opacity``` and ```transform``` only trigger composite, there are no changes in HTML->DOM, CSS->CSSOM, DOM+CSSOM->Render tree, Layout or Paint.

* Anything that moves should run at 60 FPS.

## Weapons of Jank Destruction
## JavaScript
## Styles and Layout
## Compositing and Painting
