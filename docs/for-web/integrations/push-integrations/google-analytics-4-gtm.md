**THIS DOCUMENT IS UNDER CONSTRUCTIONS**

# Google Analytics 4 via. GTM - Push Integration

This document describes how to send experiment views into GA4, which is sent via. GTM's dataLayer object.

With the integration enabled. Each page view will generate an event “optimize_view”. The parameters of this event include IDs that will allow a visitor be identified as being exposed to a specific test and experiment variation.

An example is illustrated below.

![GTM Datalayer](/assets/ga4-gtm-datalayer.png){style=max-width:500px;}

The specific IDs and Alias reconcile with what is published for the specific test in the Webtrends Optimize Dashboard.

![Optimize UI Dashboard - Experiment IDs](/assets/wop-dashboard-expids.png)

## Configuration in the Webtrends Optimize UI

You must enable and configure an integration in the Optimize UI. This will emit the dataLayer events whenever users fall into a test and trigger a "pageview" event. 

We will use the current GA/GTM integration, and swap out some content.

### 1. Enable the current GA integration

1. Navigate to Manage > Integrations
2. Find the GA integration, and Install it. 
3. As with all integrations, select the tag/s you wish to apply it to, and flip the toggle switch to ON.

### 2. Replace content

1. Hop over to the Code section, and replace the existing code with the following:

    ``` javascript
    window.dataLayer = window.dataLayer || [];
    window.dataLayer.push({
        'event': 'optimize_view',
        'event_params': {
                'project_alias': `{{eventCategory}}`,
                'test_id': `{{eventAction}}`,
                'experiment_id': `{{eventLabel}}`
        }
    });
    ```

2. Save and close. 

Your integration will take some time to go live - usually less than 10 minutes. 

Note this goes into your tag, which has a 24hr cache. You will need to hard-refresh (ctrl+shift+r / cmd+shift+r) to see an uncached page download.

## Google Tag Manager Configuration

With the datalayer being populated we need to take the following steps in Google Tag Manager to ingest the data. 

This can be achieced by creating a single **Tag**, a single custom **Trigger** and **3 Variables**

The navigation on the side allows the user to generate and review each of these GTM objects.

1 - Generate a new Tag. Choose the tag type “Google Analytics GA4 Event”:

2 - Set the **configuration Tag**: 

In our example this is **“GA4 Configuration - All Pages Functional Cookies”**

Set the Event Name: In our example we call this **“optimize_view”**

3 - Next create a custom **Trigger**.

Click on **Trigger** target, and then the **“+”** icon to generate a new trigger.


Select **“Custom Event”**

If you are following our convention, label the Event Name as **“optimize_view”**

Save the Trigger.

4 - Next Create 3 new **User Defined Variables**

Choose **“Data Layer Variable”** as variable type

Label the variable **"experiment_id”**.

For the Data Layer variable Name enter, event_params.experiment_id

Repeat this for the other 2 event attributes, 

**“project_alias”**,  event_params.project_alias

**“test_id”**,  event_params.test_id

