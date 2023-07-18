# Consent Management - Simple

!!! note "Looking for something more flexible?"
    If the below does not provide the flexibility you're after, consider checking out [Consent Management - Advanced](../consent-management-advanced)

## What is our Simple integration?

If you want Optimize to only run when users have opted-in, this simple integration is likely for you. 

There are two ways to achieve this:

1. Embed Webtrends Optimize directly into your consent management system, and only write our tag to the page when users have opted in. 
    
    This is the most common route to a simple integration. If you adopt this route, you will not need to congigure anything in Optimize. 
    
    You should, however, expect users to see the old content pre-optin, and new content post-optin. 

    **OR:**

2. Abort the tag in Optimize if a user has not consented. 

    In the pre-init script, you can detect if a user has not consented and abort the tag on our side. 

    This is a reasonable solution if you **do not** have a Single Page Application, e.g. a site built with React or NextJS.

    To do this, you can detect the state of consent and then run the following code to abort:
    ``` javascript
    WT.optimizeModule.prototype.abort();
    ```

    The full logic could look something like:
    ``` javascript
    var hasOptedIn = document.cookie.match(/opt_in=true/i);
    if(!hasOptedIn){
        WT.optimizeModule.prototype.abort();
    }
    ```

With one of these two routes, you should achieve compliance. 

It's important to note the limitations of this approach, though. Users will see the old content and then post opt-in will be served new content. Observation Bias is a real problem, and if users know they are being experimented on they may well behave differently. This could invalidate your test results.

The best possible outcome is for test delivery pre-optin, and cookie/tracking to occur post-optin. This preserves the visual experience for users. You can learn more about it in the Advanced approach mentioned above.