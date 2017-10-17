---
title: Grouping posts by tag
description: Instructions on how to group Jekyll posts by tag
tags: jekyll
---

For my site I wanted to be able to show individual lists of posts broken down by tag. This works as follows.

In the front matter for each post I specify which tags I want to link it to.

```yaml
# some-post.md
---
title: ...
tags: blog css
---
```

Then, for each tag that exists I created a page to display the posts that match. I grouped these pages into a folder
called tag. Each page has only two lines of front matter. For example the one for blog posts looks liks this:

```yaml
# tag/blog.html
---
title: Blog posts
tag: blog
---
```

The layout for these pages is specified in the _config.yml to reduce repetition.

```yaml
defaults:
  - scope:
      path: "tag"
      type: "pages"
    values:
      layout: "tag"
```

This says that for any pages inside the tag folder (path), assign the tag layout. All that remains is to create the
tag layout.

In its most basic form the tag layout looks like this:

{% raw %}
```html
---
layout: default
---

<h1>{{ page.title }}</h1>

{% for post in site.tags[page.tag] %}
<article>
  <a href="{{ post.url }}">
    <h2>{{ post.title }}</h2>
    <p>{{ post.excerpt | strip_html }}</p>
  </a>
</article>
{% endfor %}
```
{% endraw %}

The important part here is `site.tags` which contains a list of all posts grouped by tag. By passing in the
variable we set on the tag page we can retrieve all the posts for that tag. For example `site.tags['blog']` gives us all
blog posts.

Just make sure there is a page for each tag type and that's all there is to it.

