---
title: 'Event Delegation in JavaScript'
date: '2023-01-31'
---

# Event Propogation and Event Delegation

## Event Propogation: Bubbling

**Event bubbling** occurs when a DOM event is fired and the event traverses up the DOM tree to ancestor nodes. The event *"bubbles up"* to parent nodes. For example, we have this HTML:

```
<ul id="list">
    <li>item 1</li>
    <li>item 2</li>
    <li id="item-3">item 3</li>
    <li>item 4</li>
</ul>
```
And this is our JS:

```
const item3 = document.querySelector("#item-3");

item3.addEventListener("click", (event) => {
  console.log("child");
});

const list = document.querySelector("#list");

list.addEventListener("click", (event) => {
  console.log("parent");
});

```


When you click on `<li id="item-3">item 3</li>`, you will notice that both "child" and "parent" is logged to the console whereas clicking on any of the other `<li>` elements will log only "parent". The click event for inner child element will "bubble up" to the ancestor elements which is why both statements are logged to the console.

## Event Propogation: Capturing

**Event capturing** works similarly to bubbling, but the order of triggered events is reversed. In our JS, we will add a third argument to the parent listener:

```
list.addEventListener("click", (event) => {
  console.log("parent");
}, true);
```

The third argument is for capturing, which means that the order of the event will be reversed when fired. Now, if we click on the item3 child element, we will see "parent" and then "child" logged to the console.

When events bubble and capture, they go all the way up the DOM and can trigger other handlers. We can prevent his by adding the `stopPropogation()` method on the event, like so:

```
item3.addEventListener("click", (event) => {
  console.log("child");
  event.stopPropagation();
});
```

We can use bubbling and capturing to our advantage through **event delegation**.


------------------
## Event Delegation
Event delegation is when you add one event listener to a parent element and that event listener can be used by its children. For example, you can have this as your HTML:

```
<ul id="list">
    <li>item 1</li>
    <li>item 2</li>
    <li>item 3</li>
    <li>item 4</li>
</ul>
```
And this can be your JS:

```
const list = document.querySelector('#list');

list.addEventListener('click', event => {
    console.log(event.target);
});

```

If we click on one of the list items, then we will get that list item logged to the console. This is much more efficient than adding an event listener to each element separately. 

The code is more performant and it helps organize the code much better. Thanks to bubbling and capturing, event delegation is possible and brings us this efficient code. 

This works even if we dynamically add more list items with JavaScript. The event listener will be applied to any newly created items. This is great because again we don't need to worry about adding an event listener for the new items since the parent node has the listener already. 


Code Sandbox: https://codesandbox.io/s/ecstatic-butterfly-by9r4?file=/src/index.js


Sources:
- https://javascript.info/event-delegation
- https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_delegation
- https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#bubbling_and_capturing_explained
- https://www.youtube.com/watch?v=pKzf80F3O0U&ab_channel=dcode
- https://www.youtube.com/watch?v=3KJI1WZGDrg&list=PLlasXeu85E9eLVlWFs-nz5PKXJU4f7Fks&index=7&ab_channel=AkshaySaini

