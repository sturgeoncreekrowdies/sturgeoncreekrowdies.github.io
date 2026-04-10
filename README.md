# Sturgeon Creek Rowdies Rugby Club Website  

A fork of Chirpy Starter.

README

## Quick Start Overview

| What you want to do | Where to put files | Special naming |
|-------------------|-------------------|----------------|
| Add photos to gallery | `assets/img/photos/` | `YYYY-MM-DD-description.jpg` |
| Write a blog post | `_posts/` | `YYYY-MM-DD-title.md` |
| Add to Hall of Fame | `_posts/` | `YYYY-MM-DD-hof-title.md` |

---

## 1. Adding Photos to the Gallery

1. Name your photo: `YYYY-MM-DD-description.jpg`
   - Example: `2024-04-10-team-photo.jpg`
2. Upload your photo to: `assets/img/photos/`

---

## 2. Adding a Blog Post

1. Open any text editor (Notepad etc.) and copy this template in to your file:
```
---
title: "My Title"
date: YYYY-MM-DD
categories: [Category] This whole line is optional, delete if not using.
tags: [practice, social] This whole line is optional, delete if not using.
---

![Photo description]({{ '/assets/img/photos/YOUR-FILENAME.jpg' | relative_url }}) If you want to embed a photo

TEXT GOES HERE
```

2. Save the file and name it exactly like this: `YYYY-MM-DD-your-title-here.md`
   - Example: `2024-04-10-our-amazing-trip.md`
3. Upload your post to _posts


## 3. Adding a Hall of Fame post
1. Name your photo: `YYYY-MM-DD-hof-description.jpg`
   - Example: `2024-04-10-hof-team-photo.jpg`
2. Upload your photo to: `assets/img/photos/`
3. 1. Open any text editor (Notepad etc.) and copy this template in to your file:
```
---
title: "My Title"
date: YYYY-MM-DD
categories: [hall-of-fame] 
tags: [practice, social] This whole line is optional, delete if not using.
external_link: "https://yourlink"
---

![Photo description]({{ '/assets/img/photos/YOUR-FILENAME.jpg' | relative_url }}) If you want to embed a photo

TEXT GOES HERE

[Read more →]({{ page.external_link }})
```

## License

This work is published under [MIT][mit] License.

[![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy)][gem]&nbsp;
[![GitHub license](https://img.shields.io/github/license/cotes2020/chirpy-starter.svg?color=blue)][mit]
[gem]: https://rubygems.org/gems/jekyll-theme-chirpy
[chirpy]: https://github.com/cotes2020/jekyll-theme-chirpy/
[CD]: https://en.wikipedia.org/wiki/Continuous_deployment
[mit]: https://github.com/cotes2020/chirpy-starter/blob/master/LICENSE
