## Introduction

It would be very easy to say "we're great, don't worry", but we appreciate that's not going to make anyone in your IT department any happier. So, below you'll find some detailed thoughts on performance for web, and where our platform fits in.

Firstly, note that any JavaScript, whether jQuery, webpack-bundled React Components, or indeed your Experimentation platform, can affect page load performance. Minimal, lightweight pages are encouraged by google, and anything you add to a page (additional sections to your website, additional functionality, or 3rd party vendors) all detract from ideal page load speeds.

When considering page load performance, we typically consider the metrics that Google Lighthouse allows us to report on. These will be discussed below.

## DOM Content Loaded, First Contentful Paint – “load speed”

This considers how quickly the page loads in and gets content onto the page. For these metrics, the weight of our tag, and in-memory footprint is important.

To minimise our footprint here and keep our tag as small as possible, we:

Write concise code in ES5, and not transpiling – something that our competitors do. Using transpiled code adds unnecessary bloat – something others don’t consider when their tag is already considerably large.

Avoid using libraries, which often contain a plethora of features that you’ll never need.

A loader – dependancy mechanism. We have a minimal loader, and subsequent requests are pulled in as dependancies. This allows the rest of the page to continue to load, whilst we make decisions about the user and execute everything we need to – something which is better for initial footprint than having a very large file that’s blocking the critical path to content loading.

## Cumulative Layout Shift (CLS)

Cumulative Layout Shfit (CLS) is now believed to contribute 25% of your overall performance score. This metric considers content shifting the page, such as new banners moving all elements below it once loaded.

With experimentation platforms, content flickering, polling and delayed rendering of content can all exacerbate this problem. 

Webtrends Optimize implements on-page masking, which ensures nothing is shown to the user until the final experience is ready. You have the ability to set this for the website as a whole, or on a per-page basis. And, for whichever elements you're interested in masking. This means there should be no jumpy or jittery experience at all. 

Infact, if your website loads in this jumpy manner, adding the Webtrends Optimize tag may actually improve your CLS score, as shown with some internal studies we've conducted. 

## Closing thoughts

We experiment and personalise our own website. As part of our process, we discovered that our website itself, without any additional software, had poor performance. By buidling a website with great performance scores – 95+ – we noticed that our tag had minimal-to-no impact. Even when running tests, our scores remain at 90+, often 100.

In comparison to other platforms, we hope you'll find our tag to importantly be slimmer, fast and to have less impact on the metrics you care about.

We have, and continue to, find any opportunity to reduce our footprint though, and come up with solutions which ensure solid onsite experience which reflects in good performance scores. Practices such as definitively dealing with content flickering go a long way to ensuring this.