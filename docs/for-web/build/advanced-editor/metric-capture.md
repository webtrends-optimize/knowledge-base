# Metric Capture in the Advanced Editor

There are a few different approaches and options to capturing metrics in the Advanced Editor. 

**Note:** First, you should consider if the advanced editor is even the right place to track your metrics. The rule of thumb is: 

- If your metric is useful for every test you build (or many of them), consider adding it to your Conversion Package or using opt_data.

- If your metric is an adhoc metric for this one test, the advanced editor is the correct place to put it. 

This document assumes that having been through this logic, you've arrived at the latter option and understand this is the right place to add your metric. We will discuss the approaches below.

## Options available

Your options in the Advanced Editor are:

1. **Tracking custom events with code** - [Jump to this](#1-tracking-custom-events-with-code)

    This is a useful solution if your metrics are on the **same page/s** that your test runs on. e.g. I have a homepage test and want to track clicks on the homepage too.

2. **Tracking Page Load Conversions** - [Jump to this](#2-tracking-page-load-conversions)

    This is used for tracking people who reach a page **that is not part of your location**. If you want to track a subset of your test pages as a goal, simply do so with code. If page is equal to X, track metric.

3. **Tracking remote script-only conversions** - [Jump to this](#3-tracking-remote-script-only-conversions)

    This is used to capture custom behaviour on a page that is not your test page. E.g. I have a homepage test, but want to track clicks on some tabs on the login/register page.

4. **Tracking time-on-page conversions** - [Jump to this](#4-tracking-time-on-page-conversions)

    This is used to capture whether people reached a given time threshold e.g. 5s, 10s, etc. on any page of your choosing.

Achieving each of these options is detailed below.

### What will they look like in my reports?

Every metric, no matter how you get it into the platform, comes out looking the same. Whether you use opt_data, the conversion package, page load goals, javascript-based click tracking, etc. - they are all reported on by name, and are able to be filtered on, ignored, etc.

## 1. Tracking custom events with code 

### Without the Optimize Build Framework

The markup to send metrics to us is quite simply:

``` javascript
WT.click({
    testAlias: "ta_something",
    conversionPoint: "event_name_here"
});
```

If you have a lot of these metrics, you may wish to create a small "track" helper function 

``` javascript
!function(){
    var track = function(name){
        WT.click({
            testAlias: "ta_something",
            conversionPoint: name
        });
    };

    track("metric_1");
    track("metric_2");
    // etc.
}();
```

Note that we run your code in the global namespace, so you should wrap your code in an IIFE as we've done above.

In all likelihood, you will want to capture behavioural events. It is up to you to hook into these as you see fit. We have a few examples below, though. 

#### Tracking a click 

``` javascript
document.addEventListener('click', e => {

    if(e.target.closest('.my-element')){
        track('click_myelement');
    }

});
```

#### Tracking element visibility

This is a basic implementation. For more performant ones, consider throttled events or intersection observers.

It makes sure at the element is at least 200px from the bottom of the screen, so a fair amount of it is shown to users.

``` javascript
var scrolled = false;
document.addEventListener('scroll', e => {

    if(scrolled) return;

    if( window.scrollY + (window.innerHeight - 200) > document.querySelector('#about-us-section').offsetTop ){
        track("scroll_myelm");
        scrolled = true;
    }
    
});
```

#### Tracking a metric with Custom Data 
``` javascript
WT.click({
    testAlias: "ta_something",
    conversionPoint: "event_name_here",
    data: {
        key1: "value1",
        key2: "value2"
    }
});
```

!!! note "Don't forget to declare your custom data fields"
    If you're tracking custom data, don't forget to declare your custom data fields in the conversions tab, along with their type - Text, Integer or Decimal.

![AE Custom Data](/assets/ae-custom-data.png)

### With the Optimize Build Framework

The code is simpler if tracking metrics within the Optimize Build Framework. There is no need to specify the testalias on each metric capture.

``` javascript
Test.conversion("event_name_here");
```

#### Tracking a metric with Custom Data in the Optimize Build Framework

``` javascript
Test.conversion("event_name_here", {
    data: {
        key1: "value1",
        key2: "value2"
    }
});
```

## 2. Tracking Page Load Conversions

Tracking page load conversions relies on the same Locations mechanism you will have used to define your Project Location on the first screen of the Advanced Editor. 

![AE Conversion Pageload](/assets/ae-conversion-pageload.png)

!!! note "Tip: Make sure you click Add Conversion"

    Make sure you click the Add Conversion button once you're happy with your inputs. Easily forgotten.

## 3. Tracking remote script-only conversions

This is just like a Page Load conversion, except that you'll be presented with some script to edit. We include some sample code for you to play with.

## 4. Tracking time-on-page conversions

This is just like a Page Load conversion, except that you'll be presented with a time threshold. 

To track a metric if people have been on the site for 10 seconds, simply put 10 in the box. By tracking these as binomial metrics, comparisons, uplifts and significance are all easy to interpret.