# Svelte

[TOC]


## Reactive Programming

- specifies the dynamic behavior of a value completely at the time of declaration
- needs way to assign variable that doesn't copy values but only link, like mathematical variables or spreadsheet cells

```javascript
// pseudocode
a = 1;
b = a * 2;
a = 21;
/* b should now be 42 */
```

- could be implemented as function which recomputes dependent value only if independent value changed

```javascript
let a = 1;

const b = (function () {
    let oldA = a;
    let cache;

    return function () {
        if (oldA === a && cache !== undefined) {
            return cache;
        } else {
            cache = a * 2;
            oldA = a;
            return cache;
        }
    };
}());

a = 21;
b(); // 42
```

- needs code that continuously "reacts" based on different outside conditions, e.g. the independent variable is the mouse coordinates and all dependent variables are computed from it as soon as it changes
- reactive JS frameworks make HTML reactive
- can use JS variables in HTML, update DOM as soon as the value in JS updates
- without needed to manually hop out to JS, attach event handler, select DOM element, update it
- frameworks abstracts away the manual JS code, write HTML as if it had reactive variables
- but at end of day is just the same JS, variables can only live in JS
- just developing experience is more integrated, dynamisism not on top of HTML but inside it
- "keep DOM in sync with your application state", means update DOM elements using the values of the variables in the components
- beware: can use reaction to change DOM because every DOM node is independent, but can't use reaction to change parts of JS code, since code is interdependent on other code is not possible to change out a bit without affecting rest, e.g. what would happen to other lines using `b`, would need to reexecute them, but then needs to reexecute the lines that depend on those lines as well, but in what order, nondeterministic issue, also can't just execute whole file again since would just get in same problem


## Introduction

- `.svelte` components combine HTML, JS, and CSS
- components get compiled to JS classes, can be imported and instantiated
- scripts and styles are scoped by default to component, no need to use deep class structures anymore to target elements from within JS or CSS
- top-level is HTML, styles and scripts go inside their tags
- can use JS expressions inside `{}` in HTML, like template literals in JS without `$`

```html
<script>
  const name = "Peter"
</script>

<p>Hi, I am {name}.</p>
```

- DOM is automatically updated as soon as JS expressions change, e.g. using event handler

```html
<script>
	let count = 0;
</script>

<p>You clicked the button {count} {count === 1 ? 'time' : 'times'}.</p>

<button on:click={() => {count += 1}}>Submit</button>
```

- reactivity is implemented using labeled statement `$` in JS, executed if value of referenced variables changes
- reactive declarations update affected DOM nodes as soon as value changes

```html
<script>
  let x = 0;
  $: y = x * 2;
</script>

<p>You did not click the button {y} times.</p>

<button on:click={() => {x += 1}}>Submit</button>
```

- reactive statements get executed as soon as referenced values change

```html
<script>
  let x = 0;

  $: {
  console.log(`The current value is ${x}.`);
  }
</script>

<p>You clicked the button {x} times.</p>

<button on:click={() => {x += 1}}>Submit</button>
```

- since reactivity is triggered by assignments, mutating objects won't trigger an update, e.g. e.g. `obj.foo = 42`, `arr.push(42)`, `arr[0] = 42`, etc., need to do a useless self assign afterwards to trigger, e.g. `obj = obj`
- beware: doesn't make sense to use reactive declarations in JS itself, see [Reactive Programming](#) ❗️



## Summary

- use small enough components that bundling HTML, JS and CSS in one file makes sense again



## Open questions

### Markup

- what are components compiled to
- what about the `index.html`, what is in it if all is in `.svelte`?

### Styles

- what about SASS features
- global variables, e.g. primary color
- how do you layout components

### Scripts



## Ideas

- why don't implement reactive operator in JS for reactive declaration, e.g. `y :: x * 2`;
- why don't put HTML, JS, and CSS all top level? no separate `style` or `script` tags