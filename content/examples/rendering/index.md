---
title: Rendering Content
---

Many different JavaScript libraries can be used to render input or output content. 
Here is an example of using several common libraries to create input elements:

#### Mithril
<div id="mithrilx"></div>
<br/>

#### Preact
<div id="preactx"></div>
<br/>

#### React
<div id="reactx"></div>
<br/>

#### Vue
<div id="vuex"></div>
<br/>

<br/>

Here's the code used to create the form above:

```js
    function section(h, str) {
        return h("div", {}, ["Input from " + str + ":", 
          h("br"),
          h("input", {type: "numeric", mdpad: "num", value: 3}),
          h("input", {type: "checkbox", mdpad: str + "chk"}),
          h("br")]);
    }
    // Mithril
    m.render(document.getElementById("mithrilx"), section(m, "mithril"));
    // Preact
    preact.render(section(preact.h, "preact"), document.getElementById("preactx"));
    // React
    ReactDOM.render(section(React.createElement, "react"), document.getElementById("reactx"));
    // Vue does it a little differently
    function vuesection(h, str) {
        return h("div", {}, ["Input from " + str + ":", 
          h("br"),
          h("input", {attrs: {type: "numeric", mdpad: "num", value: 3}}),
          h("input", {attrs: {type: "checkbox", mdpad: str + "chk"}}),
          h("br")]);
    }
    new Vue({ render: h => vuesection(h, "vue") }).$mount("#vuex");
```

The `render` methods of common libraries have similar forms.

## More complicated example using Mithril 

<div id="mithrilx2"></div>

