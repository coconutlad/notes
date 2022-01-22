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
// TODO

## Variables
Hugo's templates are context aware and can access many values. Here are a few of the important ones
### `.Site`
These are site level vars ("global vars"). Some of them are defined in the config file but Hugo has inbuilt vars too. `.Site.Params` has the values from the `params` section from the config.
### `.Site.Menus`
Holds named array of menu entries. E.g. an array `main` in the config file can be accessed as `.Site.Menus.main`.