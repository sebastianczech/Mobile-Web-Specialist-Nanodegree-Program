# Lesson 2.9 - Full Responsiveness

* In JavaScript you can get the source of an ```img``` element with ```currentSrc```.

* The ```sizes``` attribute gives the browser information about the display size of an image element – it does not actually cause the image to be resized. That's done in CSS!

* ```srcset``` - there are two flavors of ```srcset```, one using x to differentiate between device pixel ratios (DPR), and the other using w to describe the image's width:

```html
<img src="image_2x.jpg" srcset="image_2x.jpg 2x, image_1x.jpg 1x" alt="a cool image">
```

`srcset` is a comma separated string of filename multiplier pairs, where each multiplier must be an integer followed by an `x`.
`1x` represents 1x displays and `2x` represents displays with twice the pixel density, like Apple's Retina displays. The browser will download the image that best corresponds to its DPR

```html
<img src="image_200.jpg" srcset="image_200.jpg 200w, image_100.jpg 100w" alt="a cool image">
```

`srcset` is a comma separated string of filename widthDescriptor pairs, where each widthDescriptor is measured in pixels and must be an integer followed by a `w`. Here, the widthDescriptor gives the natural width of each image file, which enables the browser to choose the most appropriate image to request, depending on viewport size and DPR. Without the widthDescriptor, the browser cannot know the width of an image without downloading it.

* ```src``` attribute is a fallback.

* `sizes` consists of comma separated mediaQuery width pairs. `sizes` tells the browser early in the load process that the image will be displayed at some width when the mediaQuery is hit.

```html
<img  src="images/great_pic_800.jpg"
      sizes="(max-width: 400px) 100vw, (min-width: 401px) 50vw"
      srcset="images/great_pic_400.jpg 400w, images/great_pic_800.jpg 800w"
      alt="great picture">
```

* if `sizes` is missing, the browser defaults sizes to 100vw, meaning that it expects the image will display at the full viewport width.

* `picture` example:

```html
<picture>
  <source media="(min-width: 512px)" srcset="images/example.jpg">
  <source media="(min-width: 1024px)" srcset="images/example.jpg">
  <img src="images/example.jpg" alt="Example title">
</picture>
```

* `alt` attributes should be descriptive for important images, like this body surfer. Because body surfing is important, I guess.

* `alt` attributes should be empty for images that are just decorations, like this boiler image.

* `alt` attributes should be set on every image.

* Articles and links:
   * [Responsive Hero Images](https://cloudfour.com/thinks/responsive-hero-images/)
   * [Responsive Images Audits](https://cloudfour.com/thinks/responsive-images-audits/)
   * [Srcset and sizes](http://ericportis.com/posts/2014/srcset-sizes/)
   * [Phone Pixel Density (PPI) List](http://pixensity.com/list/phone/)
   * [High DPI Images for Variable Pixel Densities](https://www.html5rocks.com/en/mobile/high-dpi/)
   * [Working with h units](https://github.com/ResponsiveImagesCG/picture-element/issues/86)
   * [High DPI Images for Variable Pixel Densities](https://www.html5rocks.com/en/mobile/high-dpi/)
   * [A new image format for the Web](https://developers.google.com/speed/webp/?csw=1)
   * [Built-in Browser Support for Responsive Images](https://www.html5rocks.com/en/tutorials/responsive/picture-element/)
   * [Picturefill](http://scottjehl.github.io/picturefill/)
   * [CSS Element Queries](https://github.com/marcj/css-element-queries)
   * [Responsive Images: Use Cases and Documented Code Snippets to Get You Started](https://dev.opera.com/articles/responsive-images/)
   * [Responsive images group](http://responsiveimages.org/)
