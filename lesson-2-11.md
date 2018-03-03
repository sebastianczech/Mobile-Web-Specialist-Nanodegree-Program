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

* The ARIA Authoring Practices doc (or "ARIA Design Patterns doc") is a great resource for figuring out what kind of keyboard support your complex components should implement:
   * [WAI-ARIA Authoring Practices 1.0](https://www.w3.org/TR/wai-aria-practices/)
   * [WAI-ARIA Authoring Practices 1.1](https://www.w3.org/TR/wai-aria-practices-1.1/)cl

* Move focus to a heading in the new page:
```javascript
  newPage.querySelector('h2').focus();
```

* Change focus for radiou group:
```javascript
RadioGroup.prototype.changeFocus = function(idx) {
  // Set the old button to tabindex -1
  this.focusedButton.tabIndex = -1;
  this.focusedButton.removeAttribute('checked');

  // Set the new button to tabindex 0 and focus it
  this.focusedButton = this.buttons[idx];
  this.focusedButton.tabIndex = 0;
  this.focusedButton.focus();
  this.focusedButton.setAttribute('checked', 'checked');
};
```

* To find your missing focus you can type the following into your console:

```javascript
document.activeElement
```

* Articles and links:
   * [Keyboard Accessible: Make all functionality available from a keyboard](https://webaim.org/standards/wcag/checklist#sc2.1.1)
   * [Adaptable: Create content that can be presented in different ways (for example simpler layout) without losing information or structure](https://webaim.org/standards/wcag/checklist#sc1.3.2)
   * [tabindex](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/tabindex)
   * [Sequential focus navigation](https://www.w3.org/TR/html5/editing.html#sequential-focus-navigation-and-the-tabindex-attribute)
   * ["Skip Navigation" Links](https://webaim.org/techniques/skipnav/)
   * [Removing Headaches from Focus Management](https://developers.google.com/web/updates/2016/03/focus-start-point?hl=en)
   * [DocumentOrShadowRoot.activeElement](https://developer.mozilla.org/en-US/docs/Web/API/DocumentOrShadowRoot/activeElement)
   * [The Dialog element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog)
   * [No Keyboard Trap](https://webaim.org/standards/wcag/checklist#sc2.1.2)
