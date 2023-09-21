# Masking

## What is masking?

In Webtrends Optimize, we refer to masking as the technique by which we hide the page, or parts of the page, until the final content is rendered for users. By doing so, we ensure there is no Content Flickering - where users see the old content, a flash, and then the new content loads in. 

## Where does masking occur?

We have different places where masking can occur:

- Before the tag, if an async implementation is used.
- In the tag config or preinit, while tests are being loaded
- In the test, if you're waiting on additional things from the page before rendering your changes. 

Each of these steps are vital in ensuring there is zero content flickering.

## Does content flickering matter?

Yes. 

## Before the tag

An alternative implementation approach to adding our script tag as HTML directly to the head of the website is to write it asynchronously with masking.

The approach we take here is:

- Hide the page 
- Set a safety timeout to remove the masking
- Write the tag to the page using Javascript asynchronously.
- Onload and Onerror of the tag, remove the masking

This approach has been used successfully on some request-light websites, and effectively removes Webtrends Optimize from the critical load path.

## In tag masking

!!! note "24 hour cache"
    Our tag sits on a CDN and is cached for upto 24 hours to keep Google happy. Please plan your updates in advance. 

While you're waiting for your tests to load, you have two core options for masking.

1. Mask the whole website
2. Mask on a page-by-page (or section-of-page by section-of-page) basis.
3. Write your own rules.

### 1 - Masking the whole website

When modifying your tag, you'll see Display Mode. The options in this dropdown are as follows:

- None: No masking. This is enabled by default, to keep our footprint light unless we decide otherwise.
- Display: Applying `display: none` to the body tag. 
- Visibility: Applying `visibility: hidden` to the body tag
- Shift: Applying `left: -1000%; posiiton: absolute` to the body tag.
- Overlay: Create a layer that sits above the web page.

We pick from these options based on the website - some are fine to have the body hidden, some aren't. 

We can also use the below library, with a global regex (e.g. .*), and your own rules, e.g. `body { opacity: 0.0000001 !important }`. We often use this technique for sitewide page hide, as this is the most reliable in not breaking website functionality. 

Snippet:

``` javascript
    // Add things into here for all traffic.
    WT.optimizeModule.prototype.whitelist = [
        {
            URLs: [
                /.*/i
            ],
            css: 'body { opacity: 0.00000001 !important; }'
        },
    ];
```

### 2 - Masking on a per-page basis

For this, we use the library as below and simply add rules into the array `WT.optimizeModule.prototype.whitelist` each time you build a new test.

This code, as provided, should go into the pre-init section of your tag to fire at the right time.

``` javascript
//Page Hide
(function pageHide(){
    // Add things into here for all traffic.
    WT.optimizeModule.prototype.whitelist = [
        {
            URLs: [
                /[\?&]_wt.testWhitelist=true/i
            ],
            css: 'body { opacity: 0.00000001 !important; }'
        },
        {
            URLs: [
                /mysite.com\/basket/i
            ],
            css: 'body { opacity: 0.00000001 !important; }'
        },
        {
            URLs: [
                /\/collections\//i
            ],
            css: 'body { opacity: 0.00000001 !important; }'
        },
        // other rules
    ];
    // Add things into here if you only want them to show whilst in staging mode.
    if (window.location.href.match(/_wt.mode=staging/i)) {
        var stagingWhitelist = [
        ];
        for (var i = 0; i < stagingWhitelist.length; i++) {
            WT.optimizeModule.prototype.whitelist.push(stagingWhitelist[i]);
        }
    }
    WT.optimizeModule.prototype.checkWhitelist_CUSTOM=function(){var e,t,o,n=function(e){var o=e||"";return{add:function(e){e.length&&(o+=e+"\n")},output:function(e){var t;if(o.length){if(e&&(t=document.getElementById(e)),t)return!1;(t=document.createElement("style")).setAttribute("type","text/css"),e&&(t.id=e),t.styleSheet?t.styleSheet.cssText=o:t.appendChild(document.createTextNode(o)),document.getElementsByTagName("head")[0].appendChild(t),o=""}},remove:function(e,t){if(!e)return!1;e=(t=t||window.document).getElementById(e);e&&"style"==e.nodeName.toLowerCase()&&e.parentNode.removeChild(e)}}},i=WT.optimizeModule.prototype.whitelist||[];try{for(var r="",a=!1,d=0,s=i.length;d<s;d++){var u,c=i[d],p=c;c.URLs&&c.css&&(p=c.URLs,u=c.css||""),!0===function(e){if(!e)return!1;!1==e instanceof Array&&(e=[e]);for(var t,o=0;t=e[o];o++)if(window.location.href.match(t))return!0;return!1}(p)&&(u&&(r+=u),WT.optimizeModule.prototype.wtConfigObj.s_pageTimeout=5e3,WT.optimizeModule.prototype.wtConfigObj.s_pageDisplayMode="custom",a=!0)}return""!==(WT.obfHide=r)&&(e=(e=r).match(/[\{\}]+/)?e:e+"{ opacity: 0.00001 !important; }",t=new n(e),o="wto-css-capi-"+Math.floor(1e3*Math.random()),WT.addEventHandler("hide_show",function(e){e.params&&(!1===e.params.display&&t.output(o),!0===e.params.display&&t.remove(o))}),setTimeout(function(){t.remove(o)},5100)),a}catch(e){}};
    WT.optimizeModule.prototype.checkWhitelist_CUSTOM();
})();
```

