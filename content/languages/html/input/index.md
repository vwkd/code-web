---
title: Input
author: vwkd
index: 5
tags:
  - languages
  - html
---

## `<form>`

- can contain various types of input elements
- sends key-value pair `name=value` from every input inside the form, multiple chained with &-character
- `action` attribute: URL where to send the form-data when a form is submitted, usually server-side script on destination page processes form data, default is current page, e.g. `<form action="/dest.php">`
- `method` attribute: HTTP method used to send form data, `GET` is default, appends form data into URL in name-value pairs, visible to everyone, can’t send lots of data since length of URL is limited to 2048 characters, but can be bookmarked, with `POST` is not visible in URL, can send lots of data
- `target` attribute ???


<!-- ToDo: finish -->

If a form is sent using the GET method, the data is appended to the URL after an ? as a series of name/value pairs separated by an &

If a form is sent using the POST method, the data is appended to the body of the HTTP request as a series of name/value pairs separated by an &

prefer POST over GET since:
- with GET sensitive data is visible in the URL bar, can be read by JS on resulting page, though doesn't mean it's transmitted if it's encrypted with HTTPS ??? just seems insecure
- with GET the amount of data possible to be send is limited by URL length limit

however POST doesn't make data transmission more secure, no encryption, just not visible and modifiable to end user
also via POST request not saved in browser history

<!-- ToDo: finish -->


## `<fieldset>`

- groups form elements into visible box
- header using `<legend>` element within



## `<input>`

- empty element
- `type` attribute: `text` (default), `password`, `number`, `range`, `checkbox`, `radio`, `file`, even `submit` for submit button, `reset` for reset button (better use button because can style full HTML), many new ones with HTML5 like `color`, `date`, `email`, browser shows unsupported types as `text`
- `name` attribute: key for key-value pair, if omitted input is not sent
- `value` attribute: initial value, must be set for some input types, e.g. `checkbox, radio`
- label using `<label>` element with `for`-attribute to ID of input element, label is click target for input, e.g. checks checkbox, etc.

```html
<form action="/destination.html" method="POST">
  <p>This form can contain any element</p>

  <input type="submit" value="Submit">
</form>
```

- `placeholder` attribute: hint in input field, only available on some input types

### `type="text"`

- a one-line text input field
- default width is 20 characters

```html
<label for="myid">Select username</label><br>
<input id="myid" type="text" name="username" value="lizard56">
```

```html
<label for="myid ">Select password</label>
<input id="myid" type="text" name="password">
```

- is default if `type` attribute is invalid or missing

### `type="number"`

```html
<label for="myid">Select amount</label><br>
<input id="myid" type="number" name="amount" value="6" step="0.5">
```

### `type="range"`

- slider, exact value not shown
- default range is 0 to 100

```html
<label for="myid">Select doneness</label><br>
<span>low</span>
<input id="myid" type="range" name="doneness" value="3" min="1" max="5">
<span>high</span>
```

### `type="checkbox"`

- a toggle button, select individual choices
- can pre-set checkbox with boolean attribute `checked`
- only sent if is checked

```html
<span>Select topping</span><br>
<input id="myid1" type="checkbox" name="topping" value="lettuce">
<label for="myid1">Lettuce</label>
<input id="myid2" type="checkbox" name="topping" value="tomato">
<label for="myid2">Tomato</label>
<input id="myid3" type="checkbox" name="topping" value="onion">
<label for="myid3">Onion</label>
```

### `type="radio"`

- a radio button, select one of several choices
- specify choices using identical `name` attribute
- can pre-set radio button with boolean attribute `checked`
- only sent if is checked
- beware: can't uncheck without checking another choice, i.e. once checked can not go back into undetermined state ❗️

```html
<span>Did you like it?</span><br>
<input id="myid1" type="radio" name="feedback" value="yes">
<label for="myid1">Yes</label>
<input id="myid2" type="radio" name="feedback" value="no">
<label for="myid2">No</label>
<input id="myid3" type="radio" name="feedback" value="maybe">
<label for="myid3">Maybe</label>
```

### `type="hidden"`

- a hidden input field, e.g. include timestamp in form

```html
<input type="hidden" name="timestamp" value="1286705410">
```

### `type="submit"`, `type="reset"`, `type="button"`

- same as for `<button>` element, but only plain-text content since is empty element
- use `<button>` element instead ❗️

### `type="image"`

- image as `submit` button
- as value the coordinates of the click relative to the image are submitted

### `type="file"`

- file picker
- `accept` attribute: allowed file types
- `multiple` attribute: allow multiple files

```html
<input type="file" name="attachment" accept="image/*" multiple>
```

- need to use `POST` method and `enctype="multipart/form-data"`

### Other types

- `email`, `url`, `tel`, `search`, `color`, `date`, `datetime-local`, `time`, `month`, `week`, etc.
- provide built-in UIs and basic input validation

### Input validation

- input validation can be done natively by browser on element, or with JS
- `min`-/`max`-attribute: set limits if type is numeric or date, e.g. `number, range`
- `minlength`-/`maxlength`-attribute: set limits if type is text
- `pattern`-attribute: regular expression input needs to match, e.g. `[0-9]{3}-[0-9]{2}-[0-9]{3}`
- `step`-attribute: set step size if type is numeric, e.g. `number, range`
- boolean attributes: `required`, `readonly`, `disabled`, `multiple`
- browser input validation is never foolproof, JS can be used to toggle submit, custom browser can send any data to server whatsoever, input must always be checked by the server as well ❗️



## `<datalist>`

- text field input with predefined options via dropdown
- still handled differently by browsers

```html
<label for="myid">Select or write sauce</label><br>
<input id="myid" list="mylist" name="sauce">
<datalist id="mylist">
  <option value="ketchup"/>
  <option value="mayo"/>
  <option value="mustard"/>
</datalist>
```



## `<output>`

- shows result of a calculation

```html
<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">
  <input type="range" id="a" value="50">
  <input type="number" id="b" value="50">
  <output name="x" for="a b"></output>
</form>
```



## `<select>`

- dropdown input
- `<option>` elements is option that can select
- attributes: `multiple`, `size`

```html
<label for="myid">Select bun</label><br>
<select id="myid" name="bread">
  <option value="sesame">Sesame</option>
  <option value="potatoe">Potato</option>
  <option value="pretzel">Pretzel</option>
</select>
```



## `<textarea>`

- multi-line text input
- attributes: `rows`, `cols`, better with CSS style

```html
<label for="myid">Write comment</label><br>
<textarea id="myid" name="comment" rows="3" cols="40">Default value</textarea>
```



## `<button>`

- clickable button
- attributes: `type`, `onclick`
- `type` attribute: `submit` (default value) submits form to action URL, `reset` resets form to default data, `button` does nothing, i.e. use `button` for custom JavaScript effects

```html
<button type="button" onclick="alert('Hello World!')">Click Me!</button>
```

- often uses JS to send data asynchronously in background, instead of page reload, i.e. use `button` type