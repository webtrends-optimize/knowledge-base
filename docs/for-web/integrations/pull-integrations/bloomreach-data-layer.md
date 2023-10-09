# Integrate Webtrends Optimize with Bloomreach Data Layer - Pull Integration

This integration takes data found in the Bloomreach Data Layer, and ingests it into the Optimize data layer so that you can use it for Location targeting and Segments / Audiences. 

This code lives in post-load, and overwrites the default mechanism of when to run the Optimize platform. 

## Postload script

The script is found below. 

Bloomreach segments are populated into `bre.segments.cdp_segments`. Our code takes all values found in here, and ports them over. 

Once ingested, we also perform some calculations on the data pulled in. In this example, we take the loyalty_expiry date, and turn it into "time left in days", so that we can then target experiences based on there being 30 days left.

``` javascript
var dataObj = WT.optimizeModule.prototype.wtConfigObj.data;
try {
    var bloomreach = window.bre && bre.segments && bre.segments.cdp_segments;
    if(bloomreach){
        for(var key in bloomreach){
            dataObj[key] = bloomreach[key];
        }
    }

    if(dataObj.loyalty_expiry){
        dataObj.loyalty_timeleftdays = Math.floor( (dataObj.loyalty_expiry - (Date.now()/1000)) / 60 / 60 / 24 );
        if(dataObj.loyalty_timeleftdays < 0){
            dataObj.loyalty_timeleftdays = 0;
        }
    }

} catch(er){}

WT.optimize.setup(WT.optimizeModule.prototype.wtConfigObj);
```

## Targeting based on Bloomreach Data 

Once ingested into the platform via. post-load script, we can use Data Object Attribute matching for Locations and Segments.

Make sure you use the same names sent into Bloomreach's data layer.

## Things to note

- This script assumes the data is available by the time the Optimize tag loads. 
    
    If it isn't, you'll need to poll for the data layer before running all of this code including `WT.optimize.setup`.