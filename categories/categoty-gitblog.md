---
title: "Blog"
layout: archive
permalink: categories/gitblog
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories.Gitblog %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}
