# Content Security Policies

## What are Content Security Policies?

Content security policies are a modern security feature, which tell the browser which domains are allowed to operate on your website, and what permissions they're allowed.

They are specified either on the **Request Headers** (most common) for the document, or by using the **Meta Tag**. e.g.:

HTTP Header example: 

``` http
Content-Security-Policy: default-src 'self'; img-src https://*; child-src 'none';
```

Meta Tag example:

``` html
<meta
  http-equiv="Content-Security-Policy"
  content="default-src 'self'; img-src https://*; child-src 'none';" />
```

## Adding Webtrends Optimize to your Content Security Policies

!!! note "Use of 'all' and default"

    Many websites opt to use "all" and default as an option for some rules, in which case you may not need to specify our domain explicitly. For example, default-src is an acceptable fallback for listing all of script-src, connect-src, style-src and img-src.

Webtrends Optimize has 3 domains from which we operate, depending on what you're doing with us. These are:

- `*.webtrends-optimize.com` - for core services 
- `*.webtrends-optimize.workers.dev` - for custom libraries and geolocation services
- `*.azure-websites.net` - for custom services, e.g. recommendations, social proofing. **Note:** we recognise that the open domain carries more risk, and if you are able to respond quickly, we can provide you with precise subdomains for the custom work we do as services are created.

### The full set of permissions required, for all services

```
script-src: 'unsafe-eval' 'unsafe-inline' *.webtrends-optimize.com, *.webtrends-optimize.workers.dev
script-src: *.azurewebsites.net
connect-src: *.webtrends-optimize.com, *.webtrends-optimize.workers.dev, *.azurewebsites.net
style-src: 'unsafe-inline' *.webtrends-optimize.com, *.webtrends-optimize.workers.dev
img-src: *.webtrends-optimize.com
```

### Simpler permissions listing

!!! warning "Warning: Poorer security"

    While many websites use default-src, it does carry poorer security / more risk than the more granular approach listed above. We list the option here given how many of our customers use it.

If you use default-src rules, you can add us into these directly too.

```
default-src: 'unsafe-eval' 'unsafe-inline' *.webtrends-optimize.com, *.webtrends-optimize.workers.dev, *.azurewebsites.net
```