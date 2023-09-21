# Tag Implementation for Cookiebot

!!! info "Looking for general tagging guidance?"

    This document expects you to be familiar with tagging concepts discussed [here](../). 

Cookiebot is a consent management system, typically run from the front-end of a website.

The below is example tagging, expected to be put in the `HEAD` tag of the page, **before** the Cookiebot script to make sure we can hook into acceptance.

## Explaining compliance with consent mangagement

There are a web of options for being compliant with consent management tools like Cookiebot. These are best explained in person, and we're happy to talk through your options as needed.

### 1 - Who handles compliance?

There are two choices here, and multiple options for how it's done.

As many customers do, you could choose to handle compliance on your side. **This is the approach assumed in this document**. If you wish for us to manage compliance, please reach out to us.

If you do this, you are responsible for making sure our tag is written to the page post opt-in, and as quickly as possible for users who have opted-in. 

Alternatively, we could handle compliance. In this situation, you would put us in the head using our standard implementation described in [this document](../#tagging-options), and we would handle all compliance on our side, in our tag.

### 2 - How should the tag be written to the page?

Even with consent management the options described in [this document](../#tagging-options) apply. Both for page-load consideration (if user has opted-in), and what to do post opt-in. 

For example: "I want the tag to be written to the page Synchronously on page load if the user has opted-in so there's no content flickering, but if the user hasn't opted-in and does later, we want to just trigger the tag asynchronously and without page masking as they'll have already seen the old page". 

## Tagging options

In this document, to avoid confusion, we will only be providing a single tagging option here. Based on the above, if you wish to explore other options just reach out to us.

**This approach will have the following characteristics:**

- Fully synchronous if users have opted-in
- Asynchronous if users haven't, based on listening to the CookiebotOnAccept event.
- We want to check preferences=true as the criteria. If you wish to use other consent groups, adjust the code accordingly.
- Feel free to remove the comments - they are just there to explain the code to you. 

``` html
<script type="text/javascript">
// Make sure you replace MY-ACCOUNT-ID-HERE with your account ID.
var wto_tagurl = "//c.webtrends-optimize.com/acs/accounts/MY-ACCOUNT-ID-HERE/js/wt.js";

if(document.cookie.match(/CookieConsent=[^;]+preferences.true/)){
    // If user has already opted-in, write the tag immediately and synchronously.
    document.write('\x3Cscript type="text/javascript" src="' + wto_tagurl + '">\x3C/script>');
} else {
    // Else wait for the cookiebot-accept, check for our preferences flag, and then write the tag to the page asynchronously.
    window.addEventListener('CookiebotOnAccept', function(){
        if (Cookiebot.consent.preferences) {
            var sc = document.createElement("script");
            sc.type = "text/javascript";
            sc.src = wto_tagurl;
            document.head.appendChild(sc);
        }
    }, false);
}
</script>
```

## Read more:

- [Tag Implementation](../)
- [How the tag works](./how-the-tag-works/)
- [Impact on page load performance](./impact-on-page-load-performance)
