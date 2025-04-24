---
layout: page
title: blogs
categories:
  blogs: "blogs"
---
{% assign blogs_category = page.categories.blogs %}
{% include post_list.html category=blogs_category %}