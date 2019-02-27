# Shadow DOM 101
  * Shadow DOM removes the brittleness of building web apps. The brittleness comes from the global nature of HTML, CSS, and JS
  * Shadow DOM is one of the four Web Component standards:
      1. [HTML Templates](https://www.html5rocks.com/en/tutorials/webcomponents/template/)  
      2. [Shadow DOM](https://dom.spec.whatwg.org/#shadow-trees)
      3. [Custom elements](https://developers.google.com/web/fundamentals/web-components/customelements)
      4. [HTML Imports](https://www.html5rocks.com/en/tutorials/webcomponents/imports/)
## Benifits
  #### 1.Isolated DOM: 
  A component's DOM is self-contained (e.g. document.querySelector() won't return nodes in the component's shadow DOM).
  
  #### 2. Scoped CSS: 
  CSS defined inside shadow DOM is scoped to it. Style rules don't leak out and page styles don't bleed in.
  
  #### 3.Composition: 
  Design a declarative, markup-based API for your component.
  
  #### 4.Simplifies CSS - Scoped DOM 
  means you can use simple CSS selectors, more generic id/class names, and not worry about naming conflicts.
  
  #### 5.Productivity 
  Think of apps in chunks of DOM rather than one large (global) page.

## Basic Terminologies
  ### 1.DOM
   what we get over a wire (or wireless) is just a string of text to render something
   on the screen, the browser have to convert it to a data model in a tree structure.
   We need that to make the machines understand our documents better.
   The tree that the browser created it called DOM (Document Object Model).

  ### 2.Shadow Root
  What gets attached to an element to give that element it's Shadow DOM. Technically it's
  a non-element node, a special kind of Document Fragement
```html
<custom-picture>
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  #Shadow-root

  ------------------------------------------
  <!-- Light DOM -->
</custom-picture>
```
  ### 3.Shadow Host
  The element to which the Shadow root is attached

  ### 4.Light DOM
  The DOM which the user of your element writes.
```html
<custom-picture>
  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  #Shadow-root

  ------------------------------------------
  <!-- Light DOM Starts -->
  <img src="http://placehold.it/100/100" >
  <cite>Have a nice day </cite>
  <!-- Light DOM Ends-->
</custom-picture>
```

  ### 5.Document Fragement
  The minimal document Object that has no parent. it's used as a light version of the document
  to store a segment of document structure comprised of nodes just like a standard document [MDN](http://developer.mozilla.org/en-us/docs/web/API/DocumentFragment)

  <img src="https://mdn.mozillademos.org/files/15788/shadow-dom.png">
  
## How To Create Shadow DOM
```html
<div class="dom"></div>
```
```javascript
let el = document.querySelector('.dom');
el.attachShadow({mode: "open"});
// Just like prototype & constructor bi-directional references, we have...
el.shadowRoot // the shadow root.
el.shadowRoot.host // the element itself.

// put something in shadow DOM
el.shadowRoot.innerHTML = "Hi I am shadowed!";

// Like any other normal DOM operation.
let hello = document.createElement("span");
hello.textContent = "Hi I am shadowed but wrapped in span";
el.shadowRoot.appendChild(hello);
```
## What happens if we use an input element instead of the div to attach the shadow DOM?
  * Well, it doesn't work. Because the browser already hosts it's own shadow DOM for those elements. Bunch of red colored english alphabets will be thrown at console's face
  * Shadow DOM, cannot be removed once created. It can only be replaced with a new one.


## Shadow DOM Modes
  - **Open:**
   means that the shadow DOM can be accessible by JavaScript
```javascript
let el = document.querySelector('.dom');
el.attachShadow({mode: "open"});
// Just like prototype & constructor bi-directional references, we have...
el.shadowRoot // the shadow root.
```
  - **Closed:**
  The element can't be acessible by JavaScript
```javascript
el.shadowRoot; // null
```

## Styles inside Shadow DOM.
* They are scoped
* They can have simple names.

```html
<custom-picture>
    #shadow-root 
        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        <style>
            /*Applies only to spans inside shadow DOM. Doesn't leak out.*/
            span {
                color: red;
            }
        </style>
        <span>Hello!</span>
</custom-picture>
```
* Can have stylesheets 
```html
<custom-picture>
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    #shadow-root 
        <!--All styles coming from custom-picture.css will be scoped inside this shadow root-->
        <link rel="stylesheet" href="custom-picture.css">
        <span>Hello!</span>
</custom-picture>
```

* **:host** targets the host element. BUT!
```html
/* winner */
custom-picture {
    background: red;
}
/* loser */
#shadow-root
    <style>
        :host {
            background: green;
        }
    </style>
```

* :host(<selector>):
```html
  :host([disabled]){
    ...
}

:host(:focus){
    ...
}

:host(:focus) span {
    /*change all spans inside the element when the host has focus*/
}
```

## Slot Element
Slots are placeholders inside your component that users can fill with their own markup. By defining one or more slots, you invite outside markup to render in your component's shadow DOM

## Terminology: light DOM vs. shadow DOM
### Light DOM
```html
<better-button>
  <!-- the image and span are better-button's light DOM -->
  <img src="gear.svg" slot="icon">
  <span>Settings</span>
</better-button>
```
### Shadow DOM
```html
#shadow-root
  <style></style>
  <slot name="icon"></slot>
  <span id="wrapper">
    <slot>Button</slot>
  </span>
```

### Flatted DOM
The result of the browser distributing the user's light DOM into your shadow DOM, rendering the final product. The flattened tree is what you ultimately see in the DevTools and what's rendered on the page.
```html
<better-button>
  #shadow-root
    <style></style>
    <slot name="icon">
      <img src="gear.svg" slot="icon">
    </slot>
    <span id="wrapper">
      <slot>
        <span>Settings</span>
      </slot>
    </span>
</better-button>
```


## Resources
  1. [MDN](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_shadow_DOM)
  2. [Google Developers](https://developers.google.com/web/fundamentals/web-components/shadowdom)
  3. [fancy-tabs example](https://gist.github.com/ebidel/2d2bb0cdec3f2a16cf519dbaa791ce1b)
  4. [bitsofco](https://bitsofco.de/what-is-the-shadow-dom/)
