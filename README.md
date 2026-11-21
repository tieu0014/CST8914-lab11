# Lab 11: Accessible Custom Components & Widgets
## Missing Keyboard Interaction
According to [w3](https://www.w3.org/WAI/ARIA/apg/patterns/accordion/examples/accordion/) all keyboard functions were supported. Space and enter collapsed and expanded, Tab moved to the next focusable element, Shift + Tab moved to the previous focusable element.
<img width="652" height="456" alt="image" src="https://github.com/user-attachments/assets/d306f342-ee83-425d-b769-c56c6423dfc8" />
## Missing ARIA
'aria-expanded' attribute was missing from all buttons </br>
'aria-controls' attribute missing from all buttons </br>
'aria-role' attribute was missing from all accordian content </br>
'aria-labelledby' attribute was missing from all accordian content </br>
For this lab the provided JavaScript was modified to use 'aria-expanded' and 'hidden' attributes. But could be replaced with the following code from [w3](https://www.w3.org/WAI/content-assets/wai-aria-practices/patterns/accordion/examples/js/accordion.js):
```
/*
 *   This content is licensed according to the W3C Software License at
 *   https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document
 *
 *   Simple accordion pattern example
 */

'use strict';

class Accordion {
  constructor(domNode) {
    this.rootEl = domNode;
    this.buttonEl = this.rootEl.querySelector('button[aria-expanded]');

    const controlsId = this.buttonEl.getAttribute('aria-controls');
    this.contentEl = document.getElementById(controlsId);

    this.open = this.buttonEl.getAttribute('aria-expanded') === 'true';

    // add event listeners
    this.buttonEl.addEventListener('click', this.onButtonClick.bind(this));
  }

  onButtonClick() {
    this.toggle(!this.open);
  }

  toggle(open) {
    // don't do anything if the open state doesn't change
    if (open === this.open) {
      return;
    }

    // update the internal state
    this.open = open;

    // handle DOM updates
    this.buttonEl.setAttribute('aria-expanded', `${open}`);
    if (open) {
      this.contentEl.removeAttribute('hidden');
    } else {
      this.contentEl.setAttribute('hidden', '');
    }
  }

  // Add public open and close methods for convenience
  open() {
    this.toggle(true);
  }

  close() {
    this.toggle(false);
  }
}

// init accordions
const accordions = document.querySelectorAll('.accordion h3');
accordions.forEach((accordionEl) => {
  new Accordion(accordionEl);
});
```
