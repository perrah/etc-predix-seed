# Predix Session 3 - part 1

what you'll cover in this guide:

 * iron-ajax installation & setup
 * Data Binding
 * Data Representation


#### 1. Install iron-ajax
DevBox
```
sudo bower install polymerelements/iron-ajax --save --allow-root
```
Windows
```
bower install polymerelements/iron-ajax --save
```

#### 2. Import iron-ajax
Import in your view page code
```
<link rel="import" href="../../bower_components/iron-ajax/iron-ajax.html">
```

#### 3. Insert AJAX code
insert into your view page code
```
 	<iron-ajax
      auto
      id="getTempData"
      method="POST"
      content-type="application/json"
      url="https://etc-px-api.run.aws-usw02-pr.ice.predix.io/queryDataPoints"
      body="{{queryBody}}"
      headers="{{queryHeaders}}"
      handle-as="json"
      on-response="_handleTempData"
      debounce-duration="300">
    </iron-ajax>
```

#### 4. Update AJAX query
update properties array within Polymer javascript (remember this is JSON use comma's where needed!!)
```
queryBody : {
  type: Object,
  value : {
    "start": 1490777615730,
    "tags": [{
        "name": [
            "ETC_PX_TRAINING"
        ]
    }]
  }//end value
},//end queryBody object

```
#### 5. Update AJAX headers
update properties array within Polymer javascript (remember this is JSON use comma's where needed!! use a comma after the last object, before any comments)
```
queryHeaders : {
  type: Object,
  value: {
    "x-base64-credentials" : "Basic TOKEN_HERE",
    "x-clientid" : "healthcare_etc-px-api_prod",
    "Content-Type" : "application/json",
    "cache-control" : "no-cache"
    }//end value
}//end queryHeader

```

#### 6. Create a response function
within the polymer script create a new function to handle the data response from the iron-ajax element (this is also comma seperated! check the ready function that already exists!)
```
_handleTempData : function (req) {
	var data = req.detail.response["tags"][0]["results"][0]["values"];
console.log(data);
},

```

#### 7. Bind your data
now you have some data you need to bind it to a UI component to display it. Firstly create a new property (comma seperate...!)
```
tableData : Object,
```
and add the following to your _handleTempData function
```
this.tableData = data;
```

#### 8. Present your data in UI
now your data is bound you can pass it to a UI element that has already been installed for you. firstly import at the top of your code
```
<link rel="import" href="../../bower_components/px-data-table/px-data-table.html"/>
```
and then your ready to add the following to your main code within the blank card element
```
<px-data-table
	table-data='{{tableData}}'
    language="en"striped="true"
    filterable="true" sortable="true"
    selectable="true" single-select="true"
    show-column-chooser="true"
    include-all-columns="true">
</px-data-table>
```

#### Part 2 Pre-requisites
install another fancy component
```
npm install https://github.com/PredixDev/px-vis-timeseries
bower install https://github.com/PredixDev/px-vis-timeseries
```
