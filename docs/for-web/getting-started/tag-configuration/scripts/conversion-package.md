# Conversion Package

To have a conversion package, you should add this code to your pre-init script in your Tag.

There are two parts users typically maintain - the oConv array labelled Metrics, and Triggers.


``` javascript
/* 
    CONVERSION PACKAGE
*/
(function() {
    try {
    
        var testAliasExclusionList = ['ta_dummy_exclude']; // (3)!

        // Metrics 
        var oConv =  [ // (1)!
            {
                name: 'conversion_name', // (2)!
                regex: /example.com/i       
            },
            {
                name: "page_cart",
                path: /^\/cart\/?$/i // (4)!
            },
            {
                name: "click_atb_pdp",
                onEvent: "atb", // (6)!
                regex: /\/products/
            },
            {
                name: "page_shipping",
                path: /\/checkout/i,
                ifCondition: function(){ // (5)!
                    return document.title.match(/\s*Shipping/i)
                }
            },
            {
                name: "purchase",
                onEvent: "datalayer-ready",
                collectData: function(){ // (7)!
                    let purchaseEvent = dataLayer.filter(x => x.ecommerce);
                    return {
                        revenue: purchaseEvent.purchase.actionField.revenue,
                        units: purchaseEvent.purchase.products.length
                    }
                }
            }
        ];

        // Debugger
        if (window.location.href.match(/_wt.convPackageDebug=true/i)) document.cookie = '_wt.convPackageDebug=true;path=/;';
        var Print = {},
            bDebug = Boolean(document.cookie.match(/_wt.convPackageDebug=true/i));
        if (bDebug) {
            Print.debug = window.console;
            Print.debug.alert = window.alert;
        } else {
            Print.debug = {
                log: function() {},
                info: function() {},
                warn: function() {},
                error: function() {},
                dir: function() {},
                alert: function() {}
            };
        }
        // Core
        Print.debug.log('Conversion Package Running');
        
        var Core = {};
        WT.CPCore = Core;

        // domainID & keyToken
        var cfg = WT.optimizeModule.prototype.wtConfigObj;
        var domainID = cfg.s_domainKey;
        var keyToken = cfg.s_keyToken;
        
        Core.cookieMethods = WT.helpers.cookie;

        // Get testAliases
        Core.getTestAliases = function () {
            if (window.location.search.match(/cpDummy/i) && !Core.cookieMethods.has('wt_dummy_pkg')) {
                var answer = window.prompt();
                Core.cookieMethods.set('wt_dummy_pkg', answer, 1);
            }
            var cookies = document.cookie.match(/_wt.control\-\d+\-([^=]+)/g);
            var projectCookies = document.cookie.match(/_wt.project/i);
            if (!cookies && !projectCookies) return false;

            var arr = [];
            if (Core.cookieMethods.has('wt_dummy_pkg')) {
                var dummyTa = Core.cookieMethods.get('wt_dummy_pkg');
                arr.push(dummyTa);
            } else if (cookies) {
                for (var i = 0; i < cookies.length; i++) {
                    var c = cookies[i].replace(/_wt.control\-\d+\-/i, '');
                    arr.push(c);
                }
            }

            if (WT.getProjectCookies) {
                var list = Object.keys(WT.getProjectCookies());

                for (var i = 0; i < list.length; i++) {
                    arr.push(list[i].replace(/_wt.control\-\d+\-/i, ''));
                }
            }

            return arr;
        };

        /* -------------------- */

        // Get matching url
        Core.getMatchingUrl = function(wlh, regArr) {
            for (var j = 0; j < regArr.length; j++) {
                if (wlh.match(regArr[j])) {
                    Print.debug.log("page matches:", regArr[j]);
                    return true;
                }
            }
        };

        /* Fire CTrack2 for each Alias
        -------------------------------------------------------------*/
        Core.trackAliases = function (testAliases, testAliasExclusionList, me, dataInfo) {
            dataInfo = (typeof dataInfo === 'undefined') ? {} : dataInfo;

            //remove excluded Aliases
            for(var l=0; l<testAliasExclusionList.length; l++) {
                var excludedAlias = testAliasExclusionList[l];
                index = testAliases.indexOf(excludedAlias);
                if (index > -1) {
                    testAliases.splice(index, 1);
                }
            }

            var o = {
                testAlias: '',
                conversionPoint: me.name,
                additionalCookies: {},
                data: dataInfo
            };

            var projectCookies = {};
            if (WT.getProjectCookies) {
                projectCookies = WT.getProjectCookies();
            }

            for (var k = 0; k < testAliases.length; k++) {
                var tA = testAliases[k];
                //collect control cookie
                var cookiename = '_wt.control-' + domainID + '-' + tA;
                var controlcookie = Core.cookieMethods.get(cookiename);
                if (controlcookie) {
                    o.additionalCookies[cookiename] = { value: controlcookie };
                }

                var pcookie = projectCookies[cookiename];
                if (pcookie) o.additionalCookies[cookiename] = pcookie;
            }

            // Aliases: ta_1,ta_2
            var testAliasString = testAliases.join()
            o.testAlias = testAliasString;

            o.additionalCookies['_wt.user-'+ domainID] = {value: Core.cookieMethods.get('_wt.user-'+ domainID)};
            o.additionalCookies['_wt.mode-'+ domainID] = {value: Core.cookieMethods.get('_wt.mode-'+ domainID)};

            Print.debug.log("ctrack:", o);
            WTO_CTrack2(o);

        };

        WT.trackEvent = function(o){
            Core.trackAliases(
                [o.testAlias],
                [],
                {
                    name: o.conversionPoint
                },
                o.data || {}
            );
        };

        /* Evaluate oConv Object
        -------------------------------------------------------------*/
        function doEvaluate(onEvent){ // (8)!

            var wlh = window.location.href;
            var testAliases = Core.getTestAliases();
            if(!testAliases || !testAliases.length) return;

            for(var i=0; i<oConv.length; i++){

                var me = oConv[i];

                // Check onEvent is correct
                if((!onEvent && !me.onEvent) || me.onEvent == onEvent){
                    // fine
                } else {
                    continue; // skip this one.
                }

                // Check regex
                if(me.regex){
                    var regArr = (me.regex instanceof Array) ? me.regex : [me.regex];
                    var urlMatched = Core.getMatchingUrl(wlh, regArr);
    
                    if(!urlMatched) continue;
                }
                if(me.path){
                    var regArr = (me.regex instanceof Array) ? me.regex : [me.regex];
                    var urlMatched = Core.getMatchingUrl(location.pathname, regArr);
    
                    if(!urlMatched) continue;
                }

                //Conditional Conversion
                if(me.ifCondition){
                    try {
                        if(!me.ifCondition()){
                            continue;
                        }
                    } catch(err) {
                        continue;
                    }
                }

                //Check if we need to collect data
                var data = {};
                if( typeof me.collectData === 'function') {

                    try {
                        data = me.collectData();
                    } catch(err){}

                }
                    
                Core.trackAliases(testAliases, testAliasExclusionList, me, data);

            }//url loop
        }

        /* CTrack2
        -------------------------------------------------------------*/
        var WTO_CTrack2 = function(params) {

            var OTS_ACTION = params.conversionPoint ? "track" : "control";
            var OTS_GUID = domainID + (params.testAlias ? "-" + params.testAlias : "");
            var useTestGroupShared = false;
            
            var OTS_URL_POST = "https://ots.webtrends-optimize.com/ots/api/rest-1.2/" + OTS_ACTION + "/" + OTS_GUID;

            // Send request only when there is at least one Optimize cookie
            var aCookies = params.additionalCookies || {};
            var cookiesFound = document.cookie.match(/_wt\.(user|mode)-[^=]+=[^;\s$]+/ig);
            if (cookiesFound) {

                /* Send conversion request
                -------------------------------------------------------------*/
                var qpURL = "\x26url\x3d" + (params.URL ? encodeURIComponent(params.URL) : window.location.origin + window.location.pathname);
                var qpConversion = params.conversionPoint && "\x26conversionPoint\x3d" + params.conversionPoint || "";
                var qpData = params.data ? "\x26data\x3d" + JSON.stringify(params.data) : "";
                var offsetValue = function() {
                    var rightNow = new Date;
                    var offset = -rightNow.getTimezoneOffset() / 60;
                    return offset * 1E3 * 60 * 60
                };
                var offset = "\x26_wm_TimeOffset\x3d" + offsetValue();
                var referrer = "";
                if (window.document && window.document.referrer && document.referrer !== "") referrer = "\x26_wm_referer\x3d" + document.referrer;

                var rqElSrc = "keyToken\x3d" + keyToken + "\x26preprocessed\x3dtrue\x26_wt.encrypted\x3dtrue\x26testGroup\x3d" + (useTestGroupShared ? "shared" : "default") + qpURL + "\x26cookies\x3d" + JSON.stringify(aCookies) + qpConversion + qpData + offset + referrer;

                Print.debug.log("--- Sending Request ---");
                Print.debug.log(rqElSrc);

                if ( navigator.sendBeacon && window.Blob ) {
                    var blob = new Blob([rqElSrc], { type: 'application/x-www-form-urlencoded' });
                    navigator.sendBeacon(OTS_URL_POST, blob);
                } else {
                    var xhttp = new XMLHttpRequest();
                    xhttp.open("POST", OTS_URL_POST, true);
                    xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
                    xhttp.send(rqElSrc)
                }

            }
        };
        WT.helpers.CTrack2 = WTO_CTrack2;

        /* Triggers (9)
        -------------------------------------------------------------*/
        
        // On page load
        WT.addEventHandler("INITIALIZE", function (r) {
            doEvaluate(false); // (10)!
        });

        // Poll for page change 
        var curURL = window.location.href;
        setInterval(function(){
            if(window.location.href !== curURL){
                curURL = window.location.href;
                Print.debug.log('path changed');
                doEvaluate(false);
            }
        }, 1000);

        /* window.opt_data (11)
        -------------------------------------------------------------*/
        function processOptData(aEntry){
            try {

                if(!(aEntry instanceof Array)) return "bad input"; // bad input

                var testAliases = Core.getTestAliases();
                if(!testAliases || !testAliases.length) return "no control cookies found, nothing to store.";

                aEntry.forEach(function(o){
                    if(o.event){
                        var o_data = {};
                        if(o.data){

                            // clean data. only keep types we can store.
                            for(var key in o.data){
                                if( !(typeof o.data[key]).match(/number|string/i) ){
                                    continue; // bad data type, move on.
                                }

                                o_data[key] = o.data[key];
                            }
                        }

                        Core.trackAliases(testAliases, testAliasExclusionList, {
                            name: o.event
                        }, o_data);
                    }
                });

            } catch(err){}
        }

        if(window.opt_data && opt_data.length){
            // we have records, do something with it.
            processOptData(opt_data);
        }
        if(!window.opt_data || window.opt_data instanceof Array){
            window.opt_data = {
                h: (window.opt_data || []),
                push: function(o){
                    try {
                        if(!o) return "opt_data.push - missing any arguments";
                        if(!o.event) return "opt_data.push - you should provide o.event";

                        this.h.push(o);

                        processOptData([o]);

                        return this.h;

                    } catch(err){
                        return err;
                    }
                }
            };
        }

    } catch(err){
        // console.warn(err);
        if(document.cookie.match(/_wt.bdebug=true/i)) console.log(err);
    }
})();
```

