---
title: Using SVGs in Jekyll
description: How I ended up displaying SVGs on my Jekyll site
tags: jekyll svg
---

For a typical project where icons are required I prefer using the 
[SVG symbol method](https://css-tricks.com/svg-symbol-good-choice-icons/).

This approach involves wrapping all of your SVG icons with the `<symbol>` element and then combining them into a
single SVG which is injected into the top of the document. Each individual icon can then be added to the page where
needed using the `<use>` element. There are build tools for both Grunt and Gulp which can automate this for you.

I like this approach as it gives you lots of flexibility, saves on HTTP requests and can help with accessibility.

This approach wasn't going to work with Jekyll. Instead I took advantage of the built-in include system...

### SVGs as include files

All of my SVGs live inside an icons folder inside my _includes directory.

```yaml
_includes/
  icons/
    aws.svg
    babel.svg
    html5.svg
    ...
```

Adding an icon to a page was then a simple case of using:

{% raw %}
```html
{% include icons/html5.svg %}
```
{% endraw %}

This embeds the SVG into the page, saving an HTTP request.

I wanted to retain the styling flexibility that the symbol approach gives you. To do this you can again leverage 
Jekyll's include system, which allows for variables to be passed to the included file. The following code allows me to
add class names and inline styling to my SVGs:

{% raw %}
```html
<!-- _includes/icons/html5.svg -->
<svg {% if include.classes %}class="{{ include.classes }}"{% endif %} {% if include.styles %}style="{{ include.styles }}"{% endif %}>
  ...
</svg>
```
{% endraw %}

{% raw %}
```html
{% include icons/html5.svg classes="icon icon--html5" styles="margin: 10px;" %}
```
{% endraw %}

In order to control the colour with CSS make sure that any inline `fill` attributes have been removed from your 
SVGs.

The above method allows me to re-use the same SVG file and alter the styling however I need. The SVGs can be included
in both pages and posts written in markdown.

{% include icons/react.svg styles="width: 40%; fill: peachpuff; margin-bottom: 20px;" %}
{% include icons/react.svg styles="width: 30%; fill: khaki; margin-bottom: 20px;" %}
{% include icons/react.svg styles="width: 20%; fill: lightblue; margin-bottom: 20px;" %}

### Displaying icons programmatically

If you need to output a series of icons, such as I have on my About page, you can do the following:

Create a data file for the icons you want to output. Add this to your _data directory. It can be either yaml or json.
Inside create a list/array of each icon you want to output. Give each at least a path and class.

```yaml
# _data/my_icons.yml
- path: aws.svg
  class: icon--aws
- path: babel.svg
  class: icon--babel
- path: html5.svg
  class: icon--html5
```

The following code can then be used to iterate over the list and output the icons:

{% raw %}
```html
{% for icon in site.data.my_icons %}
  {% include {{ icon.path | prepend: 'icons/' }} classes="{{ icon.class }}" %}
{% endfor %}
```
{% endraw %}

I'm using a filter here to add the folder name to each path value. Not essential but saves some typing in the data file.