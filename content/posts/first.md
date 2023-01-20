---
title: "Fedora post install guide"
date: 2023-01-15T15:16:14+04:00
categories: [development, publishing]
tags: [hugo,content,static site generator]
---


This is a test  content

```python

def factorial(x):
    """This is a recursive function
    to find the factorial of an integer"""

    if x == 1:
        return 1
    else:
        # recursive call to the function
        return (x * factorial(x-1))



```

What you’ll learn here:

What a taxonomy is


How to configure taxonomies


How to use taxonomies to navigate your site’s categories and tags

Terminal
copied
# Starting repo
git clone https://github.com/CloudCannon/hugo-learn-blog-template.git

# Starting branch:
git checkout taxonomies-intro-start

# Finished branch:
git checkout taxonomies-intro-finish
What is a taxonomy?
The Hugo term “taxonomy” might sound a bit complicated, but it’s really just the things that your content has in common. Things like categories and tags on a post are considered taxonomies in Hugo.

As a user, you’re probably used to being able to click on a category or tag to find related content on a site. In fact, this is so common that Hugo supports “categories” and “tags” taxonomies by default - if you include these in your front matter, Hugo will automatically sort your pages into these groups, which you can then navigate to with the right layout setup. It’s also incredibly easy to set up your own taxonomies for navigation.

Setting up taxonomies and getting that right layout setup is exactly what we aim to do here!

How to create and manage taxonomies
This is the first time we are starting to look at global configuration. To configure taxonomies, or anything else for our site, we can adjust our config.toml file.

For now, the contents is just this:

copied
baseURL = ""
languageCode = "en-us"
title = "CawCannon"

[params]
  author = "CloudCannon"
We don’t need to do anything at all to configure categories and tags, but if we want to create our own custom ones, all we need to do is add a singular “key” with a plural “value” under a [taxonomies] key:

copied
# New "examples" taxonomy available
[taxonomies]
  tag = "tags"
  category = "categories"
  example = "examples"
That’s all there is to it. Hugo will take over the rest.

Note that we need to add the default taxonomies too, or otherwise Hugo will consider these disabled:

copied
# Now we will have no category taxonomy
[taxonomies]
  tag = "tags"
  example = "examples"
Practical work: using our default taxonomies
Creating the layouts

In the previous lesson, we added blog posts to content, which we linked to layouts. If you look at the blog posts, you will see that each post has associated tags and categories (feel free to add more or change them).

What we want to do here is use those tags/categories to create pages that allow us to navigate between content - by letting Hugo do its magic with specifically named taxonomy layouts. Fortunately, implementing this design with these pages is surprisingly easy. All you really need is a few layouts:

layouts/_default/term.html - a listing page for a specific term (e.g. specific tags/categories) under a taxonomy.
layouts/_default/taxonomy.html - a listing page for all terms that belong to a taxonomy.
Hugo knows what to do with these and will give you access to the generic .Data variable in these pages, which you can use to list all pages belonging to these groupings.

Let’s put this into practice. We already have tags and categories in our blog posts from the previous lesson, so Hugo will manage this data and assign .Type to terms and taxonomies. First, create layouts/_default/taxonomy.html and add this content to it:

copied
{{ define "main" }}
  <div class="container">
    <h1>All {{ .Type }}</h1>
    <ul>
      {{ range .Data.Pages }}
        <li>
          <a href="{{ .Permalink }}">{{ .Title }}</a>
        </li>
      {{ end }}
    </ul>
  </div>
{{ end }}
This will create a page for all taxonomies, listing all terms within it and page links to them. You will now be able to navigate to /tags to see this.

Next, we want to add this content to layouts/_default/term.html:

copied
{{ define "main" }}
  <div class="container">
    <h1>All posts for {{ .Type | singularize }} "{{ .Title }}"</h1>
    <ul>
      {{ range .Data.Pages }}
        <li>
          <a href="{{ .Permalink }}">{{ .Title }}</a>
        </li>
      {{ end }}
    </ul>
  </div>
{{ end}}
With the same example as above, this will create a page showing, for example, <h1>All posts for tag "Conservation"</h1> and list all posts relating to this tag.

Now, with very little effort on our part, we can let Hugo create pages for our taxonomies, giving our site greater power of navigation that you might expect in a dynamic website.

With the same example as above, this will create a page showing, for example, <h1>All posts for tag "Conservation"</h1> and list all posts relating to this tag.

Now, with very little effort on our part, we can let Hugo create pages for our taxonomies, giving our site greater power of navigation that you might expect in a dynamic website.

Adding the navigation
Now that we’ve created the layouts for our terms and taxonomies, it’s probably nice to be able to use them. Fortunately, this is now incredibly easy - we only have to create a link to these specific pages, such as /tags/<tag name>. Let’s do this in our blog page layout, so that any tags (or categories if you prefer) in a post allow us to navigate to related content. Place this in layouts/blog/single.html where there is an HTML comment (you can also do this on the index page):

copied
<div class="tag-group">
  {{ range .Params.tags }}
  <div class="tag">
    <span class="tag-text">
      <a href="/tags/{{. | urlize }}">{{.}}</a>
    </span>
  </div>
{{end}}
</div>
Now, inside a post, if we click on any tags at the top of the post, we’ll be directed to our term.html page listing all related posts.

We aren’t using our taxonomy.html page explicitly, but it is available if we navigate to “/tags” or “/categories” in our browser. If we want them available, we can simply add links to layouts/partials/nav.html (just for demonstration purposes - we could use them more meaningfully in landing pages, posts, etc.):

copied
<a href="/tags">Tags</a>
<a href="/categories">Categories</a>
What's next?
With taxonomies, Hugo gives us an incredibly simple way - with very little code - to manage related content on our site, making it more powerful and seemingly “dynamic”.

We are slowly improving our pages and understanding how Hugo gives us the power to create amazing sites, without having to set up any infrastructure. Next, let’s improve our blog pages even more with two great Hugo features: page bundles and shortcodes.