# Lesson 2.2: Staring small

* Materials about[units & measurements, including device-independent pixels](https://material.io/guidelines/layout/units-measurements.html).

* The viewport and the device pixel ratio are both likely causes for the differences between devices.

* ``HP / DRP = CP``, where ``HP`` - hardware pixels,  ``DPR`` - device pixel ratio, ``CP`` -  CSS pixels.

* ``VW`` = ``HP / DPR``, where ``VW`` - width of the viewport.

* Article about [setting viewport](https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag).

* Example of viewport:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

* Recommendation about using relative units when specifying width of elements.

* Recommendation about using max-width on elements e.g.:

```CSS
img {
  max-width: 100%;
}
```
* Minimal size of buttons should be ``48px``:

```CSS
nav a, button {
  min-wdith: 48px;
  min-height: 48px;
}
```

* It is recomended to add additional padding between links:

```CSS
.nav a {
  ...
  padding: 1.5em;
  ...
}
```

* Start design form smartphone, then for tablet and then for computer (*from small to large*).
