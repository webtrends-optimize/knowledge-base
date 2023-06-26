# Roadmap

## Last 5 Releases

### Jun 12, 2023 - UI Release 66

Features included:

- Bug ENG-23474 - Locations always saved as regex
- Enhancement ENG-23031 - Faster login
- Enhancement ENG-23044 - Faster loading of projects and tests
- Enhancement ENG-23088 - Faster loading of meta tables and segments
- Enhancement ENG-23153 - Faster loading of advanced editor
- Enhancement ENG-23451 - Faster loading of script content
- Enhancement ENG-23451 - Faster loading of script content

Some of these have been released to internal beta for real-user verification and will be released to everyone in the near future.

Read more at: [UI Release 66](/roadmap/past-releases/2023-12-06-r66)

### May 30, 2023 - UI Hotfix 2023.8

Features included:

- Feature ENG-23575 - Push integration for Contentsquare
- Feature ENG-23574 - Push integration for Fullstory

These were two initial features for what will be a stream of activity as we upgrade our Integrations library with dozens of new push-button integrations.

Read more at: [UI Hotfix 2023.8](/roadmap/past-releases/2023-05-30-h2023-8)

### May 24, 2023 - UI Hotfix 2023.7

Features included:

- Feature ENG-23575 - Push integration for Contentsquare
- Feature ENG-23574 - Push integration for Fullstory

Remove access to MS SQL Server Reports, Non-Binomial Data Extract formatting fix & bug fixing.

Read more at: [UI Hotfix 2023.8](/roadmap/past-releases/2023-05-24-h2023-7)

### May 16, 2023 - UI Hotfix 2023.6

Details TBC.

Read more at: [UI Hotfix 2023.6](/roadmap/past-releases/2023-05-16-h2023-6)


### Apr 26, 2023 - UI Release 65

Features included:

- Features ENG-23445, ENG-23444, ENG-23443, ENG-23442, ENG-23441, ENG-23332 - Clickhouse reporting. 

This is the biggest shift we've had for the Optimize platform since moving from a UI built in Flash to one in HTML.

The culmination of the past 2 years of R&D to migrate our data warehouse from Microsoft SQL Server to Clickhouse, allowing us to ingest data in under 1 minute, report on billions of rows, and produce most reports sub-second. 

Read more at: [Milestone - Clickhouse](/roadmap/past-releases/milestone-clickhouse)

## Upcoming Releases (next 2 months)

We have the following in active sprints, with planning and design complete.

### UI Relase 67:

#### Reporting

- ENG-23469 - SRM Warnings - making these available and clear our various reporting screens.
- Numerous bugs and speed improvements for Clickhouse reports.

#### UI Performance and Stability

- ENG-23490 - Faster preview link generation
- ENG-23493 - Faster dashboard carousel loads

#### 3rd Party Intergrations

- ENG-23564 - Push Integration - Accoustic Tealeaf 
- ENG-23566 - Push Integration - Segment 
- ENG-23568 - Push Integration - Microsoft Clarity 
- ENG-23578 - Shopify - Pull Product Feed 
- ENG-23583 - Consent Management - Simple

#### UI Features

- ENG-23482 - Allow use of localhost in Sites - the product is capable of supporting this. The feature allows it to be exposed to users. Added based on customer demand.
- ENG-23396 - Reintroduction of session usage based on Clickhouse data

### UI Relase 68:

#### 3rd Party Integrations:

- ENG-23561 - Push Integration - Hotjar
- ENG-23562 - Push Integration - Mosueflow
- ENG-23563 - Push Integration - Piwik Pro
- ENG-23565 - Push Integration - Sessioncam 
- ENG-23567 - Push Integration - Mixpanel 
- ENG-23569 - Push Integration - Heap Analytics 
- ENG-23570 - Push Integration - Amplitude 
- ENG-23571 - Push Integration - AT Internet 
- ENG-23579 - Push Integration - GA4 via. GTM 
- ENG-23580 - Push Integration - GA4 direct 
- ENG-23581 - Pull Integration - GA4 via. Bigquery
- ENG-23584 - Consent Management - Advanced
- ENG-23590 - Consent Management - Onetrust - Simple
- ENG-23591 - Consent Management - Onetrust - Advanced
- ENG-23585 - SPA - Simple 
- ENG-23586 - SPA - Advanced 
- ENG-23587 - _wt.disabled query string - from Journey Further
- ENG-23588 - Abort tag for known bots
- ENG-23594 - Pull Integration - Bloomraech data layer
- ENG-23595 - Shopify - Extensibility UI - New tag

#### Bug fixes:

- ENG-23550 - Long URLs in the dashboard bleed into other columns
- ENG-23543 - URLs in the dashboard run into eachother

#### UI Features:

- ENG-23549 - Multi-page testing for Visual Editor
- ENG-23508 - User controls for Multi-Armed Bandit
- ENG-23483 - Dashboard - New previewing
- ENG-22830 - Advanced editor - New previewing

#### Reporting:

- ENG-23513 - Publicly shareable reports
- ENG-23544 - Tags on tests

## Long Term Vision (next 6 months)

We recognise the following to be gaps or opportunities in the Optimize platform, which we will be addressing over the next 6 months. Whilst ambitious, we've proven our ability to deliver at pace, and are growing the team to exceed expectations.

### Reporting

Now that we have migrated onto Clickhouse, we can commit to phase 2 of our reporting suite, targeted at **Automated Insights**. We will be able to scan hundreds of user attributes and journeys, and point you to where the insights are. This should be a considerable time-saver for Analysis, where finding insights is the most time-consuming part of the job.

We will then loop-back, and augment our data collection with more user profile data, and allow you to **filter on and apply dimensions of your own custom attributes**.

### AI Personalisation

We will be progressing our application of AI in Optimize in 3 core areas:

- **Predictive modelling**: Predicting the likelihood of a user achieving a given outcome. E.g. purchasing, abandoning, etc.
- **Content delivery**: Applicable to both product/content recommendations and to AB/n and MVT tests, we'll be allowing users to provide vast amounts of unclassified data, and we will make sense of what to show which users based on these attributes you provide.
- **Test outcomes**: Applying machine learning to uncover insights for you.

### 3rd Party Integrations

As the above demonstrates, we have a **long list of push-button integrations** we will be building for the platform. These are all capabilities today, and so the simplified versions will be delivered into the product at pace.

### Developer experience 

We are reconsidering how to best fit into the flow that developers typically adopt - particularly alongside tools like Github. We will be embedding ourselves deeply into this stack, therefore allowing developers to **build tests without ever logging into the platform**. All with version control history, branching, staging environments etc. in place.

### Help website

As customers continue to join us from sunsetting tools like Google Optimize, Maxymiser, from other vendors, and as our agency programme grows at pace, we have recognised the need for a better organised help website. 

This **new Knowledge Base** is the new home for all help content, and we will be growing the articles posted here.

With this in place, users will be able to better support themselves with all aspects of the platform, with freshly-written content.