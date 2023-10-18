# Segment by Data Layer values

## What is the Optimize Data Layer / Data Object?

If you want to feed additional data into Optimize, there are several ways of doing it – both using out of the box connectors and custom connectors.

The Optimize Data Layer, also known as the Data Object, is a Javascript-driven method of feeding extra data to Optimize, based on your own logic or variables you scrape off the page. In the Optimize UI, you will find areas when building Locations and Segments where you can provide your Name/Key, and build logic around Values.

The data layer is found as an object, at `WT.optimizeModule.prototype.wtConfigObj.data`. You can add keys/values to this object in the pre-init section, so that they're available for both Locations and Segments.

## Using the Optimize Data Layer

### Using the Optimize Data Layer in Locations
![Data-Layer-Lookups-in-Location-Objects](/assets/Data-Layer-Lookups-in-Location-Objects.png)

### Using the Optimize Data Layer in Segments
![Data-Layer-Lookups-in-Segment-Objects](/assets/data-layer-matching-for-segments.png)

## What kind of data can you feed in?
You can feed in Strings, Integers and Decimal/Floats. If you wish to feed in an object or array, flatten it first.

For example, change: `{“arr”:[“001”, “002”]}` to `{“arr[0]”:”001”, “arr[1]”:”002″}`

## How much data can you feed in?

All data you feed in is carried on each request, as decisions take place on our servers and not the browser. Whilst there is **no hard limit** to how much data you can pass in, the larger these payloads become, the slower your calls will become. We therefore suggest limiting the data to only what you need, as performance is an important factor in what we all do.

## When can I add data into the Data Layer?

**On page load**
You can add your collection code into the Pre-Init section of your [Tag Context]. By adding it here, it will be available for all evaluation we do (locations, segments).

**After a delay**
If you wish to wait for something to become present (i.e. the content appears after the Optimize tag fires), refer instead to [Handling Pageviews] in the [Optimize Build Framework]. You do not need to feed the data into the Data Layer – just use JavaScript to perform whichever checks you’d like, and decide on the fly whether or not you want someone to enter and be counted.

**On the fly**
If you want to capture custom metrics on the fly, refer instead to [Custom Data Collection].

## Additional use – Custom Data Collection
If you create a Custom Data Field for your test, where the name/key matches an item in the Data Layer, this will automatically be captured for all Views and Conversions that take place within that load of Optimize. An example of this would be to scrape and feed in the Order Value on a Confirmation page, so that any metrics you then capture on the Confirmation page will have the Order Value recorded against them.

Note that there are two places you can attach custom data to your tests:

**Automatic Conversions Section (Global rule)**

![automated-tracking-custom-data](/assets/automated-tracking-custom-data.png)

**Advanced Editor – Conversions Section (Individual rule).**

![ae-in-custom-data](/assets/ae-in-custom-data.png)

## Examples

### Example – Retail PDP – Product Metadata Capture

Let’s say you’re working on [this page](https://www.jdsports.co.uk/product/white-adidas-originals-beckenbauer/16006265/) on JD Sports, and you see the [Schema.org](http://schema.org/) markup before the Optimize (as we suggest it be implemented):

``` json 
{
    "@context": "http://schema.org",
    "@type": "Product",
    "id": "16006265",
    "name": "adidas Originals Beckenbauer",
    "color": "white",
    "image": ["https://i8.amplience.net/i/jpl/jd_361488_a?qlt=92&w=300&h=300&v=1","https://i8.amplience.net/i/jpl/jd_361488_b?qlt=92&w=300&h=300&v=1","https://i8.amplience.net/i/jpl/jd_361488_c?qlt=92&w=300&h=300&v=1","https://i8.amplience.net/i/jpl/jd_361488_d?qlt=92&w=300&h=300&v=1","https://i8.amplience.net/i/jpl/jd_361488_e?qlt=92&w=300&h=300&v=1","https://i8.amplience.net/i/jpl/jd_361488_f?qlt=92&w=300&h=300&v=1"],
    "sku": "16006265",
    "description": "Rep iconic style from adi's classic archives with these men's Beckenbauer trainers from adidas Originals. In a white colourway with bold red accenting, these trainers are made with a premium leather upper and come sat on a responsive midsole with a grippy gum outsole to keep you stepping on the streets. With a secure, tonal lace-up fastening for a locked in fit, these sneakers feature adi's signature 3-Stripes to the sidewalls and are finished with premium Beckenbauer Allround gold branding to the sidewalls with a Trefoil on the heel.",
    "category": "Men / Mens Footwear / Trainers / Classic Trainers",
    "offers": {
        "@type": "Offer",
        "url": "https://www.jdsports.co.uk/product/white-adidas-originals-beckenbauer/16006265/",
        "priceCurrency": "GBP",
        "price": 70.00,
        "availability": "http://schema.org/InStock",
        "itemCondition": "http://schema.org/NewCondition",
        "seller": {
            "@type": "Organization",
            "name": "JD Sports"
        }
    },
    "brand": {
        "@type": "Thing",
        "name": "adidas Originals",
        "url": "https://www.jdsports.co.uk/men/brand/adidas-originals/"
    }
}
```

And, let’s say we want to capture the `sku`, `offers.price` and `brand.name`.

To do this, we could insert code similar to this into our Pre-Init script:

``` javascript
// Using an anonymous function to contain variables
(function(){
    // first capture and parse the JSON-LD script tag:
    var o = JSON.parse(jQuery('script[type*="application/ld+json"]:contains("@type": "Product")').text());

    // pass in each property we're interested in to the Data Layer
    WT.optimizeModule.prototype.wtConfigObj.data.sku = o.sku;
    WT.optimizeModule.prototype.wtConfigObj.data.price = o.offers.price;
    WT.optimizeModule.prototype.wtConfigObj.data.brand = o.brand.name;
})();
```

That’s it. The data will instantly be available to use for building more personal experiences, both in Locations and Segments.

### Example – Travel List Page – Booking Details Capture

Let’s say you’re working on the Matrix page for GWR, where the URL looks somewhat like:

Let’s say you’re working on the Matrix page for GWR, where the URL looks somewhat like:

<https://www.gwr.com/tickets/#/3087/3271/2020-06-25/D12:00/2020-06-25/D20:00/1/0/0/1/1/N/N/N>

Where the hash parameters are:

- Origin station code
- Destination station code
- Outbound Journey Date
- Outbound Journey Time
- Return Journey Date
- Return Journey Time
- \# of Adults
- \# of Children
- etc.

Let’s say we wanted to build a personal experience where we target family journeys with at least 1 adult and 1 child with a new promotional message.

Our code in Pre-Init could look like:

``` javascript
// Using an anonymous function to contain variables
(function(){
    var p = location.hash.split('/'); // Split the hash into parts 

    // Feed the relevant parts into the tag
    WT.optimizeModule.prototype.wtConfigObj.data.adults = p[7];
    WT.optimizeModule.prototype.wtConfigObj.data.children = p[8];
})();
```

We could then build a Segment for our Target that looked like:

![segments-dataobject-familybooking](/assets/segments-dataobject-familybooking.png)