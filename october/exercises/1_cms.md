### 1. CMS

Go to the backend `/backend`, log in (`admin` / `admin`), and click on the CMS tab up top. Check out the other tabs if you want to, but we're going to focus on the CMS for now.

**Basic page operations**

* Create a new page `/about`.
* Add html content to it.
* Review the client side.
* Try different options like with/without a layout.

**Twig**

* Use some basic twig tags: `{{ this.page.id }}`, `{{ this.page.title }}`

**Real Files**

At this point, if you leave the backend CMS interface and check out the command line and do a `git status` you'll see there's a new `.htm` file matching the page you just created. It looks a little different than it does in the backend, but it's essentially the same.

Notice how it's in the "demo" theme and the directory structure of this theme is relatively straightforward.

What would happen if you had created a nested page with a url like `/about/bio`? Try doing this so that the `bio.html` file is in a directory called `/about`.

**Partials**

So, we haven't done anything outside of the CMS interface yet. Let's keep it that way for now.

* Create a new partial in a directory called "bios" and with the name "me".
* Add some content: "My name is..."
* Include your bio partial from the `/about/bio` page. (Hint: Look at the twig markup in other pages to see how they include partials.)
* Review your work on the `/about` page.

For fun, what happens if you require the partial `bios/me.html` instead of `bios/me.htm`?

**Content Blocks**

Reading the documentation, it looks like content blocks are nearly the same as partials, but what's the difference?

**Layouts**

By now, you can tell that creating and using layouts is rather straight forward. It'd be tedious to create a new layout and apply to our `about` pages. If it's not obvious, try that out now.

Even still, the demo theme's default layout is worth checking out, especially the twig tags.

* How does the layout know which page is active?
* How are the navigation links generated?
* Try adding your `/about` page to the default layout navigation.

**Assets**

* Add an image of yourself (or just some image) to your `/about` page.

**Components**

We haven't created our own components or anything. But the demo theme has a Todo component we can work with.

* Try adding that to your `/about` page to see what happens. To do this, you can actually just click on the component from the sidebar drop-menu while you're editing the `/about` page. Once you do this, you'll see the little Todo info box show up on your page in the backend.
* Check out the demo theme's "Plugin components" page to see how to include this component on your `/about` page within the markup.

---

**_Commit or stash your changes before continuing_**

---
