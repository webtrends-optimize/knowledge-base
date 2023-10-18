# Segment by Geo Location

You can segment by the user's location in Webtrends Optimize, or use these values as part of your test code by accessing the values with JavaScript.

We use Cloudflare, one of the world's largest networks, as the engine for doing these lookups.

**Note:** We will need to enable this for you - we default to downloading as little data as possible and so don't add these values for customers unless required. Please email us at support@webtrends-optimize.com to get it enabled.

## Available Values

The following values are available - here are the names and descriptions.

| Attribute               | Data Type | Description |
| ----------------------- | --------- | ----------- |
| _wm_TimeOffset | integer | Timezone Offset, in milliseconds
| _wm_city | string | The user's city.
| _wm_continentCode | string | Two-character continent code.<br>E.g. EU
| _wm_countryCode | string | Two-character country code.<br>E.g. UK
| _wm_countryName | string | Full Country Name as per ISO specification
| _wm_dcCompanyName | string | Domain controller company name<br>E.g. Vodafone UK
| _wm_latitude | decimal as string | Latitude
| _wm_longitude | decimal as string | Longitude
| _wm_postalCode | string | First block of postal code<br>E.g. NW1
| _wm_regionName | string | Region Name. In the UK, this will be England, Scotland, etc. In the US, this will be the State.
| _wm_timezone | string | Timezone name.<br>E.g Europe/London

## Accessing geolocation information with JavaScript

You can access this geolocation information as part of your test. You'll find the information in the Optimize Data Layer. 

``` javascript
WT.optimizeModule.prototype.preinit.wtConfigObj.data.attribute_name
```

You could use this value to dynamically create an experience. For example changing the URL of a banner:

``` javascript
var region = WT.optimizeModule.prototype.preinit.wtConfigObj.data._wm_regionName;
var image;

if(region === "England") image = "banner_england.png";
else if(region === "Scotland") image = "banner_scotland.png";

document.getElementById('my-banner').src = image;
```