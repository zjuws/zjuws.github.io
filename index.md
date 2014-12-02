---
layout: page
title: Technolog Blog
tagline: 个人博客
---
{% include JB/setup %}

我的CSDN博客： [ACM术](http://blog.csdn.net/fnzsjt)

## 哦哦&玉米
	属于我们2个人的博客

    
## 博客文章

以下是博客列表.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## 关于

个人博客，仅供学习使用


