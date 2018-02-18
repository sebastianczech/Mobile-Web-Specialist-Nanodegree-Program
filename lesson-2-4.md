# Lesson 2.4 - Common responsive patterns

* Most used patterns:
   * _Column drop_
   * _Mostly fluid_
   * _Layout shifter_
   * _Off canvas_

### Example of code in pattern _off canvas_ used to create menu with toggle:

JavaScript used to toggle the open class:

``` JavaScript
menu.addEventListener('click', function(e) {
  drawer.classList.toggle('open');
  e.stopPropagation();
});
```

CSS for transitioning the hamburger menu:

``` CSS
nav {
  width: 300px;
  position: absolute;
  /* This trasform moves the drawer off canvas. */
  -webkit-transform: translate(-300px, 0);
  transform: translate(-300px, 0);
  /* Optionally, we animate the drawer. */
  transition: transform 0.3s ease;
}
nav.open {
  -webkit-transform: translate(0, 0);
  transform: translate(0, 0);
}
```

* Article about [events and event propagation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Examples#Example_5:_Event_Propagation).
