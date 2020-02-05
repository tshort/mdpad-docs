---
title: Rendering Output
---

Many different JavaScript libraries can be used to render input or output content. 

<div id="input"></div>

## Results

Here is an example of some output we can create:

<div id="mdpad"></div>


<script src="/js/mdpad-0.1.0.min.js"></script>

<!-- <script crossorigin src="https://unpkg.com/mithril/mithril.js"></script> -->
<script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/mithril/2.0.4/mithril.min.js"></script>
<script src="https://code.jquery.com/jquery-3.4.1.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js" integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy" crossorigin="anonymous"></script>
<script src="https://cdn.plot.ly/plotly-1.52.2.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vega@5.9.1"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-lite@4.1.1"></script>
<script src="https://cdn.jsdelivr.net/npm/vega-embed@6.2.2"></script>

<script>


// function panelsection(title, idout) {
//   var idref = "ref" + Math.random().toString().substring(2,15);
//   return m(".panel.panel-default",
//             m(".panel-heading",
//               m(".panel-title",
//                 m("a.accordion-toggle", {"data-toggle":"collapse" href:"#" + idref}, title,))),
//             m(".panel-collapse.collapse.in#" + idref,
//               m(".panel-body",
//                 m("pre.jsplain#" + idout,))));
// }

function card(title, content, doshow) {
  var idref = "ref" + Math.random().toString().substring(2,15);
  return m(".card",
            m(".card-header",
              m(".mb-0",
                m("button.btn.btn-link", {"data-toggle":"collapse", "data-target":"#" + idref}, title, ))),
            m(".collapse" + (doshow ? ".show" : "") + "#" + idref,
              m(".card-body", content, )));
}

var vegaspec = {
  "$schema": "https://vega.github.io/schema/vega-lite/v4.json",
  "description": "A scatterplot showing horsepower and miles per gallons.",
  "data": {"url": "https://vega.github.io/vega-lite/examples/data/cars.json"},
  "mark": "point",
  "encoding": {
    "x": {"field": "Horsepower", "type": "quantitative"},
    "y": {"field": "Miles_per_Gallon", "type": "quantitative"},
    "color": {"field": "Origin", "type": "nominal"},
    "shape": {"field": "Origin", "type": "nominal"}
  }
}

function mdpad_init() {
    m.render(document.getElementById("input"), 
      m(".form-group",
        m("label.col-sm-7.col-form-label", "Number input"),
        m(".col-sm-4", 
          m("input.form-control", {mdpad:"A", type:"number", value:10}))));
}

function make_table(tbl) {
    var keys = Object.getOwnPropertyNames(tbl);
    var idx = Array(keys.length).fill()
    var rows = []
    for (i = 0; i < tbl[keys[1]].length; i++) {
        var cells = []
        for (j = 0; j < keys.length; j++) {
            cells.push(m("td", tbl[keys[j]][i]))
        }
        rows.push(m("tr", cells))
    }
    return m("table.table.table-striped",
             m("tr", keys.map((x) => m("th", x))),
             rows)
}

var tbl = {
    a: [10, 20, 50, 99],
    b: ["apple", "apple", "banana", "apple"],
    c: [1.3, 99, -3.3, 0.0]}

function mdpad_update() {
    m.render(document.getElementById("mdpad"), 
      m("", 
        card("HTML text", m("span", "hello ", m("b", "world")), true),
        card("Plain text", "hello world"),
        card("Vega scatter plot", m("#vis")),
        card("Plotly-js histogram", m("#hist")),
        card("Table", make_table(tbl)),
        ));
    vegaEmbed('#vis', vegaspec);
    var x = [];
    for (var i = 0; i < 500; i ++) {
    	x[i] = Math.random();
    }
    Plotly.newPlot('hist', [{x: x, type: "histogram"}]);
}

</script>

