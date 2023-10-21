# Consent Management - Advanced

!!! note "Looking for something more straightforward?"
    If the below feels too complex, you may want to check out [Consent Management - Simple](../consent-management-simple)

## What problem/s does the Advanced approach solve?

The primary drawback of traditional Consent Management integrations is the user experience. By delaying the rendering of experiences until after opting-in, users have a disjointed experience seeing some of the old content and then the new.

This can lead to Observation Bias, where users know they're being experimented on, and so will behave differently. In doing so, your results are likely invalid, and when you hard-code the variation of the test, you can expect to see very different results from your Analytics reports.

To remain compliant, we must not cookie users (preserve any information about them, in any format) or track/measure any behaviour (transmit it back to yourself or to us), but applying visuals to the page pre opt-in is perfectly legal. And so the appraoch taken by this Advanced approach directly takes this line.

## Enabling the Advanced approach

!!! note "Are you on the right tag version?"
    This process requires tag version 5.8+.

We need two key pieces of information for this approach to work. 

1. How can we accurately detect the current status of consent. 
2. How can we detect any changes in consent.

If we're able to do these two things, we can then:

- Make sure the tag respects the consent on the first page load
- Track behaviour and cookie when the status changes
- Make sure we openly perform our actions on the 2nd page load after opt-in. 

Once you are on tag version 5.8+, enabling this method requires two functions to be created in your pre-init script:

``` javascript
WT.consent_checkInitialState = function(){
    return value_for_user_has_already_opted_in;
};

WT.consent_updateState = function(cb){
    window.addEventListener("user_optin_event", function(ev){
        cb();
    });
};
```

If present, the tag will listen to these methods and switch to the advanced consent mechanism automatically.

## Known optin hooks we use:

**Note**: We appreciate you may put us in another category based on your usage of the platform. The categories may change, but the hooks listed here are reliable.

### Cookiebot

``` javascript
WT.consent_checkInitialState = function(){
    return !!document.cookie.match(/CookieConsent=[^;]+preferences.true/)
};

WT.consent_updateState = function(cb){
    window.addEventListener('CookiebotOnAccept', function() {
        if (Cookiebot.consent.marketing) {
            cb();
        }
    }, false);
};
```

### Onetrust

``` javascript
WT.consent_checkInitialState = function(){
    var optanoncookie = document.cookie.match(/OptanonConsent=([^;]+)/)[1];
    return optanoncookie.match(/C0002:1/i)
};

WT.consent_updateState = function(cb){
    window.addEventListener("consent.onetrust", function(ev){
        if(ev.detail.includes("C0002")){
            cb();
        }
    });
};
```