# Lesson 2.6 - 2.7 - Getting up and running, Units, Formats and Environments

* ``total bits = pixels * bits per pixel``

* Window size may change

* ``max-width: 100%`` is your friend

* Example of relative sizing: ``width: calc((100% - 10px)/2)``. Note: There MUST be a space on each side of the ``+`` and ``-`` operators. (A space is not required around ``*`` and ``/`` as the problem is an ambiguity around negation.) For example: ``calc(100px - 10%)`` will work. ``calc(100px-10%)`` will not.

* VH unit - 1% of the viewport height, so 100vh = 100% height

* VW unit - 1% of the viewport width, so 100vw = 100% width

* Vmin unit, viewport minimum - 1% of the viewport width or height, whichever is smaller

* Vmax unit, viewport maximum - 1% of the viewport width or height, whichever is greater

* Raster (e.g. photos) [``JPEG``, ``WebP``] and vector images (e.g. logos and icons) [``SVG``, ``PNG``], which can be scaled infinitely

* Links:
   * [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
   * [Grunt PageSpeed plugin](https://www.npmjs.com/package/grunt-pagespeed)
   * [PageSpeed Node module](https://github.com/addyosmani/psi/)
   * [cURL examples](https://www.thegeekstuff.com/2012/04/curl-examples/)


* Articles:
   * [using `calc()` CSS function for calculations when specifying CSS property values](https://developer.mozilla.org/en-US/docs/Web/CSS/calc)
   * [PNG, GIF, or JPEG? Which is the Best Image Format for Email?](https://litmus.com/blog/png-gif-or-jpeg-which-ones-should-you-use-in-email)
   * [Image optimization](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization)
   * [A new image format for the Web](https://developers.google.com/speed/webp/?csw=1)
   * [WebP image format](https://caniuse.com/#feat=webp)
