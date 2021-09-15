---
title: "Category"
layout: archive
permalink: /categories/
author_profile: true
sidebar_main: true
---

<nav class="nav__list">
  <input id="ac-toc" name="accordion-toc" type="checkbox" />
  <label for="ac-toc">{{ site.data.ui-text[site.locale].menu_label }}</label>
  <ul class="nav__items" id="category_tag_menu">
    <ul>
      {% for category in site.categories %}
        {% if category[0] == "Algorithm" %}
          <li><a href="/categories/algorithm" class="">Algorithm ({{category[1].size}})</a></li>
          {% endif %}
          {% if category[0] == "AlgorithmRe" %}
          <li><a href="/categories/algorithmRe" class="">Algorithm 복습 ({{category[1].size}})</a></li>
          {% endif %}
          {% endfor %}
    </ul>
    <ul>
      {% for category in site.categories %}
        {% if category[0] == "Chatbot" %}
          <li><a href="/categories/chatbot" class="">Chatbot (with Selenium) ({{category[1].size}})</a></li>
          {% endif %}
      {% endfor %}
    </ul>
    <ul>
      {% for category in site.categories %}
        {% if category[0] == "Gitblog" %}
          <li><a href="/categories/gitblog" class="">Git Blog (with Jekyll) ({{category[1].size}})</a></li>
        {% endif %}
      {% endfor %}
    </ul>

  </ul>
</nav>
