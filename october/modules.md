# October Modules

In a fresh install of an October site, there are three modules that basically make up what you know as the October CMS: System, Backend, and CMS. With so many pieces it's hard to know what's doing what. So let's lay them all out:

---

### The vendor packages

These are general composer packages and you'll generally find them in the `/vendor` directory.

#### [laravel/framework](https://github.com/laravel/framework) 

Without going too deep, it basically all starts with the Laravel framework. This provides the MVC setup, the IoC layer, helpers, configurations, database migrations, etc. **One important thing to note is that this is the [_framework_](https://github.com/laravel/framework) not the [_app_](https://github.com/laravel/laravel).** The app is one particularly way of using the framework. October uses the framework a different way. 


#### [october/rain](https://github.com/octobercms/library)

A basic Laravel site, is not a CMS. There are some things we need to tweak to the Laravel framework so we can add things like themes, plugins, modules, translations, templating engines, etc. That's what the `october/rain` package provides. It _does not_, you'll notice, provide a working web interface. It does not have routes, views, controllers, and things to run the CMS interface. This just makes that possible while still relying on Laravel. 


### The module packages

These are also composer packages, but because of [custom composer installers](composer_installer.md) these are not installed in the typical `/vendor` directory but added to the `/modules` directory instead. 

These three modules are the entryway into the site while maintaining the needs of the CMS. For example, in a Laravel you have a `routes.php` file somewhere that configures your routes/controllers. Well, that's dispersed among these three modules. 

Since these modules are under the `/modules` directory and not under the `/vendor` directory, you'd think that it should be ok to keep these modules in version control or even to modify these files to your liking. That would be a bad idea. Just like with other composer packages, you probably don't want to track them or modify them. Otherwise you're kind of defeating the purpose of dependency managers like composer in the first place!

#### [october/system](https://github.com/octoberrain/system)

The system module is October's hub of functionality. It's sort of the lowest level of the "CMS". It handles things like serving cached files/assets, logging events, plugin management, configuration management, and the like. 


#### [october/backend](https://github.com/octoberrain/backend)

The backend module is basically the web application for the backend interface. Nothing here gets invoked for normal requests to the site.


#### [october/cms](https://github.com/octoberrain/cms)

The CMS module is what converts the work you've done in the backend interface into something the end-user will see. This means rendering pages, components, themes, etc. 