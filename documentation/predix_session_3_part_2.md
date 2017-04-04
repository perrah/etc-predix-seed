# Predix Session 3 - part 2

what you'll cover in this guide:

 * time series
 * auto update



#### 10. Insert Time Series
firstly import
```
<link rel="import" href="../../bower_components/px-vis-timeseries/px-vis-timeseries.html">
```
insert this into your main body of your view (its very hard to read but thats ok - check out predix-ui.com to see how this works, the UI page has a great tool for building the time series out.)
```
<px-vis-timeseries prevent-resize="true" debounce-resize-timing="250" width="700" height="450"
        progressive-rendering-points-per-frame="16000" progressive-rendering-min-frames="1"
        chart-horizontal-alignment="center" chart-vertical-alignment="center" margin='{"top":30,"bottom":60,"left":65,"right":65}'
        tooltip-config='{}' register-config='{"type":"vertical","width":200}' selection-type="xy"
        chart-data='{{timeSeriesData}}'
        series-config='{"y0":{"name":"y0","x":"x","y":"y0","yAxisUnit":"C","axis":{"id":"axis1","side":"left","number":"1"}}}'
        chart-extents='{"x":["dynamic","dynamic"],"y":[20,40]}'
        threshold-data='[{"for":"y0","type":"max","value":32},{"for":"y0","type":"min","value":26},{"for":"y0","type":"mean","value":29}]'
        display-threshold-title="true"
        threshold-config='{"max":{"color":"red","dashPattern":"5,0","title":"MAX","showThresholdBox":true,"displayTitle":true}}'
        x-axis-config='{"title":"Date"}'
        y-axis-config='{"title":"Temperature","titleTruncation":false,"unit":"C"}'
        dynamic-menu-config='[{"name":"Delete","action":"function(data) {var conf = this.seriesConfig;delete conf[data.additionalDetail.name];this.set(\"seriesConfig\", {}); this.set(\"seriesConfig\", conf);}","eventName":"delete","icon":"fa-trash"},{"name":"Bring To Front","action":"function(data) {this.set(\"serieToRedrawOnTop\", data.additionalDetail.name);}","eventName":"bring-to-front","icon":"fa-arrow-up"}]'
        toolbar-config='{"config":{"advancedZoom":true,"pan":true,"tooltip":true,"logHover":{"buttonGroup":2,"tooltipLabel":"The submenu item of this menu will define custom mouse interaction","icon":"fa-leaf","subConfig":{"customClick":{"icon":"fa-coffee","buttonGroup":3,"tooltipLabel":"define some custom mouse interactions on chart","eventName":"my-custom-click","actionConfig":{"mousedown":"function(mousePos) { console.log(\"custom click on chart. Context is the chart. Mouse pos is available: \" + JSON.stringify(mousePos))}","mouseup":"function(mousePos) { console.log(\"custom action on mouse up the chart \" + JSON.stringify(mousePos));}","mouseout":"function(mousePos) { console.log(\"custom action on mouse out the chart \" + JSON.stringify(mousePos));}","mousemove":"function(mousePos) { console.log(\"custom action on hovering the chart \");}"}},"customClick2":{"buttonGroup":3,"icon":"fa-fire-extinguisher","tooltipLabel":"Remove all custom interactions","actionConfig":{"mousedown":null,"mouseup":null,"mouseout":null,"mousemove":null}}}}}}' navigator-config='{"xAxisConfig":{"tickFormat":"%b %d"}}'>
      </px-vis-timeseries>
```

#### 2. Data Bind & Transform
create a new propert (remember those commas!)
```
timeSeriesData : Object
```
add the following in to your _handleTempData function
```
this.timeSeriesData = data.map(function (item) {
	return {"x" : item[0], "y0" : item[1]}
});
```


#### 2. Data Bind & Transform
create a new function (remember comma's!)
```
_updateData: function() {
        this.async(function() {
          this.$.getTempData.generateRequest();
        }, 300000);
      }
```
invoke your new function at the end of _handleTempData
```
this._updateData();
```
