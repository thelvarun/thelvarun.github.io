---
layout: page
title: writeups
bloomin: "bloomin"
categories:
  writeups: "writeups"
  blogs: "blogs"
tags:
  writeups:
    algorithms: "algorithms"
    chess: "chess"
---
{% assign writeups_category = page.categories.writeups %}

This is a collection of writeups for various different technical fields I find
interesting. This ranges from algorithms to physics problems to chess annotations
and maybe even media analysis. Idk!

## Algorithms

{% assign algorithms_tag = page.tags.writeups.algorithms %}
{% include post_list.html category=writeups_category tag=algorithms_tag %}

## Chess Annotations

{% assign chess_tag = page.tags.writeups.chess %}
{% include post_list.html category=writeups_category tag=chess_tag %}