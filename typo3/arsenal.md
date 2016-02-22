## The CIC Arsenal of TYPO3 Extensions

### Core Utilities, Helpers, and Tools

Here are several extensions available for making life a little easier when developing TYPO3 extensions. 

Extension | Description
--- | ---
[CICAccessor](https://github.com/castiron/cicaccessor) | Domain models are the heart of a typical TYPO3 extension. If you used the Extension Builder to scaffold your models, you undoubtedly have a long list of simple getters and setters cluttering up your models. **Get rid of them!** Boilerplate code gets in the way and can easily turn your simple model into hundreds of lines of code. 
[CICAPI](https://github.com/castiron/cicapi) | If you've ever built a TYPO3 extension that handled AJAX requests, you know there's a lot of boilerplate setup and some silly `typenum` shenanigans. This _very simple_ extension makes AJAX stuff a breeze handling cleaner routes (w/out realurl), route parameters, and connecting to extbase controller actions.
[CICBase](https://github.com/castiron/cicbase) | TYPO3 extension development would not be bearable without this little gem. It supplements the many missing features of ExtBase and TYPO3. Migrations, utilities, email handling, general fixes, FAL fixes, and abstractions. There is a lot in here and that's why it's a dependency of every one of these extensions and that's why it's found on every CIC wobsite.
[CICDispatch](https://github.com/castiron/cicdispatch) | This is easily one of the most useful extensions in the arsenal. A lot of web applications start out small, but then grow too big to be understood. Using this extension, you can split out behaviors into event listeners to handle your cross-cutting concerns.
[CICFixtures](https://github.com/castiron/cicfixtures) | Testing is a weak point in the TYPO3 world. This is perhaps because there isn't an easy way to mock up your domain models. This extension does a pretty good job of getting you to a place where you can test your application with real data using your complex models.
[CICMeta](https://github.com/castiron/cicmeta) | Many times, clients need more information from users than is required to build out the web application; information you'll probably never do anything with other than perhaps display it somewhere on the site. Since the site is already complicated enough, last thing you need is more model fields and more database columns and more clutter. Using this extension, you can easily compartmentalize that data into a single JSON field. 
[CICRest](https://github.com/castiron/cicrest) | Clients are always wanting to grab data from or send data to Salesforce and other external APIs. Because `guzzle` exists in the real world of PHP development (that is, the composer world) and not the infantile imagination of Kasper Skaarhoj, this handy extension is a great alternative to managing HTTP requests.
 
### Feature-based Extensions

These are extensions that you'd only use if you needed the specific feature that they provide. 

Extension | Description
--- | ---
[CICCal](https://github.com/castiron/ciccal) | Every organization wants a calendar on their website or, at least, a list of events. Anyone who's worked with date data knows it's not always a straightforward problem. This extension attempts to solve this once and be a re-usable tool for any site.
[CICRegister](https://github.com/castiron/cicregister) | Not every site requires frontend user registration. But when you need it, a lot of the grunt work of authenticating users, validating them, adding them to the right groups, and everything is provided by this extension. 
[CICOAuth](https://github.com/castiron/cicoauth) | OAuth is not a fun thing, but at least it's standardized. Here it is already implemented. Use it, fix it, add to it.
[CICResponsive](https://github.com/castiron/cicresponsive) | 
[CICStaticUrls](https://github.com/castiron/cicstaticurls) | 
[CICBlog](https://github.com/castiron/cicblog) [CICRSS](https://github.com/castiron/cicrss) [CICSlide](https://github.com/castiron/cicslide) [CICTwitter](https://github.com/castiron/cictwitter) | These are rather old, but still used and occasionally useful.

### Infrastructure Pieces

These peripheral elements do some of the heavy lifting to keep things running smoothly.

Extension | Description
--- | ---
[CICAssetPipeline](https://github.com/castiron/cicassetpipeline) | This extension provides viewhelpers for rendering CSS and JS assets into the frontend of a site based on a dist file and a manifest file.
[ClearTypo3Cache](https://github.com/castiron/cleartypo3cache) | Blasts your TYPO3 caches away, for that fresh, clean feeling! :sweat_drops:
[FluidHtml](https://github.com/castiron/fluidhtml) | TYPO3 comes with an HTML content element that is basically just for inserting HTML into a page. However, that's not sufficient for most projects. A lot of times, we need access to the power of fluid templates used in the rest of the site. 
[FluidPages](https://github.com/castiron/fluidpage) | In the beginning, there was typoscript. It was typoscript that told TYPO3 how to render your content, how to build the markup for your site, how to basically do everything. Don't know HTML? Fine, use typoscript instead. **--Yeah, no. That's a terrible idea.** This extension jumps off that ship as soon as possible and calls the fluid templating system to render layouts and templates for your pages. 