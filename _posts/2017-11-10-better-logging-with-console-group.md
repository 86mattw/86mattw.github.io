---
title: Better logging with console.group
description: Improve console logging with the console.group command.
tags: javascript
---

If you're logging a lot of information to the console for debugging it can get a little tricky to search through the
output.

The `console.group` method allows you to indent and collapse related logs. A group can be created as follows:

```js
console.group('first group');
console.log('some message');
console.groupEnd('first group');
```

In recent versions of Chrome and Firefox we also have the ability to add CSS styling to our console logs to further
improve readability and make key values stand out:

```js
console.group('second group');
console.log('%c this message has styling', 'color: green;');
console.groupEnd('second group');
```

Open dev tools and check out the console to see these examples in action.

{% raw %}
<script>
  console.group('first group');
  console.log('some message');
  console.groupEnd('first group');

  console.group('second group');
  console.log('%c this message has styling', 'color: green;');
  console.groupEnd('second group');
</script>
{% endraw %}
