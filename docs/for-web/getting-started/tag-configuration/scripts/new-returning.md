# New Returning

## Script for preinit 

``` javascript
/* 
    NEW OR RETURNING - DATA LAYER ENTRY AND STORAGE
    
    Rules:
    - If GA cookie is present, look back at the timestamp. 
      - If > 3hrs ago, assume returning. 
      - Else assume new

    - Else look for _wt.nr_s cookie 
      - If present and _wt.nr cookie present, assume value of _wt.nr cookie
      - If present and _wt.nr cookie not present, assume new 
      - If not present and _wt.nr cookie is present, set _wt.nr cookie to returning and assume returning
      - If not present and _wt.nr cookie not present, set _wt.nr cookie to new and assume new

    Dependencies:
    - WT.Storage

*/

;!function(){

    var new_returning = "new";

    try {

        var gacookie = WT.helpers.cookie.get("_ga");
        if(gacookie){

            var ga_ts = parseInt( gacookie.match(/\d+$/)[0] );
            var now = Date.now();
            var _3hrs = 1000*60*60*3;

            if(ga_ts < (now - _3hrs)){
                new_returning = "returning";

            } // else stay as new

        } else {

            var wtnrcookie = WT.Storage.get("_wt.nr");
            var wtnrscookie = WT.Storage.get("_wt.nr_s");

            if(wtnrscookie){

                if(wtnrcookie){
                    new_returning = wtnrcookie;
                } else {
                    // stay as new 
                    WT.Storage.set("_wt.nr", "new");
                }

            } else {

                if(wtnrcookie){
                    new_returning = "returning"
                } else {
                    // stay as new 
                }

                WT.Storage.set("_wt.nr", new_returning, 30);
                WT.Storage.set("_wt.nr_s", "1");

            }

        }

    } catch(err) {}

    WT.optimizeModule.prototype.wtConfigObj.data.new_returning = new_returning;

}();
```

## Segmenting by New or Returning

Once this is done, you will be able to apply new_returning attributes to both Locations and Segments. 

It's worth noting the difference between the two:

- **Locations** evaluate on every page load, and so users can fall out if their criteria no longer matches. 

- **Segments on Targets** behave the same way. They evaluate on every load and so have the same behaviour of being able to not see something you saw previously. 

- **Segments on Tests** evaluate once, are sticky, and so users stick with whatever decision and allocation was made. So if we target new users and users come back 2 days later, they'll stay as new. This is less of a problem for targeting returning users, which is a "final state" in that users who become returning won't ever be new again.

## Dependencies

Note that this requires access to [WT.Storage](../wt-storage), which is a helper for handling browser-side storage.