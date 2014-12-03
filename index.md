---
layout: page
title: Technolog Blog
tagline: 个人博客
---
{% include JB/setup %}

`我的CSDN博客：` [ACM术](http://blog.csdn.net/fnzsjt)


![image](http://bolgimage.qiniudn.com/go.jpg)


### 每日一句

>  乔布斯&gt;The only way to do great work is to love what you do. If you haven't found it yet, keep looking. Don't settle. As with all matters of the heart, you'll know when you find it.
成就一番伟业的唯一途径就是热爱自己的事业。如果你还没能找到让自己热爱的事业，继续寻找，不要放弃。跟随自己的心，总有一天你会找到的。 
	

    
### 博客文章

`博客文章列表.`

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

### 关于

个人博客，仅供学习使用。


