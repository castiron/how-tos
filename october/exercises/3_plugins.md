### 2. Plugins

This is a broad topic, so let's narrow it down:

* plugin vs. component
* how does laravel fit in? 
* reusability
* models for frontend and backend

Let's make a boring blog plugin. We'll have some Post models and list them and such. So fun, right?

```
php artisan create:plugin CIC.Blog
```

You should follow along in the documentation to get fuller details about what's what, like to get a description of the `Plugin.php` file we just created. It **seems pretty important**, since it's the only php file that was generated.

Let's make more stuff for our plugin. We need a way to list our posts on the frontend, right? So we'll make a component that can be added to any page from the CMS. We also need a controller and a model. 

```
# save our work so we can see what changes 
# are made by the next commands
add plugins/
git commit -m 'created CIC.Blog plugin'


# run, review, save
php artisan create:component CIC.Blog Post
add .; git commit -m 'created CIC.Blog Post component'

# run, review, save
php artisan create:model CIC.Blog Post
add .; git commit -m 'created CIC.Blog Post model'

# run, review, save
php artisan create:controller CIC.Blog Posts
add .; git commit -m 'created CIC.Blog Post controller'
```

Oh boy. We just did a lot of things. 

