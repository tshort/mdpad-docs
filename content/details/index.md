---
title: Details
weight: 20
---

## Overview

*mdpad* does the following:
  - Run `mdpad_init()` after the page loads if it exists.
  - If the function `mdpad_update` exists,
    - Add event listeners to form elements to check for changes. 
    - When a form element changes, take all form elements with a `mdpad`
      attribute, and create variables within `mdpad` with that same name. 
      Then, run `mdpad_update()`.

## Typical layout

An *mdpad* file often has some input with one or more `<div>` elements
used as placeholders. 
These placeholders will then be filled in by `mdpad_init()` or `mdpad_update()`. 
The page's JavaScript often goes at the end. 
There are normally `<script>` tags to load in `mdpad.js` or
`mdpad-0.1.0.min.js` along with other JavaScript packages. 
These can be included in the template instead.

Here is a minimal example:

```md
# Intro

Here is some input:

<div id="input"></div>

Here is some output:

<div id="output"></div>

<script url="/js/mdpad-0.1.0.min.js"></script>

<script>
function mdpad_init() {
    document.getElementById("input").innerHTML = 
        "<input type='number' value=3 mdpad='x1'></input>";
}

function mdpad_update() {
    document.getElementById("output").innerHTML = 
        "Your input = " + mdpad.x1;
}
</script>
```

## User API

*mdpad* returns an object `api` with several function used for setup. 
  * `mdpad.api.calculate()` -- run a page calculation. 
  * `mdpad.api.enable_url()` -- adjust the URL as inputs are changed. (default)
  * `mdpad.api.disable_url()` -- disable this feature.
  * `mdpad.api.enable_replace_url()` -- don't keep history of changes.
  * `mdpad.api.disable_replace_url()` -- keep history of changes. (default)

`mdpad` also houses the variables created from forms with `mdpad`-labeled inputs.

The `enable_url` and `disable_url` control whether variable changes are
updated in the URL. 
The `enable_replace_url` and `disable_replace_url` control whether the 
variable changes are included in the browser history.
The default is to include them in the browser history. 
That means that the back and forward buttons work as undo and redo.
If this is disabled, the URL is still updated, but the back button
will leave to the prior page rather than the last variable change. 

