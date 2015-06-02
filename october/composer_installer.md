# Composer Installers

So composer exposes an [API](https://getcomposer.org/doc/articles/custom-installers.md) for doing stuff when your composer package is included elsewhere, when it is installed. 

If you think about it, it's kinda cool. You could do something like write a plugin for some framework and have it installed where the framework expects it (like, outside the `vendor` directory). All the while, your plugin is still installed via composer. It's still it's own package. 

Not going to win an Oscar, but it works when you need it. 

### Submodule or Composer Installer?

Let's say you have an events plugin for October. To make it reusable as a plugin for other October sites, you want to keep this in it's own repository. 

You could simply add it as a submodule in the `/plugins` directory. And that's it. Goal achieved.

But now you want to share your events plugin with the world. Well, not everyone digs submodules the way you do. And most people are not really going to be doing much with your plugin other than using it. So the submodule solution is no good. The composer method is actually just as good. 

October makes this easy, too. The hard stuff is already done for you [here](https://github.com/composer/installers/blob/master/src/Composer/Installers/OctoberInstaller.php). Now in your plugin's `composer.json` you do something like

```
{
	"name": "castiron/events",
	"type": "october-plugin", 
	"require": {
		"composer/installers": "~1.0"
	}
}
``` 

That's it.


Even if you didn't want to share your plugin with the world. You could still use the composer installer to place it in the correct directory. And then in whatever project you want to use it, you require it locally the various ways that composer provides.
