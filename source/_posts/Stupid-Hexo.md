---
title: Stupid Hexo
categories:
    -   Others
---

## How to add Category and Tag page
If you want one page list all categories, just create `source/categories/index.md`, and then write `type: categories` in it, which tells Hexo to create `public/categories/index.html`. "Tags", "About" are the same.

```shell
cd source
mkdir categories
cd categories
vi index.md
```

```md
<!-- index.md -->
---
title: categories
type: "categories"
---

```

