# Hugo 😺
I like [Hugo](https://gohugo.io). All the websites I build use Hugo. It's only fair to learn to use it correctly (or just enough to be productive) and know what to do when things don't work.

## References
* [The OG Hugo docs](https://gohugo.io/documentation/)
* [Compact guide by Strapi](https://strapi.io/blog/guide-to-using-hugo-site-generator)
* [Some sections from this Hugo mini course](https://hugo-mini-course.netlify.app)
* [If you like videos, there's a nice YouTube playlist](https://www.youtube.com/watch?v=qtIqKaDlqXo&list=PLLAZ4kZ9dFpOnyRlyS-liKL5ReHDcj4G3)

## Setup
* The easiest way to install is to pick the "extended" binary from [the GitHub page](https://github.com/gohugoio/hugo/releases/latest) and place it in your `$PATH`.
* Create a new site with `hugo new site [site name]`, place your preferred theme under `themes/` and edit your `config.toml` as required. Build it locally with `hugo server` and get the static files with just `hugo` (you can use CI/CD to automate this).
* The default directories created are [archetypes which lets you define templates for new content](#archetypes), content which well ..., [data (which is empty)](#data), layouts which stores the template files in `.html` format, and static which holds static content like images, JS, CSS, etc.

## Configuration
* By default, this is done with the `config.toml` file. You can change this and even have multiple config files.
* There are plenty of options to change. You can enable emojis, set the content & data dirs, set minify params, pagination, etc. [They are available on the Hugo site](https://gohugo.io/getting-started/configuration/).
* If you want to check out how to configure something **an easy way** is to grep for it, like so `hugo config | grep emoji`.
* The values under `params` in the config file will be available under `.Site.Params` for the templates.

## Hugo Modules
I have no clue what is going on and I hope I don't have to use it.

## Site Structure
### Archetypes
When you do `hugo new [dirname]/[filename].md` it searches for these files: `archetypes/[dirname].md`, `archetypes/default.md` and then the archetypes directory in the theme being used. You can also specify one particular template to be used e.g. `hugo new --kind [template-file/directory] [new file path]`.
### Assets
These files require processing before usage. They can be placed in resource objects and processed, like so: `resource.get "sass/form.scss" | resource.ToCSS` where the file is in `assets/sass`.
### Data
This folder can hold data that will be used in your content. Popular formats used are `JSON`, `TOML`, `CSV`, etc. They will be available under `.Site.Data`.

## Content Management
### Organization
You should organize your content inside `content/` the way you want it to be in the output site. `_index.md` lets you add content and front matter to your homepage and section pages.
### Front Matter Variables
There are many default page variables available by default, which can be changed. They are under `.Page`. You can also define your own variables which will be available under `.Params` (these vars have to be in lowercase). You can specify a `weight` variable which can be used to order content within a list.
### Static
Static files are served without modification. The files are served from the site root, so `<SITE PROJECT>/static/me.png` will be available here: `<MY_BASEURL>/me.png`.
### Shortcodes
They are small snippets you can use in content files (i.e. Markdown) that render with a predefined template. They are of the format: `{{% shortcodename params %}}` where multiple parameters are space separated. You can also use named parameter instead of positional parameters (they are of the format `name="value"`). **Some shortcodes require closing tags** (`{{% /shortcodename %}}`). `< ... >` can also be used instead of `% ... %` when the inner content does not need markdown rendering. Examples:
* `{{< figure src="/media/spf13.jpg" title="Steve Francia" >}}`
* `{{< instagram BWNjjyYFxVx hidecaption >}}`
### Page Bundles
A leaf bundle is a **regular** page while a branch bundle is a **list** page. A leaf bundle is a directory that has an `index.md` while a branch bundle has an `_index.md`. There can be other bundles nested under a branch bundle. **A page bundle is a directory that has `_index.md` or `index.md` at its root**. 
### Page resources
Images, other pages, and data files are available to the page they are bundled with. They can be accessed via `Resources.ByType` or `Resources.Match`. Their content and meta-data can be accessed.

## Pipes
I don't think I'll be using this often but I found this interesting thing here…
### Error Handling!
```
{{ with resources.GetRemote "https://gohugo.io/images/gohugoio-card-1.png" }}
  {{ with .Err }}
    {{ warnf "%s" . }}
  {{ else }}
    <img src="{{ .RelPermalink }}" width="{{ .Width }}" height="{{ .Height }}" alt="">
  {{ end }}
{{ end }}
```  
I suppose that kind of error handling works in other places too.

## Functions
You use functions like this `{{ funcName param1 param2 }}`. There are many inbuilt ones in Hugo that you can find on [the website](https://gohugo.io/functions) including string processing and even `emojify`!  
These are frequently used with conditional statements since `if` can only check one value for "true" or "false" (like C). You can, therefore, have powerful stuff like `if (lt $var1 .Var2)` which makes use of the `lt` function.

## Variables
Hugo's templates are context aware and can access many values. Here are a few of the important ones
### `.Site`
These are site level vars ("global vars"). Some of them are defined in the config file but Hugo has inbuilt vars too. `.Site.Params` has the values from the `params` section from the config.
### `.Site.Menus`
Holds named array of menu entries. E.g. an array `main` in the config file can be accessed as `.Site.Menus.main`.
### `.Title`
Can be set in the configuration file. It's meant to be the title used in HTML.
### Custom
You can define custom variables within templates using `{{ $varName := varValue }}` syntax. You can access the value of the variable in other places within the template by using `{{ $varName }}`.

## Templates
### The `baseof` page
You can define it under `layouts/_default/` in your project root. It's the template that all other templates inherit from. You can use **blocks** in your template files. These blocks are replaced with content while rendering the HTML files. The format is: `{{ block "blk_name" .}} [default content if the block is not defined elsewhere] {{ end }}`. You can **define blocks** in other files and the content within them will be put where the blocks are used. The format is `{{ define "blk_name" }} [content to replace the default block content] {{ end }}`.
### The homepage
Create a file `index.html` under `layouts/`. You can call other templates within blocks too by using `{{ .Render "tmpl_name" }}`. The file `layouts/tmpl_name.html` should exist for this to work.
### Single pages
For any page, if nothing else matches, the template used is `layouts/single.html`. Some variables that are available are: `.Title`, `.Content`, `.Date`, etc.
### List pages
`layouts/list.html` is the template for directories (list pages). They work regardless of whether an `_index.md` file is present in the root of that directory. The `.Pages` variable gives all the pages in that directory.
### Partials
These are small templates that are like "components" and can be used to build larger templates. E.g. You might have a "navbar component" that you want to use in different places in your site. It can be placed under `layouts/partials/` as `navbar.html`. You can use the content in it by using `{{ partial "navbar.html" }}`. You can also pass a variable to your partial, like so: `{{ partial "navbar.html" .}}`.

## Note
* With both blocks and partials, you can pass in a single variable to block/partial definition. It's the last parameter and usually `.` but you can pass other variables too. You can even create a custom variable using `(dict key1 value1 key2 value2 ...)` and the definition can access the values through the keys like this: `.key1`.