If empty, the array should look like this:

``` javascript
WT.optimizeModule.prototype.whitelist = [];
```

#### Testing whitelist entries

We recommend a Chrome Extension like Resource Override or application like Fiddler for rapidly testing updates locally (on your machine).

If used, you'll be able to find the whitelist array, add values into it, and test it locally before saving changes in the UI.

In the above example, _wt.testWhitelist can be added to any URL to test how the page would respond with whitelisting enabled. e.g. https://www.webtrends-optimize.com/?_wt.testWhitelist=true

You could add multiple values with different CSS rules for each one, if you'd like to test whole page masking vs. masking a block on the page, for example.

### 3 - Write your own rules

The Optimize tag is highly configurable by a JavaScript dev. In the case of page hiding, we broadcast the `hide_show` event which you can hook into with `WT.addEventHandler`, and create your own page hide rules. So if you'd like to use Opacity, or anything else, you can simply write a hook for the `hide_show` event (which contains details of whether it's a hide or show) and apply any CSS to the page that you like. 

``` javascript
WT.addEventHandler("hide_show", function(e){
    console.log(e);
});
```

## In test masking

Let's say you want to wait for the page to load before executing your changes, and so you hook into the domready state with jQuery:

``` javascript
jQuery(document).ready(function(){
    jQuery('#hero p').text("my new text");
});
```

Optimize's layer-2 masking will only take effect up until your code is delivered to the page. At this point, it will step away. 

This third layer of masking needs to happen while you wait for your conditions to be fulfilled. Given you can code these in any way, it is also on you to decide if/how to mask the page.

Example:
``` javascript
// Mask the page 
WT.helpers.css.add('#hero { opacity: 0 !important; }', 'wt-001-mask');

// Make sure there's a safety redisplay
setTimeout(function(){
    // Remove the mask
    WT.helpers.css.del('wt-001-mask');
}, 4000);

jQuery(document).ready(function(){
    jQuery('#hero p').text("my new text");

    // Remove the mask
    WT.helpers.css.del('wt-001-mask');
});
```

### How the Optimize Build Framework handles masking

The Optimize Build Framework (OBF) writes styles to the page based on your `Config.cssHide` value. Once polling conditions are fulfilled and transformations are handled, you'll see a `showHide` function called that removes this mask. 

Thankfully, these are not things you need to pay attention to, although if you wish to override the functionality the code is there for you.

## Impact on page load performance 

All JavaScript has a detrimatental impact on page load performance, but masking plays a critical role in limiting the negative impacts of experimentation. 

Core Web Vitals find Cumulative Layout Shift (CLS) to be one of the most important factors for onpage performance. This is where parts of the page jump, fractionally or drastically, during the page load process. 

For example, a button slowly nudges lower and lower as more aspects load. 

Masking combats this - showing nothing and then the final page at the end. If well-built, your tests should have a near-zero CLS score for this reason. 

Infact, we have found masking to by the remedy to websites with poor CLS scores, where it doesn't impact other metrics too much but solves CLS issues.

Other metrics though, such as First Contentful Paint, will be affected by masking. They are less impactful than CLS, but will be affected. 

The degree to which they're affected depends on page performance, but you can see our requests execute in typically under 150ms (often closer to 30ms).