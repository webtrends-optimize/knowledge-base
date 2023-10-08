---
glightbox: false
---

# Integrations

<style>
.integration-tiles { display: flex; flex-wrap: wrap; gap: 20px; }

.integration-tiles > a { flex: 1 1 calc(33% - 40px); color: inherit; text-decoration: none; text-align: center; border: 1px #eee solid; font-size: 85%; }

.integration-tiles > a > div:first-child { border-bottom: 1px #eee solid; padding-bottom: 10px; height: 150px; background: #f0f0f0 url() center center no-repeat; background-size: contain;     display: flex; align-items: center; justify-content: center; }

.integration-tiles > a > div:first-child img { max-height: 100px; max-width: 80%; }

.integration-tiles > a > div:nth-child(2) { line-height: 30px; }

</style>

## Push

These integrations send data **from Webtrends Optimize** into 3rd party platforms. They're focused on web, and the sending of experiment data specifically (project alias, test id, experiment id).

<div class="integration-tiles">
<script>

var list = [
    'Accoustic Tealeaf :: accoustic-tealeaf.svg :: ./push-integrations/accoustic-tealeaf',
    'Adobe Analytics :: adobe-analytics.png :: ./push-integrations/adobe-analytics',
    'Amplitude :: amplitude.svg :: ./push-integrations/amplitude',
    'AT Internet :: at-internet.svg :: ./push-integrations/at-internet',
    'Contentsquare :: contentsquare.svg :: ./push-integrations/contentsquare',
    'Fullstory :: fullstory.svg :: ./push-integrations/fullstory',
    'Glassbox / Sessioncam :: glassbox.svg :: ./push-integrations/glassbox',
    'Google Universal Analytics :: gua.png :: ./push-integrations/google-universal-analytics',
    'Google Analytics 4 (GTM) :: ga4.svg :: ./push-integrations/google-analytics-4-gtm',
    'Google Analytics 4 (GTAG) :: ga4.svg :: ./push-integrations/google-analytics-4-gtag',
    'Heap Analytics :: heap.svg :: ./push-integrations/heap-analytics',
    'Hotjar :: hotjar.svg :: ./push-integrations/hotjar',
    'Medallia / Decibel Insights :: medallia.svg :: ./push-integrations/medallia',
    'Microsoft Clarity :: microsoftclarity.png :: ./push-integrations/microsoft-clarity',
    'Mixpanel :: mixpanel.svg :: ./push-integrations/mixpanel',
    'Quantum Metric :: quantum-metric.svg :: ./push-integrations/quantum-metric',
    'Salesforce CRM :: salesforce.svg :: ./push-integrations/salesforce',
    'Tealium :: tealium.svg :: ./push-integrations/tealium',
    'Twilio Segment.io :: segmentio.png :: ./push-integrations/segmentio',
];

list = list.sort().map(x => {
    let [name, img, link] = x.split(' :: ');

return (
`<a href="${link}">
    <div><img alt="${name}" src="/assets/vendors/${img}"></div>
    <div>${name}</div>
</a>`
);

}).join('');

console.log(list);
document.write(list);

</script>
</div>

### Create your own 

You can create your own integrations - here are guides depending on your role.
<div class="integration-tiles">
<a href="./push-integrations/create-your-own-push-integration-user">
    <div>&nbsp;</div>
    <div>As a user</div>
</a>
<a href="./push-integrations/create-your-own-push-integration-vendor">
    <div>&nbsp;</div>
    <div>As a vendor</div>
</a>
</div>

## Pull

These integrations address the consumption of data from third party systems **into Webtrends Optimize**. 

### External Audiences 

We can pull audiences from 3rd party platforms into Webtrends Optimize.

<div class="integration-tiles">
<script>

var list = [
    'Bloomreach Data Layer :: bloomreach.png :: ./pull-integrations/bloomreach-data-layer/',
    'GTM Data Layer :: gtm.png :: ./pull-integrations/gtm-data-layer/',
    'Google Analytics 4 Segments :: ga4.png :: ./pull-integrations/ga4-audiences/',
];

list.sort().forEach(x => {
    let [name, img, link] = x.split(' :: ');

document.write(
`<a href="${link}">
    <div>&nbsp;</div>
    <div>${name}</div>
</a>`
);
});

</script>
</div>

### Conversion Tracking

We can pull events from 3rd party platforms into Webtrends Optimize.

<div class="integration-tiles">
<script>
var list = [
    'GTM Event Mirroring :: gtm.png :: ./pull-integrations/gtm-events-mirroring/',
];

list.sort().forEach(x => {
    let [name, img, link] = x.split(' :: ');

document.write(
`<a href="${link}">
    <div>&nbsp;</div>
    <div>${name}</div>
</a>`
);
});

</script>
</div>

