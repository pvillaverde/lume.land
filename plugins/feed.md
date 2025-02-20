---
title: Feed
description: Generate a RSS or JSON Feed automatically for your site
docs: plugins/sitemap.ts/~/Options
tags:
  - utils
---

## Description

This plugin generates RSS or JSON feeds automatically with any page collection
(like posts, articles, events, etc).

## Installation

Import this plugin in your `_config.ts` file to use it:

```js
import lume from "lume/mod.ts";
import feed from "lume/plugins/feed.ts";

const site = lume();

site.use(feed({
  output: ["/posts.rss", "/posts.json"],
  query: "type=post",
  info: {
    title: "=site.title",
    description: "=site.description",
  },
  items: {
    title: "=title",
    description: "=excerpt",
  },
}));

export default site;
```

See
[all available options in Deno Doc](https://doc.deno.land/https/deno.land/x/lume/plugins/feed.ts/~/Options).

## Configuration

Internally, this plugin uses [Search](./search.md) to search and return the
pages, so you have to provide a query to search the pages and optionally the
sort and limit parameters.

You need to configure also the generic data of the feed (in the `info` key) and
the info of the items (in the `items` key). This is an example with all
available options:

```js
site.use(sitemap({
  output: ["/posts.rss", "/posts.json"], // The file or files that must be generated
  query: "type=post", // Select only pages of type=post
  sort: "date=desc", // To sort by data in ascendent order
  limit: 10, // To show only the 10 first results
  info: {
    title: "My blog", // The feed title
    description: "Where I put my thoughts", // The feed subtitle
    date: new Date(), // The publish date
    lang: "en", // The language of the feed
    generator: true, // Set `true` to automatically generate the "Lume {version}"
  },
  items: {
    title: "=title", // The title of every item
    description: "=excerpt", // The description of every item
    date: "=date", // The published date of every item
    content: "=children", // The content of every item
    lang: "=lang", // The language of every item
  },
}));
```

The options `info` and `items` use the
[same aliases as `metas` plugin](./metas.md): any value starting with `=`
represents a variable name that will be used to extract this info. For example,
the description of the items has the value `=excerpt`, which means every item
will use the value of the variable `excerpt` for the description.

It's also possible to extract the info using CSS selectors. For example, let's
say we want to generate a RSS with the same content as the div `.post-content`.
We just have to start the value of the code with `$`:

```ts
site.use(sitemap({
  // general config
  info: {
    // info config
  },
  items: {
    title: "=title",
    description: "=excerpt",
    date: "=date",
    content: "$.post-content", // Use the content of .post-content element
    lang: "=lang",
  },
}));
```

If you want to create more than one feed, just use the plugin once per feed:

```ts
site.use(feed({
  output: "/posts.rss",
  // Posts feed configuration
}));

site.use(feed({
  output: "/articles.rss",
  // Articles feed configuration
}));
```
