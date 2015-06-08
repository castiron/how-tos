### 4. Static Pages Plugin

* Install the plugin from the marketplace. 
	* You'll need to log in to the marketplace.
	* You'll need to set your project ID in the October CMS
	* In the marketplace, add the plugin to your project
	* Then in October, sync with the marketplace by updating the plugins

Just try this:

http://octobercms.com/blog/post/getting-started-static-pages

**Converting plugin component to a Static Pages snippet**

We created the `showPost` component earlier. Let's just make it a snippet so that non-technical users can drag it into the RTE without much confusion. 

We have this:

```php
public function registerComponents() {
    return [
      'Castiron\Dogs\Components\Dog' => 'showDog'
    ];
}
```

Now just add this:

```php
public function registerPageSnippets() {
    return [
      'Castiron\Dogs\Components\Dog' => 'showDog'
    ];
}
```

Bam. Boom. Done. 

Anyone can now just drag your component to the Static Pages RTE and use your component. Cool?

On top of that, users can also configure the component as needed. 
