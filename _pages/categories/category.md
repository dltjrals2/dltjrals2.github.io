---
title: "Category"
layout: archive
permalink: /categories/
author_profile: true
sidebar_main: true
---

<!-- Gitblog -->
{% assign posts = site.categories.Gitblog %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}

<!-- Django -->
{% assign posts = site.categories.Django %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}

<!-- Programmers -->
{% assign posts = site.categories.Programmers %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
