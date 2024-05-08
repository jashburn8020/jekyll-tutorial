# Jekyll Tutorial

## Setup

### Installation

- [Install Ruby](https://jekyllrb.com/docs/installation/).
- Install Jekyll from the terminal:

```bash
$ gem install jekyll bundler
```

- Create a new `Gemfile` to list your project's dependencies:

```bash
$ bundle init
Writing new Gemfile to /path/to/jekyll/Gemfile
```

- Edit the `Gemfile` in a text editor and add jekyll as a dependency:

```text
gem "jekyll"
```

- Run `bundle` to install jekyll for your project.

```bash
$ bundle
Bundle complete! 1 Gemfile dependency, 31 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.

$ bundle info jekyll
  * jekyll (4.3.3)
	Summary: A simple, blog aware, static site generator.
	Homepage: https://jekyllrb.com
	Source Code: https://github.com/jekyll/jekyll
	Changelog: https://github.com/jekyll/jekyll/releases
	Bug Tracker: https://github.com/jekyll/jekyll/issues
	Path: /path/to/gems/gems/jekyll-4.3.3
```

- You can now prefix all jekyll commands listed in this tutorial with `bundle exec` to make sure you use the jekyll version defined in your `Gemfile`.

### Create a site

- Create a new directory for your site
- Create [`index.html`](tutorial_site/index.html) in your site root directory.
- Note: You can also create a new Jekyll site by running the `jekyll new <PATH>` command.
  - E.g., `jekyll new myblog` creates a new Jekyll site at `./myblog`.
  - Jekyll will install a site that uses a gem-based theme called Minima.
  - See "Themes" section below.

### Build

- Run either of the following commands to build your site:
  - `jekyll build` - Builds the site and outputs a static site to a directory called `_site`.
  - `jekyll serve` - Does `jekyll build` and runs it on a local web server at <http://localhost:4000>, rebuilding the site any time you make a change.
    - To force the browser to refresh with every change, use `jekyll serve --livereload`.
    - Not suitable for deployment.

```bash
$ jekyll serve
Configuration file: none
            Source: /path/to/jekyll/tutorial_site
       Destination: /path/to/jekyll/tutorial_site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.005 seconds.
 Auto-regeneration: enabled for '/path/to/jekyll/tutorial_site'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

### References

- _Setup_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/01-setup/>

## Directory Structure

- A basic Jekyll site usually looks something like this:

```text
.
├── _config.yml
├── _data
│   └── members.yml
├── _drafts
│   ├── begin-with-the-crazy-ideas.md
│   └── on-simplicity-in-technology.md
├── _includes
│   ├── footer.html
│   └── header.html
├── _layouts
│   ├── default.html
│   └── post.html
├── _posts
│   ├── 2007-10-29-why-every-programmer-should-play-nethack.md
│   └── 2009-04-26-barcamp-boston-4-roundup.md
├── _sass
│   ├── _base.scss
│   └── _layout.scss
├── _site
├── .jekyll-cache
│   └── Jekyll
│       └── Cache
│           └── [...]
├── .jekyll-metadata
└── index.html # can also be an 'index.md' with valid front matter
```

### References

- _Directory Structure_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/structure/>

## Liquid

- [Liquid](https://shopify.github.io/liquid/) is an open-source template language created by Shopify and written in Ruby.
- It has 3 main components: objects, tags, and filters.

### Objects

- Objects tell Liquid to output predefined [variables](https://jekyllrb.com/docs/variables/) as content on a page.
- Use double curly braces for objects: `{{` and `}}`.
- For example, `{{ page.title }}` displays the `page.title` variable.

### Tags

- [Tags](https://jekyllrb.com/docs/liquid/tags/) define the logic and control flow for templates.
- Use curly braces and percent signs for tags: `{%` and `%}`.
- For example:

```liquid
{% if page.show_sidebar %}
  <div class="sidebar">
    sidebar content
  </div>
{% endif %}
```

- This displays the sidebar if the value of the `show_sidebar` page variable is true.

### Filters

- [Filters](https://jekyllrb.com/docs/liquid/filters/) change the output of a Liquid object.
- They are used within an output and are separated by a `|`.
- For example:

```liquid
{{ "hi" | capitalize }}
```

- This displays `Hi` instead of `hi`.

### Use Liquid

- Use Liquid to make your `Hello World!` text lowercase:

```liquid
...
<h1>{{ "Hello World!" | downcase }}</h1>
...
```

- To make Jekyll process your changes, add "front matter" to the top of the page:

```text
---
# front matter tells Jekyll to process Liquid
---
```

- Example: [`tutorial_site/liquid/index.html`](tutorial_site/liquid/index.html) (<http://localhost:4000/liquid/>)

### References

- _Liquid_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/02-liquid/>

## Front Matter

- Front matter is a snippet of YAML placed between two triple-dashed lines at the start of a file.
- You can use front matter to set variables for the page:

```text
---
my_number: 5
---
```

- You can call front matter variables in Liquid using the `page` variable.
- For example, `{{ page.my_number }}`.

### Use front matter

- Change the `<title>` on your site to use front matter: [`tutorial_site/front_matter/index.html`](tutorial_site/front_matter/index.html) (<http://localhost:4000/front_matter/>)

### References

- _Front Matter_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/03-front-matter/>

## Layouts

### Creating a layout

- Layouts are templates that can be used by any page in your site and wrap around page content.
- They are stored in a directory called `_layouts`.
- Create the `_layouts` directory in your site's root folder and create a new [`default.html`](tutorial_site/_layouts/default.html) file.
- This HTML is almost identical to `index.html` except:
  - there's no front matter, and
  - the content of the page is replaced by a `content` variable.
- `content` is a special variable that returns the rendered content of the page on which it's called.

### Variables

- You can set front matter in layouts, the only difference is when you're using in Liquid, you need to use the `layout` variable instead of `page`.

```text
---
city: San Francisco
---
<p>{{ layout.city }}</p>

{{ content }}
```

### Use layouts

- To make [`index.html`](tutorial_site/layouts/index.html) use your new layout, set the `layout` variable in the front matter.
- Since the layout wraps around the content on the page, you can call front matter like `page` in the layout file.
  - When you apply the layout to a page, it uses the front matter on that page.

### Build the About page

- Jekyll supports Markdown in addition to HTML when building pages.
- Add [`about.md`](tutorial_site/layouts/about.md) (<http://localhost:4000/layouts/about.html>) to use your new layout in the About page.

### References

- _Layouts_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/04-layouts/>
- _Layouts_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/layouts/>

‌

## ‌Includes

- Navigation should be on every page so adding it to your layout is the correct place to do this.

### Include tag

- The `include` tag allows you to include content from another file stored in an `_includes` folder.
- Includes are useful for having a single source for source code that repeats around the site or for improving the readability.

### Include usage

- Create a file for the navigation at [`_includes/navigation.html`](tutorial_site/_includes/navigation.html).
- Use the `include` tag to add the navigation to [`_layouts/default_include.html`](tutorial_site/_layouts/default_include.html).
- Reference the modified layout in [`tutorial_site/includes/index.html`](tutorial_site/includes/index.html) and [`tutorial_site/includes/about.md`](tutorial_site/includes/about.md). (<http://localhost:4000/includes/>)

### Current page highlighting

- [`_includes/navigation.html`](tutorial_site/_includes/navigation.html) needs to know the URL of the page it's inserted into so it can add styling.
- Using the `page.url` variable, you can check if each link is the current page and colour it red if true:
  - `<a href="/includes/" {% if page.url == "/includes/" %}style="color: red;"{% endif %}>`
  - `<a href="/includes/about.html" {% if page.url == "/includes/about.html" %}style="color: red;"{% endif %}>`

### References

- _Includes_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/05-includes/>

## ‌Data Files

- Jekyll supports loading data from YAML, JSON, and CSV files located in a `_data` directory.
- Data files are a great way to separate content from source code to make the site easier to maintain.

### Data file usage

- Create a data file for the navigation at [`_data/navigation.yml`](tutorial_site/_data/navigation.yml).
- Jekyll makes this data file available to you at `site.data.navigation`.
- Instead of outputting each link in [`_includes/navigation_data.html`](tutorial_site/_includes/navigation_data.html), now you can iterate over the data file instead. (<http://localhost:4000/data/>)

### References

- _Data Files_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. https://jekyllrb.com/docs/step-by-step/06-data-files/

## Assets

- Place CSS, JS, images, and other assets in your site folder and they'll copy across to the built site.
- Jekyll sites often use this structure to keep assets organized:

```text
.
├── assets
│   ├── css
│   ├── images
│   └── js
...
```

### Sass

- Inlining the styles used in [`_includes/navigation_data.html`](tutorial_site/_includes/navigation_data.html) is not a best practice.
- Instead, let's style the current page by defining a class in a new css file: [`_includes/navigation_assets.html`](tutorial_site/_includes/navigation_assets.html).
- You could use a standard CSS file for styling, but we're going to take it a step further by using [Sass](https://sass-lang.com/).
  - Sass is baked right into Jekyll.
- Create a Sass file at [`assets/css/styles.scss`](tutorial_site/assets/css/styles.scss).
  - `@import "main"` tells Sass to look for a file called `main.scss` in the `_sass` directory by default.
- Create the `current` class in order to colour the current link green at [`_sass/main.scss`](tutorial_site/_sass/main.scss).
- Reference the stylesheet in your layout ([`_layouts/default_assets.html`](tutorial_site/_layouts/default_assets.html)) and add the stylesheet to the `<head>`.
  - `styles.css` referenced here is generated by Jekyll from `styles.scss` in `assets/css/`.
- Load up <http://localhost:4000/sass/> and check that the active link in the navigation is green.

### References

- _Assets_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/07-assets/>

## Blogging

### Posts

- Blog posts live in a folder called `_posts`.
- The filename for posts have a special format: the publish date, then a title, followed by an extension.
- Create your first post at [`_posts/2018-08-20-bananas.md`](tutorial_site/_posts/2018-08-20-bananas.md).
  - `author` is a custom variable; it's not required and could have been named something like `creator`.
- Create more posts:
  - [`_posts/2018-08-21-apples.md`](tutorial_site/_posts/2018-08-21-apples.md)
  - [`_posts/2018-08-22-kiwifruit.md`](tutorial_site/_posts/2018-08-22-kiwifruit.md)

### Layout

- Create the layout for the blog post at [`_layouts/post.html`](tutorial_site/_layouts/post.html).
  - This is an example of layout inheritance.
  - The post layout outputs the title, date, author and content body, which is wrapped by the default layout.
  - Also note the `date_to_string` filter, which formats a date into a nicer format.

### List posts

- Jekyll makes posts available with `site.posts`.
- Create [`blog.html`](tutorial_site/blogging/blog.html) in your site root directory.
  - `post.url` is automatically set by Jekyll to the output path of the post.
  - `post.title` is pulled from the post filename and can be overridden by setting `title` in front matter.
  - `post.excerpt` is the first paragraph of content by default.
- Add [`_data/navigation_blogging.yml`](tutorial_site/_data/navigation_blogging.yml) for navigation.
- Open <http://localhost:4000/blogging/> and have a look through your blog posts.

### References

- _Blogging_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/step-by-step/08-blogging/>

## Collections

- Collections are similar to posts except the content doesn't have to be grouped by date.

### Configuration

- Jekyll configuration to set up a collection happens in a file called `_config.yml` (by default).
- Create [`_config.yml`](tutorial_site/_config.yml) in the site root directory.
- Restart the jekyll server (`bundle exec jekyll serve`) to (re)load the configuration.

### Add authors

- Documents (the items in a collection) live in a folder in the root of the site named `_*collection_name*`, in this case, `_authors`.
- Create a document for each author:
  - [`_authors/jill.md`](tutorial_site/_authors/jill.md)
  - [`_authors/ted.md`](tutorial_site/_authors/ted.md)

### Staff page

- Let's add a page which lists all the authors on the site.
- Jekyll makes the collection available with `site.authors`.
- Create [`staff.html`](tutorial_site/collections/staff.html) and iterate over `site.authors` to output all the staff.
  - Since the content is markdown, you need to run it through the `markdownify` filter.
  - This happens automatically when outputting using `{{ content }}` in a layout.
- You also need a way to navigate to this page through the main navigation: [`_data/navigation_collections.yml`](tutorial_site/_data/navigation_collections.yml).

### Output a page

- By default, collections do not output a page for documents.
- In this case we want each author to have their own page.
- Add `output: true` to the author collection configuration: [`_config.yml`](tutorial_site/_config.yml).
  - Restart the jekyll server for the configuration changes to take effect.
- You can link to the output page using `author.url` in [`staff.html`](tutorial_site/collections/staff.html).
- Just like posts, create a layout for authors: [`_layouts/author.html`](tutorial_site/_layouts/author.html).

### Front matter defaults

- What you want is all posts to automatically have the post layout, authors to have author layout, and everything else to use the default layout.
- You can achieve this by using [front matter defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/) in [`_config.yml`](tutorial_site/_config.yml).
- You set a scope of what the default applies to, then the default front matter you'd like.
- Now you can omit layout from the front matter of all pages and posts.

### Link to authors page

- The posts have a reference to the author so let's link it to the author's page in in [`_layouts/post.html`](tutorial_site/_layouts/post.html).
  - `{% assign author = site.authors | where: 'short_name', page.author | first %}`:
    - assign the `site.authors` collection to the `author` variable;
    - apply the `where` filter to create an array to include only objects where the `short_name` property is the post page's `page.author` custom variable value;
    - apply the `first` filter to return the first item in the array.
- Open up <http://localhost:4000/collections> and have a look at the staff page and the author links on posts.

### References

- _Collections_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. https://jekyllrb.com/docs/step-by-step/09-collections/
- Shopify. (2024). _where_. Liquid Template Language. <https://shopify.github.io/liquid/filters/where/>
- Shopify. (2024). _first_. Liquid Template Language. <https://shopify.github.io/liquid/filters/first/>

## Deployment

- See <https://jekyllrb.com/docs/step-by-step/10-deployment/>.

## Themes

- Jekyll themes specify plugins and package up assets, layouts, includes, and stylesheets in a way that can be overridden by your site's content.
- You can find and preview themes on different galleries: <https://jekyllrb.com/docs/themes/#pick-up-a-theme>.

### Understanding gem-based themes

- When you create a new Jekyll site (by running the `jekyll new <PATH>` command), Jekyll installs a site that uses a gem-based theme called [Minima](https://github.com/jekyll/minima).
- With gem-based themes, some of the site's directories (such as the `assets`, `_data`, `_layouts`, `_includes`, and `_sass` directories) are stored in the theme's gem.
- In the case of Minima, you see only the following files in your Jekyll site directory:

```text
.
├── 404.html
├── about.markdown
├── _config.yml
├── Gemfile
├── index.markdown
└── _posts
    └── 2024-05-07-welcome-to-jekyll.markdown
```

- If you have the theme gem, you can run `bundle update` to update all gems in your project.
- You can run bundle `update <THEME>`, replacing `<THEME>` with the theme name, such as `minima`, to just update the theme gem.

### Overriding theme defaults

- You can override any of the theme default data, layouts, includes, and stylesheets with your own site content.
- To replace layouts or includes in your theme, create a file with the same name as the file you wish to override in your site's `_layouts` or `_includes` directory.
  - For example, if your selected theme has a `page` layout, you can override it by creating your own `_layouts/page.html`.
- To modify any stylesheet you must take the extra step of also copying the main sass file (`_sass/minima.scss` in the Minima theme) into the `_sass` directory in your site's source.
- To locate a theme's files on your computer, run `bundle info --path <THEME>`, e.g., `bundle info --path minima`.

```bash
$ bundle info --path minima
/path/to/gems/minima-2.5.1

$ tree $(bundle info --path minima)
/path/to/gems/minima-2.5.1
├── assets
│   ├── main.scss
│   └── minima-social-icons.svg
├── _includes
│   ├── disqus_comments.html
│   ├── footer.html
│   ├── google-analytics.html
│   ├── header.html
│   ├── head.html
│   ├── icon-github.html
│   ├── icon-github.svg
│   ├── icon-twitter.html
│   ├── icon-twitter.svg
│   └── social.html
├── _layouts
│   ├── default.html
│   ├── home.html
│   ├── page.html
│   └── post.html
├── LICENSE.txt
├── README.md
└── _sass
    ├── minima
    │   ├── _base.scss
    │   ├── _layout.scss
    │   └── _syntax-highlighting.scss
    └── minima.scss

5 directories, 22 files
```

### Installing a gem-based theme

- The `jekyll new <PATH>` command isn't the only way to create a new Jekyll site with a gem-based theme.
- To install a gem-based theme:
  - Add the theme gem to your site's `Gemfile`, or if you've started with the `jekyll new` command, replace `gem "minima", "~> 2.0"` with the gem you want:
    - `gem "jekyll-theme-minimal"`
  - Install the theme:
    - `bundle install`
  - Add the following to your site's `_config.yml` to activate the theme:
    - `theme: jekyll-theme-minimal`
  - Build your site:
    - `bundle exec jekyll serve`

### References

- _Themes_. (2024, April 23). Jekyll • Simple, Blog-Aware, Static Sites. <https://jekyllrb.com/docs/themes/#pick-up-a-theme>
