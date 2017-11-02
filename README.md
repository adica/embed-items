# embed-items

* introduction - where I work, what we doing there.
* what we did until now - `iframe` with `src`, the source is on our servers, show code snnipet
* what is the problem with classic iframe embed - 
 * very slow 
 * not responsive desgin (js nedded)
 * hard to do SEO
 * `post-messages` are very slow
 * `iframe` asstets are prioritize very low on browser quoue
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
