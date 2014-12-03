---
layout: page
title: Technolog Blog
tagline: 个人博客
---
{% include JB/setup %}

`我的CSDN博客：` [ACM术](http://blog.csdn.net/fnzsjt)

![hello](http://zjuws.githu.io/images/go.jpg)

### 每日一句

>  励志名言&gt;在我们的生活中最让人感动的日子总是那些一心一意为了一个目标而努力奋斗的日子，哪怕是为了一个卑微的目标而奋斗也是值得我们骄傲的，因为无数卑微的目标积累起来可能就是一个伟大的成就。金字塔也是由每一块石头累积而成的，每一块石头都是很简单的，而金字塔却是宏伟而永恒的。 
	

    
### 博客文章

`博客文章列表.`

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

### 关于

个人博客，仅供学习使用。


