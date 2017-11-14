# embed web pages with friendly iframe

During the past year, I'm working at [playbuzz](https://www.playbuzz.com) - a platform that helps publishers and bloggers to create more engaging content.

After our users use the [creator](https://publishers.playbuzz.com/create-with-playbuzz/) to create their own stories, they embed it into their sites. For doing that we use a [classic iframe](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe). This is the most common way to embed items into web pages, and it has been used on many sites for more than 20 years.

## Embed with a good old iframe
To embed with `iframe` we give the user a code as follow:

```js
<script src="//cdn.playbuzz.com/sdk.js"></script>
<div class="pb_feed"
   data-item="xxx-xxx-xxx">
</div>
```

When the `sdk.js` script run - it builds an `iframe` on the host page and refer its `src` attribute to the relevant item on the `playbuzz` servers.

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

In this way `playbuzz` controls the entire `iframe` page. The host side does not care how we handle it, and there is a separation between the `host` and the `iframe` page.

While this method works well for many years on many sites (`youtube` still uses it for example), it had many flows:
* the communication between `iframe` and the host (`post-messages`) is very slow.
* `iframe` assets are prioritized very low on the browser queue - the browser will allocate resources to the `iframe` page only after the host page, slowing the `iframe` page render.
* `iframe` is not responsive - every time the host page changes its layout (user switch from portrait to landscape mode for example) the `sdk.js` should be anonunced about it somehow, to change the `iframe` layout as well.
* it is hard to optimise the SEO - google will index the page as a `playbuzz` page, while our partners would like it to be index under their own domain.
 
## Embed with friendly iframe 
A `friendly iframe` is another way to embed webpages into host sites. Basically, `friendly iframe` is an `iframe` that shares the same domain as the main page it is hosted on (an iframe without `src`).

We use the same code snippet as before, but this time we build an `iframe` without `src` attribute, and then inject our item page (`head` and `body`) content into this `iframe`. This way we get the same result, but now without any reference to `playbuzz` servers on the host site.

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
* now our content is just another part of the host page DOM, our page will be rendered faster
* SEO is now as our partners want it to be - google index the page under their host domain.
* the parent-child communication is easier - we don't need the slow `post-message` anymore, we can use `window.parent` to call the host (we could not access the parent when our page was in a different domain).
* our resources get the same priority as those of the host page when the browser allocates resources to render assets
    
## Friendly iframe cons
* cookies and localstorage are now shared with the parent, so we can't use `playbuzz` own cookie cross sites (we use [xdomain](https://github.com/contently/xdomain-cookies) library to solve this issue)
* CORS - we need to configure our servers to handle calls from multiple domains instead of `playbuzz` domain only
* it's harder to develop `freindly iframe` then regular `iframe` - we need to take care of the content injection, cookies issue, and configure CORS on our servers
* it's can be hard to convince host sites to break the isolation that regular `iframe` holds


## Conclusion
While `classic iframe` is much easier to implement and can fit many cases, there are some situations, e.g., when the SEO is important, or when you need to use parent-child communication many times, when it is better to use `friendly iframe`.
Anyway - I think you should definitely be aware of these two options, and the difference between them, so you could decide which is better for your case.
