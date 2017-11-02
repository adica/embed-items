# embed-items

* introduction - where I work, what we doing there, and what is the problem with the embed.
* what we did until now - `iframe` with `src`, the source is on our servers
* what we would do in the future - `web components` short intoduction
* freindly iframe 
  * what is it - A friendly iframe is an iframe that shares the same domain as the main page it is hosted on (an iframe without `src`).
  * pros
    * faster
    * seo
    * higher cpm (?)
    * ease the parent-child communication (no slow `post-message`)
    * part of the page
    * get higher priority in page
  * cons
    * cookies, localstorage
    * hard to convince partners
    * CORS
    * harder to develop
