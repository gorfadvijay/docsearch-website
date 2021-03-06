---
title: Required configuration
---

This section, [empowered by the details about how we build a DocSearch
index][1], gives you the best practices to optimize our crawl. Adopting this
following specification is required to let our crawler build the best experience
from your website. You will need to update your website and follow these rules.

Note: If your website is generated thanks to one of our supported tool, you do
not need to change your website as it is already compliant with our
requirements.

## The generic configuration example

```json
{
  "index_name": "example",
  "start_urls": ["https://www.example.com/doc/"],
  "sitemap_urls": ["https://www.example.com/sitemap.xml"],
  "stop_urls": [],
  "selectors": {
    "lvl0": {
      "selector": ".DocSearch-lvl0",
      "global": true,
      "default_value": "Documentation"
    },
    "lvl1": {
      "selector": ".DocSearch-lvl1",
      "global": true,
      "default_value": "Chapter"
    },
    "lvl2": ".DocSearch-content .DocSearch-lvl2",
    "lvl3": ".DocSearch-content .DocSearch-lvl3",
    "lvl4": ".DocSearch-content .DocSearch-lvl4",
    "lvl5": ".DocSearch-content .DocSearch-lvl5",
    "lvl6": ".DocSearch-content .DocSearch-lvl6",
    "text": ".DocSearch-content p, .DocSearch-content li"
  },
  "custom_settings": {
    "attributesForFaceting": ["language", "version"]
  },
  "nb_hits": "OUTPUT OF THE CRAWL"
}
```

### Overview of a clear layout

A website implementing these good practises will look simple and crystal clear.
It can have this following aspect:

<img src="https://docsearch.algolia.com/img/assets/recommended-layout.png" alt="Recommended layout for your page"/>

The main blue element will be your `.DocSearch-content` container. More details
in the following guidelines.

### Use the right classes as [selectors][2]

You can add some specific static classes to help us find your content role.
These classes can not involve any style changes. These dedicated classes will
help us to create a great learn-as-you-type experience from your documentation.

- Add a static class `DocSearch-content` to the main container of your textual
  content. Most of the time, this tag is a `<main>` or an `<article>` HTML
  element.

- Every searchable `lvl` elements outside this main documentation container (for
  instance in a sidebar) must be `global` selectors. They will be globally
  picked up and injected to every record built from your page. Be careful, the
  level value matters and every matching element must have an increasing level
  along the HTML flow. A level `X` (for `lvlX`) should appear after a level `Y`
  while `X > Y`.

- `lvlX` selectors should use the standard title tags like `h1`, `h2`, `h3`,
  etc. You can also use static classes. Set a unique `id` or `name` attribute to
  these elements as detailed below.

- Every DOM elements matching the `lvlX` selectors must have a unique `id` or
  `name` attribute. This will help the redirection to directly scroll down to
  the exact place of the matching elements. These attributes define the right
  anchor to use.

- Every textual element (selector `text`) must be wrapped in a `<p>` or `<li>`
  tag. This content must be atomic and splitted into small entities. Be careful
  to never nest one matching element into another one as it will create
  duplicates.

- Stay consistent and do not forget that we need to have some consistency along
  the HTML flow [as presented here][1].

## Introduce global information as meta tags

Our crawler automatically extracts information from our DocSearch specific meta
tags:

```html
<meta name="docsearch:language" content="en" />
<meta name="docsearch:version" content="1.0.0" />
```

The `content` value of the `meta` tag will be added to every record extracted
from the page. Given that the name is `docsearch:$NAME`, `$NAME` will be set as
an attribute in every records. Its value will be its related `content` value.
You can then transform these attributes as [`facetFilters`][3] to filter over
them from the UI. We will need to set `attributesForFaceting` of your Algolia
index exposed via the DocSearch `custom_settings` parameter.

```json
"custom_settings": {
  "attributesForFaceting": ["language", "version"]
}
```

## Nice to have

- Your website should have [an updated sitemap][4]. This is key to let our
  crawler know what should be updated. Do not worry, we will still crawl your
  website and discover embedded hyperlinks to find your great content.

- Every page needs to have their full context available. Using global elements
  might help (see above).

- Make sure your documentation content is also available without JavaScript
  rendering on the client-side. If you absolutely need JavaScript turned on, you
  need to [set `js_render: true` in your configuration][5].

Any questions? [Send us an email][6].

[1]: how-do-we-build-an-index.md
[2]: config-file.md
[3]: https://www.algolia.com/doc/guides/searching/filtering/#facet-filters
[4]: https://www.sitemaps.org/
[5]: config-file.md
[6]: mailto:DocSearch@algolia.com
