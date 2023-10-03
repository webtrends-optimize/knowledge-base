# Aborting the tag

There are many reasons where you could choose for Optimize to not run. This document covers how, where, and a few examples. 

## How and where

These are all Javascript conditions. We recommend making sure the information is available before the wt.js tag runs on the page, so that we're not waiting for the page to load prior to aborting the tag.

- If this is possible, the code you'll write should go into pre-init. 
- If this is not possible, the code should go into post-load, where you poll for some conditions before running `WT.optimize.setup`, and selectively abort at that point. Note though, that polling for conditions slows down page load times.

**Notes:**

- Aborting the tag does not force the rest of the code to stop. You may wish to either use `throw`, `return` or `if` conditions to make sure future parts of preinit do not run (if needed).

## Examples

### Abort the tag for known bots

You may not want known bots to run Optimize - aborting the tag for these will save some of your session usage.

``` javascript 
var regex = /(Yahoo Link Preview|Yahoo! Slurp|YOURLS|webcrawler|Crawler Bot|Youtube-Links|Google Search Console|Google-Adwords|Google Page Speed Insights|GoogleEarth|GoogleToolbar|Google-Apps-Script|Google Keyword Suggestion|Google-SearchByImage|Yahoo Ad monitoring|rssowl|yandex.com\/bots|GetIntent Crawler|grammarly|feedly|web spider|Go-http-client|Mediapartners-Google|AdsBot|python-requests|Google\s?bot|AhrefsBot|bingbot|YandexBot|DotBot|sitescorebot|deepcrawl|cocolyzebot|SMTBot|pingbot|leavemealonebot|pingdom.com|onetrustbot|storebot|getthit.com|taboolabot|radius compliance bot|klarnabot|pricewatcher|moatbot|Baiduspider|GrapeshotCrawler|AlphaBot|proximic|coccocbot|NetcraftSurveyAgent|SemrushBot|Mail.RU_Bot|YandexImages|ZmEu|BingPreview|FeedDemon|Twitterbot|SimplePie|Webspider|CloudFlare-AlwaysOnline|MagpieRSS|longurl|tt-rss|Google Favicon|GroupHigh|ia_archiver|google.com\/feedfetcher|MJ12bot|Scrapy|archive.org_bot|Yeti\/|zgrab|vkShare|Screaming Frog SEO|NetSeer crawler|crawler4j|letsencrypt.org|Google-Site-Verification|DomainSONOCrawler|Download Master|WhatsApp\/\d|GnuTLS|Cliqzbot|riddler.io\/|okhttp|filterdb.iss.net\/crawler|facebookexternalhit|Google Web Preview|AppEngine-Google|Jersey\/|DuckDuckGo-Favicons-Bot|rogerbot|YahooCacheSystem|Apache-HttpClient|bidswitchbot\/|Clickagy Intelligence Bot|MaxPointCrawler\/|LightspeedSystemsCrawler|Photon\/\d|FeedBurner\/|Wappalyzer|LinkpadBot|Crawler\/\d|BrokenLinkCheck.com|libcurl\/|TurnitinBot|Microsoft Office Protocol Discovery|Microsoft Windows Network Diagnostics|skype-vhod|\+https?:\/\/[\w\.\-\/]+(robot|bot|crawler|embed|fetcher|seznambot|feedparser|daum.net|brandwatch|spider|checksite|google.com|goo.gl\/))/i

if(navigator.userAgent.match(regex)){
    WT.optimizeModule.prototype.abort();
}
```

### ES6 Check / Abort

This script aborts Optimize if the browser is not ES6 compatible. 

``` javascript
var supportsES6 = (function () {
    try {
        new Function("async () => {}");
        new Function("(a = 0) => a");
        return true;
    }
    catch (err) {
        return false;
    }
}());
if (!supportsES6) {
    WT.optimizeModule.prototype.abort();
}
```

