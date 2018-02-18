# Lesson 2.3 - Building up

* Media Query syntax:

```courses
@media screen and (css) [and/or (css)]
```

* Basic Media Query example:

```CSS
@media screen and (max-width: 400px) {
	body {
		background-color: red;
	}
}

@media screen and (min-width: 401px) and (max-width: 599px) {
	body {
		background-color: green;
	}
}

@media screen and (min-width: 600px) {
	body {
		background-color: blue;
	}
}
```

* Flexbox container:

```CSS
display: flex;
flex-wrap: wrap;
```

* Flexbox intem order

```CSS
order: N
```

where ```N``` = 0, 1, ..

* Articles about:
   * [using media queries](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries);
   * [basic concepts of  grid layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/Basic_Concepts_of_Grid_Layout).
