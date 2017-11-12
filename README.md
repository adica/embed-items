# embed web pages with friendly iframe

In the past year, I'm working at [playbuzz](https://www.playbuzz.com) - a platform that helps publishers and bloggers to create more engaging content.

After our users use the [creator](https://publishers.playbuzz.com/create-with-playbuzz/) to create their stories, they need to embed it on their sites. To do that we use a [classic iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe). that is the most common way to embed items in web pages, and it's been used on many sites for almost 20 years now.

## Embed with a good old iframe
To embed with `iframe` we give the user a code snippet that looks something like that:

```js
<script src="//cdn.playbuzz.com/sdk.js"></script>
<div class="pb_feed"
   data-item="xxx-xxx-xxx">
</div>
```

When the `sdk.js` script run - it builds an `iframe` on the host page and refer its `src` attribute to the relevant item on `playbuzz` servers.

```html
//host page
<html>
   <head>...</head>
   <body>
      ...
      <div class="pb_feed" data-item="xxx-xxx-xxx">
         <iframe src="//www.playbuz.com/item/xxx-xxx-xxx">
            
            //iframe content from playbuzz servers
             <html>
               <head>...</head>
               <body>...</body>
            </html>
            
         </iframe>
      </div>
      ...
   </body>
</html>
```

That way - we at `playbuzz` control the entire `iframe` page. the host side does not care how we handle it, and there is a separation between the `host` and the `iframe` page.

While this method works for many years on many sites (`youtube` still use it for example), it had many flows:
* the way to communicate between `iframe` and the host (`post-messages`) is very slow.
* `iframe` assets are prioritized very low on browser queue - the browser will allocate resources to the `iframe` page only after the host page, and it will slow the `iframe` page render.
* `iframe` is not responsive, so every time the host page changes its layout (user switch from portrait to landscape mode for example) the `sdk.js` need to know about it somehow, and change the `iframe` layout as well.
* it's hard to optimise the SEO - google will index the page like its `playbuzz` page, and our partners would like it to be index on their domain.
 
## Embed with friendly iframe 
A `friendly iframe` is another way to embed webpages on host sites. basically, `friendly iframe` is an `iframe` that shares the same domain as the main page it is hosted on (an iframe without `src`).

We use the same code snippet as before, but this time we build an `iframe` without `src` attribute, and then inject our item page (`head` and `body`) content into this `iframe`. That way we get the same result, but now without any reference to `playbuzz` servers on the host site.

```html
//host page
<html>
   <head>...</head>
   <body>
      ...
      <div class="pb_feed" data-item="xxx-xxx-xxx">
         <iframe>
            
            //iframe page content is part of the host page
            <html>
               <head>...</head>
               <body>...</body>
            </html>
            
         </iframe>
      </div>
      ...
   </body>
</html>
```

## Friendly iframe pros
* because we are now just another part of the host page DOM, our page will render faster
* SEO is now as our partners want it to be - google index the page on host domain.
* ease the parent-child communication - now we don't need the slow `post-message` anymore, we can use `window.parent` to call the host (we could not access parent when our page was in a different domain).
* now we get the same priority as the host page when the browser allocates resources to render assets
    
## Friendly iframe cons
* cookies and localstorage are now shared with the parent, so we can't use `playbuzz` own cookie cross sites (we use [xdomain](https://github.com/contently/xdomain-cookies) library to solve this issue)
* CORS - we need to configure our servers to handle calls from multiple domains instead of `playbuzz` domain only
* it's harder to develop `freindly iframe` then regular `iframe` - we need to take care of the content injection, cookies issue, and configure CORS on our servers
* it's can be hard to convince host sites to break the isolation that regular `iframe` holds


## Conclusion
While `classic iframe` is much easier to implement and can fit many cases, in some situations like when the SEO is important, or if you need to use parent-child communication many times, it is better to use `friendly iframe`.
Anyway - I think you should definitely know this two options and the difference between them so you could decide which better for your case.
