# Create your own push integration - For Vendors

*Are you a vendor, considering your options for an integration with Webtrends Optimize where a/b testing views can be sent to your platform? If so, you're in the right place.*

## Overall goal

Most analytical platforms want as many data points as possible. Alongside what people are doing on the website / app, you'd like to know things about the user. In our case, this includes which experiments they're falling into. 

Our goal for sending you data is that this aids exploration in your platform - powering your filters, segments, dimensions etc. so that users can see your features with the lens of people who fell into specific tests/variations.

## The complexities we need to consider

We typically find it far easier to send experiment views to platforms, than for you to try and intercept this data. 

There are a few reasons for this:

- Timing: Our platform may not exist on the page at the time you try to call down on functions.
- Interpreting data: We have certain data we'd like to send in a consistent format, but have the ability to tailor what's sent. This is far easier handled by us, than you trying to pull apart our methods to find the information you need.
- Delayed execution: By building our own functions, we can make sure things like delayed entry to our tests are accounted for. Users might choose to only trigger a test on click or scroll, which could be several seconds into page load. This allows us to retain some form of quality control. 
- Building out our integration library: We want to have as many platforms showcased as possible, and have some interesting future plans for how to push your platforms to our customers. E.g. Smaller websites with no session recording platforms could be advertised X.

## Option 1 (recommended) - WTO-led

In Webtrends Optimize, we have a library of integrations. We would look you add you as a vendor here.

For users, the net result would be a  push-button integration, handled in our platform. For example:

![Toggle Switch Integration](/assets/integrations-new-vendor-toggleswitch.png)

### Required details

To have this approach, we need the following details (we are happy to work with you to produce this):

- Your logo - this will be used on a white background.
- Your name
- A short description of your platform
- What your platform does, so we can categorise you
- The JavaScript we would need to trigger to send data to your platform. 
    - Please consider the following parameters we could send you:
        - **project_alias** - a slightly friendlier name of the test.
        - **test_id** - a numeric ID for the test/target/baseline.
        - **experiment_id** - a numeric ID for the control/variation group they saw.
    - Please keep in mind that users can see multiple experiments as part of their session, so we suggest avoiding session/user scoped attributes. 
- A longer description - this can include videos, a description of your platform, and ideally screenshots of what the user will experience in your platform. 

![Integrations - Full Details](/assets/integrations-fulldetails.png)

### Example output

An example output of what we could expect is:

- Logo: Attached file
- Name: My Insights Platform
- Short Description: My Insights Platform allows users to study users on their website, seeing their journeys through recordings.
- Categories: Session Recording
- JS Code:
    ``` javascript
    _mip_track = _mip.track || [];
    _mip.track.push({
        project_alias: {{project_alias}},
        test_id: {{test_id}},
        experiment_id: {{experiment_id}}
    });
    ```
- A longer description - most easily shared in an email, which we can copy the structure and format of. 

Once you have this information, feel free to send it along to sandeep.shah@webtrends-optimize.com and we'll get it uploaded for you.

## Option 2 - Vendor-led

You are able to pick up on events in Webtrends Optimize using:

``` javascript
WT.addEventHandler("pageview", function(e){
    // users have entered a test 

    var params = e.target.params;
});
```

Within "params", you are then able to pick up on the following paramters:

- `testAlias`: the project alias value. Usually in the format of ta_1HomepageUSPs.
- `r_runID`: the test ID
- `r_experimentID || r_personalisedID || "BASELINE"`: an ID for the variation users saw, whether from a Test, Target or Baseline.

Taking the My Insights Platform example above, the full code could look like:

``` javascript
WT.addEventHandler("pageview", function(e){
    var params = e.target.params;
    var projectAlias = params.testAlias,
        testID = params.r_runID,
        experimentID = params.r_experimentID || params.r_personalisedID || "BASELINE";

    _mip_track = _mip.track || [];
    _mip.track.push({
        project_alias: projectAlias,
        test_id: testID,
        experiment_id: experimentID
    });
});
```

You would also need to make sure `window.WT` exists - there are multiple ways to do this, such as polling.

If you choose to take this approach, we would still love to hear about what you've created so we can help promote it.