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

**Editing Models in the Backend**

After creating our model, you saw that it created the model class, some yaml files, and what looks like a migration. 

`updates/version.yaml` - **This is not just some changelog.** This is actually how you trigger certain database changes. Check out the [docs](http://octobercms.com/docs/database/structure#update-files) to properly adjust your plugin versions. 

`updates/create_<model>_table.php` - The migration. You do not run this like a normal Laravel migration. Instead you must update your version.yaml file, then trigger the update of your plugin either from the CLI or the BE. **It's not obvious how this works. So ask someone.**

`models/<model>/fields.yaml` - Used in model forms.

`models/<model>/columns.yaml` - Used in lists of models.

Just having the model defined is not enough to start creating records. You must also have the necessary backend controller (which you probably want anyway). We already did this above. 

The generated controller, basically has everything we need. In order to start working with our model in the backend, you must add some navigation which looks a little like this:

```php
    public function registerNavigation()
    {
        return [
            'posts' => [
                'label'       => 'Posts',
                'url'         => \Backend::url('castiron/posts/posts'),
                'icon'        => 'icon-asterisk',
                'permissions' => ['castiron.posts.*'],
                'order'       => 100,
            ]
        ];
    }

```

The important takeaway from this code is that we haven't defined any routes and yet the October BE will figure out how to invoke your controller and list/edit/modify your models. That's sort of built in by default. 

All of this is perfectly customizable, but you get the basics. 

**Components**

todo