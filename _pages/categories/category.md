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
      <!-- etc 카테고리 글들을 모아둔 페이지인 /category/etc 주소의 글로 링크 연결 -->
      <!-- category[1].size로 해당 카테고리를 가진 글의 갯수 표시 -->
      {% for category in site.categories %}
        {% if category[0] == "Gitblog" %}
          <li><a href="/categories/gitblog" class="">Git Blog (with Jekyll) ({{category[1].size}})</a></li>
        {% endif %}
      {% endfor %}
    </ul>
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
  </ul>
</nav>
