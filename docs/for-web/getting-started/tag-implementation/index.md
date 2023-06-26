# Tag Implementation

!!! info "Tagging on Shopify?"

    Check out our guides for [Shopify](/getting-started/tag-implementation/shopify/) or [Shopify Plus](/getting-started/tag-implementation/shopify-plus/).

## What is the Webtrends Optimize Tag?

The Webtrends Optimize tag is the javascript tag you add to your website, which allows us to interface with it. It is typically a single SCRIPT tag, and when added into the HEAD tag of your website allows us to run experiments + optimals, track user activity, and provide all other experiences we build.

This is used for client-side experimentation and personalisation. We have alternative implementation guides for Server-Side, Mobile app, IoT etc.

## How to get your Tag

You can find a full tagging guide in the Optimize UI, found in the top nav. 
![Navigate to Tagging Guide](/assets/nav-taggingguide.png)

The details convered in the UI include guidance on placement, Content Security Policies and Tag Management containers.

There is also a downloadable PDF version, which you can easily hand over to your dev team.

![Tagging Guide](/assets/tagging-guide.png)

Broadly, as mentioned above, this will involve placing one SCRIPT tag into the HEAD of your website (or at least the pages you wish to test and track on). You may also need to consider adding our domains to your content security policies.

![Tag Placement](/assets/tag-placement-beforecloseofhead.png)

## How it works – serving experiences, behavioural tracking

![WTO Tag Loading Process](/assets/tag-loading-process.png)

1. Your users request the web page, in the same way they do today.
2. Your server returns a web page, in the same way it does today.
3. That web page contains our Tag, before any visual elements. This triggers as the page loads in the browser. At this point, we mask the page (optionally, to avoid flickering).
4. A request is made to Optimize to see if there is anything on the page that needs to executed (a test/target) or tracked (clicks, page loads, revenue, etc.
5. Our servers respond with instructions of transformations to make to the page (with Javascript and CSS), and things to track. These are executed when the response is handled on the page.
6. The optional masking is removed, and the final page is ready for a user

This experience may include additional steps for Single Page Apps, and depending on how you handle Consent Management. 

## Impact on page load performance

It would be very easy to say "we're great, don't worry", but we appreciate that's not going to make anyone in your IT department any happier. So, below you'll find some detailed thoughts on performance for web, and where our platform fits in.

Firstly, note that any JavaScript, whether jQuery, webpack-bundled React Components, or indeed your Experimentation platform, can affect page load performance. Minimal, lightweight pages are encouraged by google, and anything you add to a page (additional sections to your website, additional functionality, or 3rd party vendors) all detract from ideal page load speeds.

When considering page load performance, we typically consider the metrics that Google Lighthouse allows us to report on. These will be discussed below.

### DOM Content Loaded, First Contentful Paint – “load speed”

This considers how quickly the page loads in and gets content onto the page. For these metrics, the weight of our tag, and in-memory footprint is important.

To minimise our footprint here and keep our tag as small as possible, we:

Write concise code in ES5, and not transpiling – something that our competitors do. Using transpiled code adds unnecessary bloat – something others don’t consider when their tag is already considerably large.

Avoid using libraries, which often contain a plethora of features that you’ll never need.

A loader – dependancy mechanism. We have a minimal loader, and subsequent requests are pulled in as dependancies. This allows the rest of the page to continue to load, whilst we make decisions about the user and execute everything we need to – something which is better for initial footprint than having a very large file that’s blocking the critical path to content loading.

### Cumulative Layout Shift (CLS)

Cumulative Layout Shfit (CLS) is now believed to contribute 25% of your overall performance score. This metric considers content shifting the page, such as new banners moving all elements below it once loaded.

With experimentation platforms, content flickering, polling and delayed rendering of content can all exacerbate this problem. 

Webtrends Optimize implements on-page masking, which ensures nothing is shown to the user until the final experience is ready. You have the ability to set this for the website as a whole, or on a per-page basis. And, for whichever elements you're interested in masking. This means there should be no jumpy or jittery experience at all. 

Infact, if your website loads in this jumpy manner, adding the Webtrends Optimize tag may actually improve your CLS score, as shown with some internal studies we've conducted. 

### Closing thoughts

We experiment and personalise our own website. As part of our process, we discovered that our website itself, without any additional software, had poor performance. By buidling a website with great performance scores – 95+ – we noticed that our tag had minimal-to-no impact. Even when running tests, our scores remain at 90+, often 100.

In comparison to other platforms, we hope you'll find our tag to importantly be slimmer, fast and to have less impact on the metrics you care about.

We have, and continue to, find any opportunity to reduce our footprint though, and come up with solutions which ensure solid onsite experience which reflects in good performance scores. Practices such as definitively dealing with content flickering go a long way to ensuring this.