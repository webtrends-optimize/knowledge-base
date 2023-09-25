# Ingesting datalayers

There are many reasons you'd want to ingest datalayers into Optimize. Whether GTM, Bloomreach or others - ingesting data into Optimize allows you to target and segment based on it.

There are two parts to this: 

1. Delay the execution of Optimize until the datalayer is ready
2. Ingest the data required.

Let's explore both of these.

## Delaying the execution of Optimize

In the post-load script of your tag, you'll see WT.optimize.setup - this is the "go" method.

Instead of the default code:

``` javascript
WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
```

You can choose to check for some elements before running it. E.g.

``` javascript
var poll_for_datalayer = setInterval(function(){
    
    // Your condition here

    // E.g. "wait for the optimize.activate event in GTM"
    if(!window.dataLayer || !dataLayer.length || !JSON.stringify(dataLayer).match(/optimize\.activate/i)) return;

    // E.g. "wait for bloomreach's datalayer"
    var bloomreach = window.bre && bre.segments && bre.segments.cdp_segments;
    if(!bloomreach) return;


    // Happy to run Optimize 
    WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);

    poll_for_datalayer = false;

}, 100);

// Safety redisplay
setTimeout(function(){
    if(poll_for_datalayer !== false){
        clearInterval(poll_for_datalayer);
        poll_for_datalayer = false;

        // Run Optimize 
        WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
    }
}, 2000);
```

## Ingesting the required data 

Prior to triggering `WT.optimize.setup`, you can add additional data into our datalayer - `WT.optimizeModule.prototype.wtConfigObj.data`. 

Instead of the default code:

``` javascript
WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
```

You can add to our datalayer. 

``` javascript
var dataObj = WT.optimizeModule.prototype.wtConfigObj.data;

// E.g. take all bloomreach data
var bloomreach = window.bre && bre.segments && bre.segments.cdp_segments;
for(var key in bloomreach){
    dataObj[key] = bloomreach[key];
}

// E.g. take all data from a specific GTM event 
for(var i=0; i<dataLayer.length; i++){
    if(dataLayer[i].event === "my-event"){
        for(var key in dataLayer[i]){
            dataObj[key] = dataLayer[i][key];
        }
        break;
    }
}

WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
```

## Putting it all together

### GTM Example

``` javascript
var poll_for_datalayer = setInterval(function(){
    
    // Wait for my-event in in GTM
    if(!window.dataLayer || !dataLayer.length || !JSON.stringify(dataLayer).match(/my-event/i)) return;

    var dataObj = WT.optimizeModule.prototype.wtConfigObj.data;
    
    // Take all data from a specific GTM event 
    for(var i=0; i<dataLayer.length; i++){
        if(dataLayer[i].event === "my-event"){
            for(var key in dataLayer[i]){
                dataObj[key] = dataLayer[i][key];
            }
            break;
        }
    }

    // Happy to run Optimize 
    WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);

    poll_for_datalayer = false;

}, 100);

// Safety redisplay
setTimeout(function(){
    if(poll_for_datalayer !== false){
        clearInterval(poll_for_datalayer);
        poll_for_datalayer = false;

        // Run Optimize 
        WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
    }
}, 2000);
```

### Bloomreach example

``` javascript
var poll_for_datalayer = setInterval(function(){

    // Wait for bloomreach datalayer
    var bloomreach = window.bre && bre.segments && bre.segments.cdp_segments;
    if(!bloomreach) return;

    var dataObj = WT.optimizeModule.prototype.wtConfigObj.data;
    
    // Take all data from a specific GTM event 
    for(var key in bloomreach){
        dataObj[key] = bloomreach[key];
    }

    // Happy to run Optimize 
    WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);

    poll_for_datalayer = false;

}, 100);

// Safety redisplay
setTimeout(function(){
    if(poll_for_datalayer !== false){
        clearInterval(poll_for_datalayer);
        poll_for_datalayer = false;

        // Run Optimize 
        WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
    }
}, 2000);
```