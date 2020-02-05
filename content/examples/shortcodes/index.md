---
title: Shortcodes
---

Another option for form entry is to use template features of a static-
site generator.
This directly generates HTML, so forms do not need to be generated at runtime.
It also helps to decouple the look of the forms to the template used.
The main downside is that it ties the document to the particular system used.
Here is an example of using Hugo's shortcodes to generate form elements.

{{< input label="Numeric input A" min="0" value="10" step="5" mdpad="A" >}}
{{< input label="Numeric input B" min="0" value="30" mdpad="B" >}}
{{< input label="Text input C" type="text" value="hello" mdpad="C" >}}
{{< select label="Select input D" selected="Orange" mdpad="D" >}}
    {{< options "Apple" "Orange" "Banana" >}}
{{< /select >}}

The HTML to generate these inputs with shortcodes looks like:

```html
{{</* input label="Numeric input A" min="0" value="10" step="5" mdpad="A" */>}}
{{</* input label="Numeric input B" min="0" value="30" mdpad="B" */>}}
{{</* input label="Text input C" type="text" value="hello" mdpad="C" */>}}
{{</* select label="Select input D" selected="Orange" mdpad="D" */>}}
   {{</* options "Apple" "Orange" "Banana" */>}}
{{</* /select */>}}
```

The input is reasonable. Note that the `selected="Orange"` bit doesn't work.
That was beyond my Hugo shortcode-coding ability.

<script src="/js/mdpad.min.js"></script>

<script>

// define this just to watch the URL change
function mdpad_update() {
}

</script>
