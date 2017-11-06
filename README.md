# embed web pages with friendly iframe

In the past year, I've been working at [playbuzz](https://www.playbuzz.com), as part of the `story` team. we build a platform that helps our partners to create more engaging content.

After they use our [creator](https://publishers.playbuzz.com/create-with-playbuzz/) to create their pages, they need to embed it on their sites. To do that we use a simple `iframe`. that is the most common way to embed items in web pages, and it's been used on most sites for almost 20 years now.

When one of our partners finish to editing this page we give him a code snippet that looks something like that:

```js
<script src="//cdn.playbuzz.com/widget/feed.js"></script>
<div class="pb_feed"
   data-item="xxx-xxx-xxx">
</div>
```

When the `feed.js` script run - it build an `iframe` on the host page and refer its `src` attribute to the relevant item on `playbuzz` servers.

That way - we control the entire `iframe` page, the host side does not care how we handle it, and there is a separation between the `host` and the `iframe` page.

While this method works for many years for many sites (`youtube` still use it for example), it had many flows:
 * `iframe` assets are prioritized very low on browser queue
 * `iframe` is very slow 
 * not easy to create pages with responsive design (js needed, can't be done with CSS only)
 * hard to do SEO - the pages index like its `playbuzz` pages instead of the host.
 * `post-messages` (a way to communicate between `iframe` and the host) are very slow.
 
TODO:
 
* freindly iframe 
  * what is it - A friendly iframe is an iframe that shares the same domain as the main page it is hosted on (an iframe without `src`).
  * usage code snipet
  * pros
    * faster
    * seo
    * higher cpm (?)
    * ease the parent-child communication (no slow `post-message`)
    * part of the page
    * get higher priority in page
  * cons
    * cookies, localstorage (shared with parent if you don't use xdomain lib)
    * hard to convince partners
    * CORS
    * harder to develop
* performance demo - old vs new (video?)