With [Mithril](https://mithril.js.org/) or any Hyperscript-type approach, it's 
nice to be able to define helper functions that make it easier to define content. 
Here's the code used to create the form above:

```js
    function binput({title = "", mdpad = "", type = "number", step = 1, min = 0, value = 10}={}) {
        return m(".form-group.row",
                 m("label.col-sm-7.col-form-label", title),
                 m(".col-sm-4", 
                   m("input.form-control", {mdpad:mdpad, type:type, step:step, min:min, value:value})))
    }
     
    var form = 
        m(".col-md-6",
          m("form.form-horizontal",
            binput({ title:"Fault current, A", mdpad:"flti", step:1000, value:2500 }),
            binput({ title:"Fault duration, cycles (60 Hz)", mdpad:"fltt", step:5, value:20 }),
            binput({ title:"Span length, feet", mdpad:"h0", step:50, value:250 }),
            binput({ title:"Conductor sag, feet", mdpad:"y0", value:5 }),
          ));
    m.render(document.getElementById("mithrilx2"), form);
```

## Emblem

Here's an example using [Emblem](https://emblemjs.com/), a compact syntax like Slim or HAML.
This form is compact and easy to read, but it adds heavier dependencies and is slower.

<div id="emblemx"></div>

Here's the code to create that form:

```js
    var emblem_content = `
        form.form
          .row
            .col-md-4
              .form-group
                label.control-label Sub ground resistance, ohms
                input.form-control#Rsub mdpad="Rsub" type="number" step="0.1" min="0" value="0.5"
            .col-md-4
              .form-group
                label.control-label Earth resistivity, ohms
                input.form-control#rho mdpad="rho" type="number" step="50" min="0" value="100"
            .col-md-4
              .form-group
                label.control-label Manhole ground resistance, ohms
                input.form-control#Rgrnd mdpad="Rgrnd" type="number" step="5" min="0" value="10"
          .row
            .col-md-4
              .form-group
                label.control-label Duct spacing, in
                input.form-control#ductSpacing mdpad="ductSpacing" type="number" step="1" min="0" value="7"
            .col-md-4
              .form-check
                  input.form-check-input#jumpershield mdpad="jumpershield" type="checkbox" checked=false
                  label.form-check-label Jumper shield at work site
              .form-check
                  input.form-check-input#jumperphase mdpad="jumperphase" type="checkbox" checked=false
                  label.form-check-label Connect phases together
            `;
    document.getElementById("emblemx").innerHTML = Emblem.compile(Handlebars, emblem_content)(window);
```

<script src="/js/mdpad.js"></script>
<script crossorigin src="https://unpkg.com/mithril/mithril.js"></script>
<script crossorigin src="https://unpkg.com/react@16/umd/react.production.min.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.production.min.js"></script>
<script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/preact/8.5.2/preact.min.js"></script>
<script crossorigin src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
<script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/handlebars.js/2.0.0/handlebars.min.js"></script>
<script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/emblem/0.4.0/emblem.min.js"></script>

<script>

function mdpad_init() {
    function section(h, str) {
        return h("div", {}, ["Input from " + str + ":", 
          h("br"),
          h("input", {type: "numeric", mdpad: "num", value: 3}),
          h("input", {type: "checkbox", mdpad: str + "chk"}),
          h("br")]);
    }
    // Mithril
    m.render(document.getElementById("mithrilx"), section(m, "mithril"));
    // Preact
    preact.render(section(preact.h, "preact"), document.getElementById("preactx"));
    // React
    ReactDOM.render(section(React.createElement, "react"), document.getElementById("reactx"));
    // Vue does it a little differently
    function vuesection(h, str) {
        return h("div", {}, ["Input from " + str + ":", 
          h("br"),
          h("input", {attrs: {type: "numeric", mdpad: "num", value: 3}}),
          h("input", {attrs: {type: "checkbox", mdpad: str + "chk"}}),
          h("br")]);
    }
    new Vue({ render: h => vuesection(h, "vue") }).$mount("#vuex");

    // More detailed Mithril example

    function binput({title = "", mdpad = "", type = "number", step = 1, min = 0, value = 10}={}) {
        return m(".form-group.row",
                 m("label.col-sm-7.col-form-label", title),
                 m(".col-sm-4", 
                   m("input.form-control", {mdpad:mdpad, type:type, step:step, min:min, value:value})))
    }
     
    var form = 
        m(".col-md-6",
          m("form.form-horizontal",
            binput({ title:"Fault current, A", mdpad:"flti", step:1000, value:2500 }),
            binput({ title:"Fault duration, cycles (60 Hz)", mdpad:"fltt", step:5, value:20 }),
            binput({ title:"Span length, feet", mdpad:"h0", step:50, value:250 }),
            binput({ title:"Conductor sag, feet", mdpad:"y0", value:5 }),
          ));
    m.render(document.getElementById("mithrilx2"), form);

    // Emblem

    var emblem_content = `
        form.form
          .row
            .col-md-4
              .form-group
                label.control-label Sub ground resistance, ohms
                input.form-control#Rsub mdpad="Rsub" type="number" step="0.1" min="0" value="0.5"
            .col-md-4
              .form-group
                label.control-label Earth resistivity, ohms
                input.form-control#rho mdpad="rho" type="number" step="50" min="0" value="100"
            .col-md-4
              .form-group
                label.control-label Manhole ground resistance, ohms
                input.form-control#Rgrnd mdpad="Rgrnd" type="number" step="5" min="0" value="10"
          .row
            .col-md-4
              .form-group
                label.control-label Duct spacing, in
                input.form-control#ductSpacing mdpad="ductSpacing" type="number" step="1" min="0" value="7"
            .col-md-4
              .form-check
                  input.form-check-input#jumpershield mdpad="jumpershield" type="checkbox" checked=false
                  label.form-check-label Jumper shield at work site
              .form-check
                  input.form-check-input#jumperphase mdpad="jumperphase" type="checkbox" checked=false
                  label.form-check-label Connect phases together
            `;
    document.getElementById("emblemx").innerHTML = Emblem.compile(Handlebars, emblem_content)(window);

}

// define this just to watch the URL change
function mdpad_update() {
}

</script>

