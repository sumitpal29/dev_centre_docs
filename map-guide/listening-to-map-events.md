---
permalink: map-guide/listening-to-map-events.html
title: Listening to Map Events | FusionCharts
description: If you need to extend this further, you can tap into the JavaScript events raised by each map, and then build your custom behavior over it.
heading: Listening to Map Events
chartPresent: true
---

FusionCharts Suite XT lets you configure standard interactivity for items like markers, tooltips and legend through the map attributes itself. However, if you need to extend this further, you can tap into the JavaScript events raised by each map, and then build your custom behavior over it. You can listen to events related to `entities` , `markers` and `connectors` which are specific to maps.

These events let to tightly integrate maps with your own applications and add interactivity to your visualizations.

## Entity Events

Entities in FusionCharts Suite XT raise 3 events  `entityRollOver` , `entityRollOut` and  `entityClicked`.

Shown below is a map that captures data from the entity events and displays it in a message box below the map. Hover on individual continents to see the population of only that specific continent.

{% embed_chart {"source": "map-guide-listening-to-map-events-1.js", "id": "1"} %}

The data used to create this example is shown here

{% embed_data {"source": "map-guide-listening-to-map-events-1.js"} %}

This is what we did in the above data structure

* Created a World Population map exactly like we did as part of {% linkTo tutorials/map-guide/customizing-your-map.md %} and set its theme to `fint`.

* Defined event listeners in an `events` object as part of  the `FusionCharts()` constructor to listen to 3 events. This is the quickest way to define event listeners for a map. Alternatively, you can use the `addEventListener()` method on specific map instances, or on all maps globally, to listen to events.

* Each event generated by FusionCharts Suite XT has a string event-alias for ease of usage. The 3 events that we listen to, are:

    * `entitytRollOver` : Fired when the mouse moves into the entity of a map.

    * `entityRollOut` : Fired when the mouse moves out of the entity of a map.

    * `entityClick`: Fired when the mouse clicks on an entity.

* To each event listener method, two standard argument objects - `eventObject` and `argumentsObject` are provided, when the event is invoked. They are referenced locally as `evt` and `data` in the above example.

* Within these methods, we have used JavaScript functions to hide and show the text based on user mouse interactions in the `message` textarea. We get the label of rolled-over entity using `data.label` and the population using `data.value`.

* The message is reset with placeholder text showing the total population when the roll out event is triggered. When the click event triggers an alert is configured to display with information on which entity was clicked.

The code for the example is shown here

{% highlight html lineanchors %}{% raw %}
<html>
<head>
    <title>A Data Driven Map</title>
    <script type="text/javascript" src="fusioncharts/fusioncharts.js"></script>
    <script type="text/javascript" src="fusioncharts/themes/fusioncharts.theme.fint.js"></script>
<script>
FusionCharts.ready(function() {
    var populationMap = new FusionCharts({
        type: 'maps/world',
        renderAt: 'chart-container',
        width: '600',
        height: '400',
        dataFormat: 'json',
        dataSource: {
            "chart": {
                "caption": "Global Population",
                "theme": "fint",
                "formatNumberScale": "0",
                "numberSuffix": "M",
                "showLabels": "1",
                "showToolTip": "0"
            },
            "colorrange": {
                "color": [{
                    "minvalue": "0",
                    "maxvalue": "100",
                    "code": "#D0DFA3",
                    "displayValue": "< 100M"
                }, {
                    "minvalue": "100",
                    "maxvalue": "500",
                    "code": "#B0BF92",
                    "displayValue": "100-500M"
                }, {
                    "minvalue": "500",
                    "maxvalue": "1000",
                    "code": "#91AF64",
                    "displayValue": "500M-1B"
                }, {
                    "minvalue": "1000",
                    "maxvalue": "5000",
                    "code": "#A9FF8D",
                    "displayValue": "> 1B"
                }]
            },
            "data": [{
                "id": "NA",
                "value": "515"
            }, {
                "id": "SA",
                "value": "373"
            }, {
                "id": "AS",
                "value": "3875"
            }, {
                "id": "EU",
                "value": "727"
            }, {
                "id": "AF",
                "value": "885"
            }, {
                "id": "AU",
                "value": "32"
            }],
        },
        "events": {
            "entityRollover": function(evt, data) {
                document.getElementById('message').value = "" + data.label + "\n" + "Population: " + data.value + "M";
            },
            "entityRollout": function(evt, data) {
                document.getElementById('message').value =
                    "Total World Population - 6.3 Billion";
            },
            "entityClick": function(evt, data) {
                alert("You have clicked on " + data.label + ".");
            },
        }
    }).render();
});
</script>
</head>
<body>
    <div id="chart-container">A world map will load here!</div>
    <textarea id="message" rows="4" cols="54" style='margin-left:10px;text-align:center'>"Total World Population 6.3 Billion" </textarea>
</body>
</html>
{% endraw %}{% endhighlight %}

## Marker and Connector Events

Markers and Connectors raise events on mouse interactions like roll over, roll out and click just like Entities.

Shown here is the world map that we built in {% linkTo tutorials/map-guide/adding-markers.md %} as part of the connectors section. It shows the busiest routes from the Heathrow.

{% embed_chart {"source": "map-guide-listening-to-map-events-2.js", "id": "2"} %}

The data used to create this example is shown here.

{% embed_data {"source": "map-guide-listening-to-map-events-2.js"} %}

This is what we did in the above data structure

