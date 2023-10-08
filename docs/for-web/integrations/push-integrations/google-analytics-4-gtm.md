# Google Analytics 4 via. GTM - Push Integration

This document describes how to send experiment views into GA4, which is sent via. GTM's dataLayer object.

With the integration enabled. Each page view will generate an event “optimize_view”. The parameters of this event include IDs that will allow a visitor be identified as being exposed to a specific test and experiment variation.

An example is illustrated below.

![GA4 - GTM Config - Console Output](/assets/ga4-gtm-console.png){style=max-width:300px;display:block;}

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
        'project_alias': `{{eventCategory}}`,
        'test_id': `{{eventAction}}`,
        'experiment_id': `{{eventLabel}}`
    });
    ```

2. Save and close. 

Your integration will take some time to go live - usually less than 10 minutes. 

Note this goes into your tag, which has a 24hr cache. You will need to hard-refresh (ctrl+shift+r / cmd+shift+r) to see an uncached page download.

## Google Tag Manager Configuration

With the datalayer being populated we need to take the following steps in Google Tag Manager to ingest the data. 

This can be achieved by creating a single **Tag**, a single custom **Trigger** and **3 Variables**

The navigation on the side allows the user to generate and review each of these GTM objects.

![GA4 - GTM Config - 1](/assets/ga4-gtm-gtmconfig-1.png){style=max-width:400px;}

### Step 1 - Create Variables 

Variables are required to warn GTM in advance that we'll be sending parameters that need collecting.

We'll set these up first, so we can then reference them in the event capture.

We will be using **Page Variable > Data Layer Variable** as the Variable Type, and the following names both for the **GTM Variable Name** and **Data Layer Variable Name**:

- project_alias
- test_id
- experiment_id

![GA4 - GTM Config - Variables - project_alias](/assets/ga4-gtm-gtmconfig-variables-projectalias.png){style=max-width:400px;display:block;margin-bottom:10px;}
![GA4 - GTM Config - Variables - test_id](/assets/ga4-gtm-gtmconfig-variables-testid.png){style=max-width:400px;display:block;margin-bottom:10px;}
![GA4 - GTM Config - Variables - experiment_id](/assets/ga4-gtm-gtmconfig-variables-experimentid.png){style=max-width:400px;display:block;margin-bottom:10px;}

Once set up, the User Defined Variables should have these entries:

![GA4 - GTM Config - Variables - Complete](/assets/ga4-gtm-gtmconfig-variables-complete.png){display:block;}

### Step 2.1 - Create the Tag

![GA4 - GTM Config - Tags - Settings](/assets/ga4-gtm-gtmconfig-tags-newtag.png){display:block;}

1. Go to Tags, and create a new one. 
2. Give it any useful name, such as "GA4 - optimize_view"
3. Click onto the Tag Configuration block to edit it
4. Choose the **Tag Type** of **Google Analytics GA4 Event**
5. Enter your **Measurement ID**
6. Enter the **Event Name** of **optimize_view**
7. Provide the following Event Parameters:
    - **project_alias** is **{{project_alias}}**
    - **test_id** is **{{test_id}}**
    - **experiment_id** is **{{experiment_id}}**

    Note that the variables, wrapped in curly braces, should show up in the autocomplete if you've set the variables up properly.

8. Click save to close the flyout.

### Step 2.2 - Define the Trigger

![GA4 - GTM Config - Tags - New Trigger](/assets/ga4-gtm-gtmconfig-tags-newtrigger.png){style=max-width:500px;display:block;}

We'll now define when this tag fires. 

1. Still in the same section as above, scroll down to Triggering
2. Click on the block to edit it's properties. This will launch the "Choose a trigger" flyout.
3. In the top-right corner, click the + icon to create a new Trigger.
4. Give it any useful **name**, such as **GA4 - optimize_view**.
5. Click on the Trigger Configuration block, scroll down in the flyout, and select the **Trigger Type** of **Custom Event**.
6. For **Event Name**, write **optimize_view**.
7. Save the trigger.

### Step 2.3 - Save and finish

At this point, your tag should look like this.

![GA4 - GTM Config - Tags - Complete](/assets/ga4-gtm-gtmconfig-tags-complete.png){display:block;}

1. Click Save in the top-right corner, to save the tag. 
2. Preview your tag if needed
3. Click Submit in the top-right. 
4. Click Publish. 

#### Step 3 - Validating the change

Once published and the snippet updates, you'll be able to see events firing into the datalayer in the console *as you fall into tests*. 

![GA4 - GTM Config - Console Output](/assets/ga4-gtm-console.png){style=max-width:300px;display:block;}

## Configuration in Google Analytics

We're sending 3 variables that you'll want to define as dimensions in GA4. This will allow you to explore that data in reports you generate.

### Go to Custom Dimensions admin


1. Click on Admin
2. Click on Custom Definitions

![GA4 - Custom Definitions screen](/assets/ga4-gtm-ga4-step1.png){style=display:block;}

### Create 3 custom dimensions

Once on the right screen, we want to create custom dimensions with the following properties:

1. **Dimension Name:** project_alias
    **Scope:** Event
    **Description:** Project Alias for Webtrends Optimize
    **Event Parameter:** project_alias
2. **Dimension Name:** test_id
    **Scope:** Event
    **Description:** Test ID for Webtrends Optimize
    **Event Parameter:** test_id
3. **Dimension Name:** experiment_id
    **Scope:** Event
    **Description:** Experiment ID for Webtrends Optimize
    **Event Parameter:** experiment_id

Once completed, the screen should look like this:

![GA4 - Custom Definitions - Done](/assets/ga4-gtm-ga4-step2.png){style=display:block;}

## Finish

That's it. You can now explore reports in GA4, using Webtrends Optimize parameters as dimensions.

Note: It may take 24hrs for data to show up.

![GA4 - GTM Config - Console Output](/assets/ga4-gtm-done.png){style=display:block;}

## Need help debugging your implementation? 

We work with some excellent agencies who can help you debug and fix your analytics implementation if needed. 