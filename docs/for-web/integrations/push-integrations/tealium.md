# Integrate Webtrends Optimize with Tealium - Push Integration

Tealium allows for analytical reporting on web events. 

Its possible to add WTO meta data into Tealium to allow these to be filtered and reported by Project and Variations.

Tealium document reference: <https://docs.tealium.com/platforms/javascript/track/>

With Clarity installed on a site, the following function can be called and data introduced. 

`utag.link({ "tealium_event": "event_name", "fieldN": "valueN" }});`

## Pre-init code

Add the following to your pre-init script to send data to Tealium.

``` javascript
// Tealium Tracking
(function tealiumIntergration(){
    try{
        function sendToUtag(data){
            if (!window.utag || typeof utag.link !== 'function'){
                setTimeout(function(o){
                    sendToUtag(o);
                }, 500, data);
                return;
            }
            utag.link(data);
        }
        WT.addEventHandler("pageview", function (g) {
            g = g.target.getParams();
            var t = g.testAlias,
                l = g.r_experimentID || g.r_personalizedID || "BASELINE";
            sendToUtag({
                "tealium_event"  : "wto_test_entry",
                "WT_test" : t,
                "WT_variation": l
            });
        });
    } catch(err){
        if(document.cookie.match(/_wt.bdebug=true/i)) console.log(err);
    }
}());
```