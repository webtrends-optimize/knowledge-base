# Create your own push integration - For Users

Are you a user of Webtrends Optimize looking to send data to a 3rd party platform when people fall into your experiment? If so, this page is for you. 

## Globally, for all tests

## On a per test basis 

### Standard approach

``` javascript
WT.addEventHandler("pageview", function(e){
    var params = e.target.params;
    var projectAlias = params.testAlias,
        testID = params.r_runID,
        experimentID = params.r_experimentID || params.r_personalisedID || "BASELINE";

    _mip_track = _mip.track || [];
    _mip.track.push({
        project_alias: projectAlias,
        test_id: testID,
        experiment_id: experimentID
    });
});
```

### With the Optimize Build Framework 


