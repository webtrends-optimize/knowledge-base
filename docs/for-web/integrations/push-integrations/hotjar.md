# Hotjar - Push Integration

This integration will send experiment views into Hotjar as an event, allowing you to then filter on that information. 

**Note:** Hotjar has a tiered offering, and not all users have access to the feature of segmentation by event. 

**Note:** There may be nuances in scopes reported on - session vs. user. We recommend Hotjar for viewing session recordings, but not for any further analysis. 

## Pre-init script

Add the following into your pre-init to send data to Hotjar:

``` javascript
(function hotJarIntergration(){
    try{
        function sendToHJ(data){
            if (!window.hj || typeof hj !== 'function'){
                setTimeout(function(o){
                    sendToHJ(o);
                }, 500, data);
                return;
            }
            WT.helpers.bdebug.log("WTO: hotjar event", 'WTO - '+ data.WT_test +' - '+ (data.WT_variation).toString());
            hj("event", 'WTO - '+ data.WT_test +' - '+ (data.WT_variation).toString());
        }
        WT.addEventHandler("pageview", function (g) {
            g = g.target.getParams();
            var t = g.testAlias,
                l = g.r_experimentID || g.r_personalizedID || "BASELINE";
            sendToHJ({
                "WT_test" : t,
                "WT_variation": l
            });
        });
    } catch(err){
        if(document.cookie.match(/_wt.bdebug=true/i)) console.log(err);
    }
}());
```