---
title: 'mdpad'
date: 2020-02-05
type: 'front'
---

### One-file web apps

*mdpad* is a JavaScript package that makes it easier to write one-file, 
one-page web apps in Markdown. It works well with static-site 
generators like Jekyll or Hugo. Make an app in one Markdown file with 
a touch of HTML and simple JavaScript.

Key features include:

* **Auto updates**--The page recalculates when the user updates
  any of the form elements.

* **Variables from form inputs**--When a page recalculates, it 
  collects form inputs that are available to the page author.

* **Variables update the URL**--When a user changes any input,
  the URL updates to reflect the change. This gives the user
  automatic undo and redo using the forward and back buttons. 
  Saving page state is as simple as bookmarking.

**[examples](examples)**

**[code for the examples](https://github.com/tshort/mdpad-docs/tree/master/content/examples)**

Edit live Markdown files with JSBin:

* [Simple example](https://jsbin.com/payoruq/edit?html,output)

* [More complex example](https://jsbin.com/goxoxay/edit?html,output)

This mode of operation drastically simplifies the interface for 
the page author. It does not depend on a particular JavaScript 
library. The author can accomplish a lot without having deep 
knowledge of JavaScript, NPM, or any of the deeper aspects of
modern JavaScript development.

This page is an *mdpad* page. Let's find the area of an ellipse
given the following two axes:

<form class="form">
	<div class="form-group"><label class="col-sm-7 col-form-label">Length of axis A</label>
		<div class="col-sm-4"><input class="form-control" min="0" step="1" type="number" value="2" mdpad="A" /></div>
	</div>
	<div class="form-group"><label class="col-sm-6 col-form-label">Length of axis B</label>
		<div class="col-sm-4"><input class="form-control" min="0" step="1" type="number" value="1" mdpad="B" /></div>
	</div>
</form>
</br>
<div id="output"></div> 
</br>

As you change these inputs, you'll see the URL in the address bar change
with the input values. The JavaScript part of this page is:

```html
<script src="/js/mdpad.min.js"></script>

<script>

function mdpad_update() {
    var area = mdpad.A * mdpad.B * Math.PI;
    document.getElementById("output").innerHTML = 
        "The area of this ellipse = " + area;
}

</script>
```

In this case, the form elements were hard coded with HTML. 
The HTML also included a `<div>` element with `id="output"` to
hold the results of the calculation.
The input elements are defined with the attribute `mdpad` set to the name
of the variable of interest. So, the first is defined as:

```html
<input class="form-control" min="0" step="1" type="number" value="2" mdpad="A" />`
```

That `mdpad` attribute makes the variable `mdpad.A` available in the
`mdpad_update()` function.

This example is relatively simple. For more sophisticated inputs and
outputs, there are better ways to render content. See the [examples](examples) 
for different approaches.

The page author provides two JavaScript functions:
  * `mdpad_init()` -- This runs right after the page loads. This is 
    normally used for setting up input items or loading data.
  * `mdpad_update()` -- This runs whenever the user changes any form 
    elements. This is normally used to make calculations based on the
    inputs and update any output fields.

Both of these functions are optional. Because *mdpad* doesn't do much
without these defined, it is safe to include site-wide as part of a
theme.

With a fast static-site generator like Hugo, the web page will
update almost immediately after you save a page.
Modern text editors will display the JavaScript portion of the Markdown
file with nice syntax highlighting to make code editing nice. 
While *mdpad* is touted as a "one-file" approach, you can of course 
keep your JavaScript in a separate file and load it with a script tag.

Note that *mdpad* is targeted at Markdown files, but it can be used with HTML,
AsciiDoc, or other content formats.

### Installation
 
Copy `mdpad.min.js` to the JavaScript folder for your static-site generator.
This is `root/static/js` for Hugo.

You can also directly use a CDN version of *mdpad* as in the following:

```html
<script src="https://cdn.jsdelivr.net/npm/mdpad-js@0.2.0/dist/mdpad.min.js" crossorigin="anonymous"></script>  
```

### Related work
 
* [mdpad (original)](http://tshort.github.io/mdpad/) -- A more complex, 
  clunkier version of one-file apps (but quite usable).

* [Iodide](https://alpha.iodide.io/) -- Literate scientific computing and 
  communication for the web.




<script src="/js/mdpad.min.js"></script>

<script>

function mdpad_update() {
    var area = mdpad.A * mdpad.B * Math.PI;
    document.getElementById("output").innerHTML = 
        "The area of this ellipse = " + area;
}

</script>