* Created an Airport Traffic map and set its theme to `fint`.

* Defined event listeners in an `events` object as part of  the `FusionCharts()` constructor to listen to 3 events each for connectors and markers. Alternatively, you can use the `addEventListener()` method on specific map instances.

* The events generated by FusionCharts Suite XT have a string event-alias for ease of usage. The  events that we listen to, are:

    * `markerRollOver` and `connectorRollOver` : Fired when the mouse moves into the marker/connector in a map.

    * `markerRollOut` and `connectorRollOut`: Fired when the mouse moves out of the marker/connector in a map.

    * `markerClick` and `connectorClick`: Fired when the mouse clicks on the marker/connector.

* To each event listener method, two standard argument objects - `eventObject` and `argumentsObject` are provided, when the event is invoked. They are referenced locally as `evt` and `data` in the above example

* Within these methods, we have used JavaScript functions to hide and show the text based on user mouse interactions in the `message` textarea.

* We get the label of rolled-over marker or connector using `data.label`. The message is reset with placeholder text when the roll out event is triggered. The click event when triggered creates an alert with details on what was clicked.

The full HTML code for this sample is shown here

{% highlight html lineanchors %}{% raw %}
<html>
<head>
    <title>A Data Driven Map</title>
    <script type="text/javascript" src="fusioncharts/fusioncharts.js"></script>
    <script type="text/javascript" src="fusioncharts/themes/fusioncharts.theme.fint.js"></script>
<script>
FusionCharts.ready(function() {
    var routesMap = new FusionCharts({
        type: 'maps/world',
        renderAt: 'chart-container',
        width: '600',
        height: '400',
        dataFormat: 'json',
        dataSource: {
            "chart": {
                "caption": "Busiest Routes from Heathrow Airport",
                "subcaption": "For the year 2014",
                "theme": "fint",
                "markerBgColor": "#FF0000",
                "markerRadius": "10",
                "connectorColor": "#0CB2B0",
                "connectorHoverColor": "#339933",
                "entityFillColor": "#CECED2",
                "entityFillHoverColor": "#E5E5E9"
            },
            "markers": {
                "items": [{
                    "id": "London",
                    "shapeid": "triangle",
                    "x": "340.23",
                    "y": "125.9",
                    "label": "LHR",
                    "tooltext": "Heathrow International Airport {br}IACL Code : EGLL",
                    "labelpos": "left"
                }, {
                    "id": "New York",
                    "shapeid": "triangle",
                    "x": "178.14",
                    "y": "154.9",
                    "label": "JFK",
                    "tooltext": "John F Kennedy Airport {br}IACL Code : KJFK",
                    "labelpos": "bottom"
                }, {
                    "id": "Dubai",
                    "shapeid": "triangle",
                    "x": "458.14",
                    "y": "203.9",
                    "label": "DXB",
                    "tooltext": "Dubai International Airport {br} IACL Code : OMDB",
                    "labelpos": "bottom"
                }, {
                    "id": "Singapore",
                    "shapeid": "triangle",
                    "x": "558.14",
                    "y": "255.9",
                    "label": "SIN",
                    "tooltext": "Singapore International Airport {br} IACL Code : WSSS",
                    "labelpos": "bottom"
                }, {
                    "id": "Hong Kong",
                    "shapeid": "triangle",
                    "x": "573.14",
                    "y": "202.9",
                    "label": "HKG",
                    "tooltext": "Hong Kong International Airport {br} IACL Code : VHHH",
                    "labelpos": "bottom"
                }],
                "connectors": [{
                    "from": "London",
                    "to": "Hong Kong",
                    "tooltext": "<b>London to Hong Kong</b>{br} Total Passengers: 1,801,520",
                    "label": "LHR to HKK"
                }, {
                    "from": "London",
                    "to": "Singapore",
                    "tooltext": "<b>London to Singapore</b>{br} Total Passengers: 1,507,032",
                    "label": "LHR to SIN"
                }, {
                    "from": "London",
                    "to": "New York",
                    "tooltext": "<b>London to New York{br} Total Passengers: 2,551,276",
                    "label": "LHR to NYC"
                }, {
                    "from": "London",
                    "to": "Dubai",
                    "tooltext": "<b>London to Dubai</b>{br} Total Passengers: 1,974,078",
                    "label": "LHR to DXB"
                }]
            }
        },
        "events": {
            "connectorRollover": function(evt, data) {
                document.getElementById('message').value = data.label;
            },
            "connectorRollout": function(evt, data) {
                document.getElementById('message').value = "Rollover or click on a marker or connector";
            },
            "connectorClick": function(evt, data) {
                alert("You have selected the connector from " + data.label + ". \n Click on OK to continue.");
            },
            "markerRollover": function(evt, data) {
                document.getElementById('message').value = "" + data.label;
            },
            "markerRollout": function(evt, data) {
                document.getElementById('message').value = "Rollover or click on a marker or connector";
            },
            "markerClick": function(evt, data) {
                alert("You have selected " + data.label + " Airport" + ". \n Click on OK to continue.");
            },
        }
    }).render();
});
</script>
</head>
<body>
    <div id="chart-container">A world map will load here!</div>
    <textarea id="message" rows="4" cols="54" style='margin-left:10px;text-align:center'>Roll over or click on a marker or connector </textarea>
</body>
</html>
{% endraw %}{% endhighlight %}

For a list of all parameters for each of these events refer to the [API Reference for events]{% linkTo FusionCharts.events %}.