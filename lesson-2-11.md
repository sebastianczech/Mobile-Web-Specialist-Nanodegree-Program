# Lesson 2.11 - Focus

* Move focus around the page using your keyboard:
   * **TAB** will move focus forward
   * **SHIFT** - TAB will move focus backwards
   * **Arrow keys** can be used to navigate inside of a component


* The `tabindex` global attribute indicates if its element can be focused, and if/where it participates in sequential keyboard navigation. It accepts an integer as a value:
   * A negative value (usually tabindex="-1") means that the element should be focusable, but should not be reachable via sequential keyboard navigation.
   * tabindex="0" means that the element should be focusable in sequential keyboard navigation, but its order is defined by the document's source order.
   * A positive value means the element should be focusable in sequential keyboard navigation, with its order defined by the value of the number. That is, tabindex="4" would be focused before tabindex="5", but after tabindex="3". If multiple elements share the same positive tabindex value, their order relative to each other follows their position in the document source.


 * Using `tabindex` with positive values is *anti-pattern*


* Articles and links:
   * [Keyboard Accessible: Make all functionality available from a keyboard](https://webaim.org/standards/wcag/checklist#sc2.1.1)
   * [Adaptable: Create content that can be presented in different ways (for example simpler layout) without losing information or structure](https://webaim.org/standards/wcag/checklist#sc1.3.2)
   * [tabindex](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)
   * [Sequential focus navigation](https://www.w3.org/TR/html5/editing.html#sequential-focus-navigation-and-the-tabindex-attribute)
