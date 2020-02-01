---
title: Arc Flash in a Padmounted Switch
linkTitle: Arc flash
---


This app models the arc flash incident energy based on tests of PMH
padmounted switches by EPRI and PG&E. For more information, see
EPRI 1022697 [2011] and Short and Eblen [2012].

<div id="mdpad"></div>

## Notes

The incident energy in this equipment is higher than predicted by
IEEE 1584 because of horizontal busbars.
Magnetic fields from the arc current push the arcs and arc energy out
of the enclosure towards the worker. This equipment is also unusual in
that the incident energy is not linear with duration--the heat rate
increases with increasing duration. Here is a picture of the
horizontal busbars along with a video frame from an arc flash test:

<div class="row">
  <div class="col-md-4">
    <img class="img-responsive" src="padmount2.png">
  </div>
  <div class="col-md-4">
    <img class="img-responsive" src="padmount1.png">
  </div>
</div>

## References

[EPRI 1022697](http://www.epri.com/abstracts/Pages/ProductAbstract.aspx?ProductId=000000000001022697),
*Distribution Arc Flash: Phase II Test Results and Analysis*, Electric
Power Research Institute, Palo Alto, CA, 2011.

Short, T. A. and Eblen, M. L., "Medium-Voltage Arc Flash in Open Air
and Padmounted Equipment," *IEEE Transactions on Industry Applications*,
vol. 48, no. 1, pp. 245-253, Jan.-Feb. 2012.
[Approved version](//distributionhandbook.com/papers/ieee_repc_arc_flash_tshort_meblen_2011_IAS_submission.pdf)


<script src="/js/mdpad.js"></script>
<script src="//unpkg.com/mithril/mithril.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/numeric/1.2.6/numeric.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/underscore.js/1.9.1/underscore-min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/highcharts/3.0.2/highcharts.js"></script>

<script>

function binput({title = "", mdpad = "", type = "number", step = 1, min = 0, value = 10}={}) {
    return m(".form-group",
             m("label.control-label.col-sm-12", title),
             m(".col-sm-12", 
               m("input.form-control", {mdpad:mdpad, type:type, step:step, min:min, value:value})))
}

function bselect({title, mdpad, options, selected}={}) {
    var options = options.map(x => m("option", (x == selected) ? {selected: "selected"} : {}, x));
    return m(".form-group",
             m("label.control-label.col-sm-12", title),
             m(".col-sm-12", 
               m("select.form-control", {mdpad:mdpad}, options) ))
}

function mdpad_init() {
    var layout =
      m(".row",
        m(".col-md-3",
          m("br"),
          m("br"),
          m("form.form",
            binput({ title:"Working distance, in", mdpad:"D", step:5, value:36 }),
            binput({ title:m("span", "Clothing rating, cal/cm", m("sup", 2)), mdpad:"clothing", value:8 }),
            binput({ title:"Bolted current, kA", mdpad:"I", value:6 }),
            binput({ title:"Duration, sec", mdpad:"t", step:0.1, value:1.0 }),
            binput({ title:"Safety multiplier", mdpad:"k", step:0.1, value:1.15 }),
            bselect({ title:"Plotting extras", mdpad:"graphextras", options: ["Vary working distance", "Vary clothing", "None",] }),
            )),
        m(".col-md-1"),
        m(".col-md-8",
          m("h3", "Results"),
          m("#results"),
          m("#plot")))
    m.render(document.querySelector("#mdpad"), layout);
}

var pow = Math.pow

findcals = function(I, t, d, k) {
    return k * 3547 * pow(I, 1.5) * pow(t, 1.35) / pow(d, 2.1);
}

findduration = function(E, I, d, k) {
    return pow(E * pow(d, 2.1) / (k * 3547 * pow(I, 1.5)), 1/1.35);
}

var plotinfo = { chart: { type: "line", width: 500, height: 500, spacingRight: 20 },
  title: { text: "Time-current curve" },
  plotOptions: { series: { marker: { enabled: false } } },
  xAxis: 
   { type: "logarithmic",
     min: 1,
     max: 100,
     endOnTick: true,
     tickInterval: 1,
     minorTickInterval: 0.1,
     gridLineWidth: 1,
     title: { text: "Current, kA" } },
  yAxis: 
   { type: "logarithmic",
     min: 0.02,
     max: 10,
     tickInterval: 1,
     minorTickInterval: 0.1,
     title: { text: "Time, sec" } },
  legend: 
   { align: "right",
     verticalAlign: "middle",
     layout: "vertical",
     borderWidth: 0 } }


function mdpad_update() {
    var {I, t, D, k, clothing, graphextras} = mdpad
    cals = findcals(I, t, D, k)
    duration = findduration(clothing, I, D, k)
    
    m.render(document.querySelector("#results"), 
        m("div", "Incident energy for the given current and duration = ",
          m("b", cals.toFixed(1), " cal/cm", m("sup", 2)), m("br"),
          "Duration limit for the given current and clothing = ",
          m("b", duration.toFixed(2) + " secs")))
    currents = numeric.pow(10,numeric.linspace(0,2,40))
    durations1 = _.map(currents, function(I) {return findduration(clothing, I, D, k)})
    series1 = _.zip(currents,durations1)
    if (graphextras == "Vary clothing") {
        durations0 = _.map(currents, function(I) {return findduration(clothing * 2, I, D, k)})
        series0 = _.zip(currents,durations0)
        durations2 = _.map(currents, function(I) {return findduration(clothing / 2, I, D, k)})
        series2 = _.zip(currents,durations2)
        plotinfo.series = [{name: clothing*2 + " cals at " + D + "\"", data: series0},
                           {name: clothing   + " cals at " + D + "\"", data: series1},
                           {name: clothing/2 + " cals at " + D + "\"", data: series2}]
    } else if (graphextras == "Vary working distance") {
        durations0 = _.map(currents, function(I) {return findduration(clothing, I, D * 2, k)})
        series0 = _.zip(currents,durations0)
        durations2 = _.map(currents, function(I) {return findduration(clothing, I, D / 2, k)})
        series2 = _.zip(currents,durations2)
        plotinfo.series = [{name: clothing + " cals at " + D*2 + "\"", data: series0},
                           {name: clothing + " cals at " + D + "\"", data: series1},
                           {name: clothing + " cals at " + D/2 + "\"", data: series2}]
    } else {
        plotinfo.series = [{name: clothing + " cals at " + D + "\"", data: series1}]
    }
    $("#plot").highcharts(plotinfo)

}

</script>

