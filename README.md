# q-select-issue

This project is used to reproduce an issue in Quasar.

It is a simple vue application, which has a component "MySelect", which uses QSelect.
The select has an input and uses the filter event to filter its options.

App.vue renders MySelect two times: The first time as a normal component, the second time as a custom element, which renders MySelect inside a shadow DOM.


## Problem description
When QSelect is used in a shadow DOM and a callback for the filter event is used for filtering the options, 
the menu disappears after clicking a second time into the input field. 
Unlike when used outside the shadow DOM, the menu does not reappear when doing a third click into the input. 
The user has to click outside the QSelect and back into it again, to make the menu reappear.

This happens, because Quasar uses document.activeElement in use-field.js to check if the control is still the active element. 
When the active element is inside a shadow DOM, document.activeElement returns the shadow DOM's host element, 
not the active element inside the shadow DOM. 
Instead, the activeElement of the shadow DOM must be considered.

Additionally, Quasar uses the target property of a mousedown event in click-outside.js to check if the user clicked outside an element.
When the target of an event is inside a shadow DOM, the element becomes retargeted, so that the event's target is the shadow DOM's host element.
When working with a shadow DOM, you can instead check if the element is contained in the event's composedPath. 
The composedPath also contains the shadow DOM's elements, as long as it is not closed.

