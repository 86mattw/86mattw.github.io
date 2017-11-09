---
title: CSS triangles
description: A CSS only approach to creating triangles
tags: css sass
---

This is a technique which I've been using for a long time but decided to document it in a quick post.

Designs often include triangles in one form or another. Rather than using an image these can usually be produced with 
CSS.

The following would produce a triangle pointing upwards.

```css
.arrow-up {
  width: 0;
  height: 0;
  border-left: 30px solid transparent;
  border-right: 30px solid transparent;
  border-bottom: 30px solid #000000;
  font-size: 0;
  line-height: 0;
}
```

{% raw %}
<span style="
    border-left: 30px solid transparent;
    border-right: 30px solid transparent;
    border-bottom: 30px solid #000000;
    height: 0;
    width: 0;
    font-size: 0;
    line-height: 0;
    display: block;
    margin-bottom: 20px;
"></span>
{% endraw %}

We can also utilise SASS to turn this into a re-usable mixin. Below are the mixins that I use.

```sass
@mixin triangle-down($size1, $size2, $colour) {
  display: inline-block;
  border-left: $size1 solid transparent;
  border-right: $size1 solid transparent;
  border-top: $size2 solid $colour;
}

@mixin triangle-up($size1, $size2, $colour) {
  display: inline-block;
  border-left: $size1 solid transparent;
  border-right: $size1 solid transparent;
  border-bottom: $size2 solid $colour;
}

@mixin triangle-left($size1, $size2, $colour) {
  display: inline-block;
  border-top: $size1 solid transparent;
  border-bottom: $size1 solid transparent;
  border-right: $size2 solid $colour;
}

@mixin triangle-right($size1, $size2, $colour) {
  display: inline-block;
  border-top: $size1 solid transparent;
  border-bottom: $size1 solid transparent;
  border-left: $size2 solid $colour;
}
```

There are versions of this approach that combine the above mixins into one which does remove some of the repetition.
However I prefer this approach as it's more explicit and keeps the mixins as simple as possible, inline with the KISS
principle.

The mixins can be used in the following ways:

```sass
.tooltip__arrow {
  @include triangle-down(10px, 15px, #000);
  ...
}
```