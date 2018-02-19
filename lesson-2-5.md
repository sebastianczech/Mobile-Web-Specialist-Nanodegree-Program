# Lesson 2.5 - Optimizations

* Responsive table - hidden column

```CSS
@media screen and (max-width: 400px) {
	.object-class-to-hide {
		display: none;
	}
}
```

* Responsive table - contained table

```css
div.contained_table {
  width: 100%;
  overflow-x: auto;
}
```

* Articles about:
   * [responsive images](https://developers.google.com/web/fundamentals/design-and-ux/responsive/images),
   * [difference between "display: none" and "visibility: hidden" in CSS](https://www.thoughtco.com/display-none-vs-visibility-hidden-3466884),
   * [responsive data table roundup](https://css-tricks.com/responsive-data-table-roundup/),
   * [truncate string with ellipsis](https://css-tricks.com/snippets/css/truncate-string-with-ellipsis/),
   * [pure CSS for multiline truncation with ellipsis](http://hackingui.com/front-end/a-pure-css-solution-for-multiline-text-truncation/).
