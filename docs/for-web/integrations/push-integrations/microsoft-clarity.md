# Integrate Webtrends Optimize with Microsoft Clarity - Push Integration

Microsoft Clarity offers free session recording as well as heatmaps. 

Its possible to add WTO meta data into Clarity to allow these to be filtered and reported by Project and Variations.

MS Clarity document reference: <https://learn.microsoft.com/en-us/clarity/filters/custom-tags>

With Clarity installed on a site, the following function can be called and data introduced. 

`clarity("set", "key", "value");`

## Pre-init code

Add the following to your pre-init script to send data to Microsoft Clarity.

``` javascript
(function clarityIntergration(){
    try{
        function sendToClarity(data){
            if (!window.clarity || typeof clarity !== 'function'){
                setTimeout(function(o){
                    sendToClarity(o);
                }, 500, data);
                return;
            }
            WT.helpers.bdebug.log("WTO: clarity set", data.WT_test, (data.WT_variation).toString());
            clarity("set", data.WT_test, (data.WT_variation).toString());
        }
        WT.addEventHandler("pageview", function (g) {
            g = g.target.getParams();
            var t = g.testAlias,
                l = g.r_experimentID || g.r_personalizedID || "BASELINE";
            sendToClarity({
                "WT_test" : t,
                "WT_variation": l
            });
        });
    } catch(err){
        if(document.cookie.match(/_wt.bdebug=true/i)) console.log(err);
    }
}());
```

## In Microsoft Clarity

To then filter on your reports in Microsoft Clarity, you can:

1. Click on Filters
2. Scroll to “Custom Filters” ==> “Custom Tags” and from the dropdown select Project Alias.
3. Select second drop down to nominate conditions by Experiment ID.

![MS Clarity Screen 1](/assets/ms-clarity-1.png)
![MS Clarity Screen 2](/assets/ms-clarity-2.png)