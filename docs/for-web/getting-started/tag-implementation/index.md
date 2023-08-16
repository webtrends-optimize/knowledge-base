# Tag Implementation

!!! info "Tagging on Shopify?"

    Check out our guides for [Shopify](/getting-started/tag-implementation/shopify/) or [Shopify Plus](/getting-started/tag-implementation/shopify-plus/).

## TLDR

This is a highly detailed document. A summary is as follows.

We have two key methods for tag implementation. 

1. Adding our normal HTML tag to the head of your site.
2. Adding a hybrid async tag to the head of your site. 

Either of these are fine, and both will allow you to test, personalise and track behaviour with zero content flickering.

The file you run will be a lightweight Loader file with some core methods in it, and we will then download and run tests based only on what a user is eligible for. If it's not on that page or the user doesn't belong to that audience, we typically don't return that test code.

Deviating from our suggested options leaves you at risk of things like content flickering and poorer CLS scores. 

Full details of what the tag is, how it works, and options available to you are below.

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

!!! info "Content Security Policies"

    You may also need to update your Content Security Policies (CSPs) - details are found at [Content Security Policies](/for-web/getting-started/tag-implementation/content-security-policies/).

## Tagging options

We have the following options for tagging 

- Fully Synchronous 
- Hybrid Asynchronous
- Fully Asynchronous (not recommended)

Details of what they are, why they're used, and potential limitations are discussed below.

### Tagging option 1 - Fully Synchronous

#### What is the Fully Synchronous tag?

This is our regular tag, and the most simple to implement. You'll find a single remote script reference (to c.webtrends-optimize.com) as illustrated above.

Implementing this tag in the head allows us to perform all actions required.


``` html
<script type="text/javascript" src="//c.webtrends-optimize.com/acs/accounts/abcdefgh-0123-ijkl-4567-mnopqrstuvqz/js/wt.js"></script>
```

For it's intended use, you should not be adding async or defer attributes to this.

#### Why is the Fully Synchronous tag used?

This format of tag has been used for well over 10 years with Optimize.

The tag is the easiest to understand and implement, with only a single script reference to worry about.

Being a synchronous tag, it is a fraction faster to deliver tests to the page, as tag responses won't land in a queue waiting for the browser to process them - it'll handle it the moment it comes back.

#### What potential limitations are there to the Fully Synchronous tag?

There are two key limitations to the Fully Synchronous tag.

Render-blocking - Synchronous Javascript holds up the processing of anything that comes after it, and so if you have a request-heavy website, it's going to take a fraction longer to action these other things. 

Vulnerable to no-responses - If for some reason your tag, which is hosted on Azure CDN, does not respond at all (as opposed to responding with errors), this could hold your website up indefinitely. This is the case with all synchoronous tags, not just ours, but is a potential vulnerability that some choose to address.

#### Example of Fully Synchronous tag

You can find an example of this implemented at [https://www.webtrends-optimize.com/lp/competitor-tag-testing-a/wto-sync/](https://www.webtrends-optimize.com/lp/competitor-tag-testing-a/wto-sync/){target=_blank}.

You can choose to run tests on this page by adding the query string **?runtest=true**

You can also choose how to make the page:

- **?mask=none** - no masking
- **?mask=element** - only mask the h1 element
- **?mask=page** - mask the entire page

### Tagging option 2 - Hybrid Asynchronous

#### What is the Hybrid Asynchronous tag?

This is a more recent version of our tag. It's an asynchronous tag, i.e. non-blocking, but with masking until loaded/failed. This approach therefore has the visual impact of a synchronous tag (nothing shown to users at least until the tag fires), but does allow the rest of the page to continue loading in.

It addreses some of the concerns of our default approach, but does carry it's own challenges/tradoffs too.

``` html
<script type="text/javascript">
!function(){
  var src = "//c.webtrends-optimize.com/acs/accounts/abcdefgh-0123-ijkl-4567-mnopqrstuvqz/js/wt.js";
  var timeout = 2000; // 2 seconds

  var css={add:function(c, id){if(c instanceof Array){c=c.join(' ')}var a=document.getElementsByTagName('head')[0],b=document.createElement('style');b.type='text/css';if(id){b.id=id;}if(b.styleSheet){b.styleSheet.cssText=c}else{b.appendChild(document.createTextNode(c))}a.appendChild(b)}, del:function(id){var el=document.getElementById(id); if(el){el.parentNode.removeChild(el)}}};
  var cssid = 'wt_tagHide';
  css.add('body { opacity: 0.000001 !important; }', cssid);

  var sc = document.createElement('script');
  window.WT_ABORT = 0;
  sc.src = src;
  sc.onload = function(){ window.WT_ABORT = -1; css.del(cssid); }
  sc.onerror = function(){ window.WT_ABORT = 1; css.del(cssid); };

  document.getElementsByTagName('head')[0].appendChild(sc);

  setTimeout(function(){
    if(window.WT_ABORT !== -1) window.WT_ABORT = 1;
    css.del(cssid);
  }, timeout);

}();
</script>
```

#### Why is the Hybrid Asynchronous tag used?

For websites which have a lot of external content to load, it's sometimes useful to allow it to do so while we fetch and execute tests.

It also comes with the added benefit of timeouts, and so is not vulernble to no-responses. 

#### What potential limitations are there to the Hybrid Asynchronous tag?

This tag isn't an improvement for all websites. We've seen it have a great impact on some websites, but it should be tested for your approach if you're not sure which is best for you.

A large pre-init section, or very request-heavy websites may struggle with this approach.

#### Example of Hybrid Asynchronous tag

You can find an example of this implemented at [https://www.webtrends-optimize.com/lp/competitor-tag-testing-a/wto-async-with-masking/](https://www.webtrends-optimize.com/lp/competitor-tag-testing-a/wto-async-with-masking/){target=_blank}.

You can choose to run tests on this page by adding the query string **?runtest=true**

You can also choose how to make the page:

- **?mask=none** - no masking
- **?mask=element** - only mask the h1 element
- **?mask=page** - mask the entire page

### Tagging option 3 - Asynchronous / Delayed

Some companies choose to implement Optimize in a fully asynchonus manner. The tag fill function in this manner - javascript will typically execute perfectly fine if run a little late - but you should note step 3 below, which would take place too late if the tag is async.

#### What is an Asynchronous / Delayed tag?

... and why should it be avoided:

This is where usres take our usual tag, and do one of the following:

- Add an async attribute, which stops blocking the page and therefore hinders our ability to mask the page before it's revealed to users.
- Add a defer attribute, which delays execution of the tag until the browser feels there's sufficient bandwidth.
- Write the tag out with javascript instead of as a HTML tag, which for anything other than document.write makes it async by default and therefore we're unable to protect against content flicker & CLS.
- Put the tag inside a tag management container, which may itself be asynchronous but will likely write tags to the page via. javascript and so with a delay too.

## Choosing the right approach.

There is no alternative to trying it and working out what works best for you.

You do not need to necessarily do this through dev cycles with the IT team - there are plenty of Chrome Extensions and applications that can modify web page content, such as Resource Override or Fiddler. These require some developer skill, but you may find the outcome is faster for understanding impact on metrics like CLS to do so this way than to run various tags through dev cycles and understand performance in non-production environments.

## Read more:

- [How the tag works](./how-the-tag-works/)
- [Impact on page load performance](./impact-on-page-load-performance)
