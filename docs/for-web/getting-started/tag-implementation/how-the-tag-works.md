## How it works – serving experiences, behavioural tracking

![WTO Tag Loading Process](/assets/tag-loading-process.png)

1. Your users request the web page, in the same way they do today.
2. Your server returns a web page, in the same way it does today.
3. That web page contains our Tag, before any visual elements. This triggers as the page loads in the browser. At this point, we mask the page (optionally, to avoid flickering).
4. A request is made to Optimize to see if there is anything on the page that needs to executed (a test/target) or tracked (clicks, page loads, revenue, etc.
5. Our servers respond with instructions of transformations to make to the page (with Javascript and CSS), and things to track. These are executed when the response is handled on the page.
6. The optional masking is removed, and the final page is ready for a user

This experience may include additional steps for Single Page Apps, and depending on how you handle Consent Management. 