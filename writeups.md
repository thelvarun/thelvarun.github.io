---
layout: page
title: writeups
categories:
  writeups: "writeups"
  blogs: "blogs"
subcategories:
  writeups:
    algorithms: "algorithms"
    chess: "chess"
---
{% assign writeups_category = page.categories.writeups %}

This is a collection of writeups for various different technical fields I find
interesting. This ranges from algorithms to physics problems to chess annotations
and maybe even media analysis. Idk!

## Algorithms

{% assign writeups_category = page.categories.writeups %}
{% assign algorithms_subcategory = page.subcategories.writeups.algorithms %}
{% include post_list.html category=writeups_category subcategory=algorithms_subcategory %}

## Chess Annotations

{% assign chess_subcategory = page.subcategories.writeups.chess %}
{% include post_list.html category=writeups_category subcategory=chess_subcategory %}