1. This is an Array of all metrics, irrespective of when they should fire.
2. **Name**. The name you want to assign to the metric
3. **Exclusion list**. If you don't want certain tests to capture any of these metrics, just add them into this exclusion list. 
4. **URL or Path**. You can target URLs either by regex (full url) or path.
5. **ifCondition**. You can specify javascript conditions for firing the metric here. These are a final check, and expect a true/false response.
6. **onEvent**. Specifying an event corresponds to triggers, where you can pass in an event name. Consider it an additional layer of filtering - sending in the atb event to the trigger will only allow atb events to be evaluated - all others will be skipped. 
7. **collectData**. Collect any data you wish, returned as a JSON object for us to collect. Make sure the data is flat - do not include nested objects.
8. **doEvaluate**. The function that processes each item in the oConv object. 
    
    You can modify and add to this as you need to, to accept more arguments or handle them differently. 

9. **triggers**. Defining when we should trigger an evaluation of the conversion package.
10. **doEvaluate(false)**. Passing in false means assuming no event has been defined. This is the default.

    You could change this to `doEvaluate("datalayer-ready")` to evaluate tests with that event assigned.

11. **opt_data**. This provides a user-accessible function that allows them to track metrics for every project. 

    It is similar to `datalayer.push` - use it by adding it to your website and pushing in events as you wish to collect them.

    As with anything API or code driven, you do not need to define these events anywhere else, although we recommend storing them in the Automatic Conversions section. 
    
    You do, however, need to define any Custom Data attributes you attach to these events, like Revenue and Units that you'd attach to a Purchase event.

100.  :man_raising_hand: I'm a code annotation! I can contain `code`, __formatted
    text__, images, ... basically anything that can be written in Markdown.