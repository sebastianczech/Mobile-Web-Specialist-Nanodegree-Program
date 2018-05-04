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

*  Animations: [FLIP - First Last Invert Play](https://github.com/udacity/devsummit/blob/master/src/static/scripts/components/card.js)

* ```opacity``` and ```transform``` only trigger composite, there are no changes in HTML->DOM, CSS->CSSOM, DOM+CSSOM->Render tree, Layout or Paint.

* Anything that moves should run at 60 FPS.

* Time table:

![Time table](http://udacity.github.io/60fps/images/time-table.jpg)

## Weapons of Jank Destruction

* Chrome DevTools and Timeline

* Test all devices (e.g. using emulators and simulators)

## JavaScript

* JavaScript Compilers - _Just In Time (JIT)_

* We shouldn't concern about micro-optimizations (using ```for``` or ```while``` loop)

* As JS can trigger every part of the rendering pipeline, it makes sense to run it as early as possible each frame.

* [requestAnimationFrame polyfill](https://gist.github.com/paulirish/1579671)

* 1000ms / 60 = 16 ms -> 10 ms is the target time for animation

* ```setTimeout```, ```setInterval``` are not good for animations. ```requestAnimationFrame``` should be used.

* JavaScript profiler in Chrome DevTools

* [Web Workers Demo](https://github.com/udacity/web-workers-demo) and [Web Workers Demo Solution](https://github.com/udacity/web-workers-demo/tree/solution)

* [Using Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)

* [The Basics of Web Workers](https://www.html5rocks.com/en/tutorials/workers/basics/)

* [Writing Fast, Memory-Efficient JavaScript on Smashing Magazine](https://www.smashingmagazine.com/2012/11/writing-fast-memory-efficient-javascript/)

* [Memory Management on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)

* [High-Performance, Garbage-Collector-Friendly Code on Build New Games](http://buildnewgames.com/garbage-collector-friendly-code/)

* [QR Code App](https://github.com/udacity/qrcode)

## Styles and Layout

* __BEM__ - block element modifier

* [BEM - key concepts](https://en.bem.info/methodology/key-concepts/)

* [BEM and SMACSS: Advice From Developers Who’ve Been There](https://www.sitepoint.com/bem-smacss-advice-from-developers/)

* [How (not) to trigger a layout in WebKit ](http://gent.ilcore.com/2011/03/how-not-to-trigger-layout-in-webkit.html)

* __FSL__ - forced synchronous layout

* Batch your style changes and avoid running layout as much as possible.

## Compositing and Painting
