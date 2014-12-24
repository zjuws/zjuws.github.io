---
layout: post
title: "使用Jekyll-bootstrap快速搭建博客"
description: "Jekyll"
category: 博客
tags: [博客]
---
{% include JB/setup %}

<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>


<script type="text/javascript" >
 $(document).ready(function(){
      $("h2,h3,h4,h5,h6").each(function(i,item){
        var tag = $(item).get(0).localName;
        $(item).attr("id","wow"+i);
        $("#category").append('<a class="new'+tag+'" href="#wow'+i+'">'+$(this).text()+'</a></br>');
        $(".newh2").css("margin-left",0);
        $(".newh3").css("margin-left",20);
        $(".newh4").css("margin-left",40);
        $(".newh5").css("margin-left",60);
        $(".newh6").css("margin-left",80);
      });
 });
</script>
<div id="category"></div>



##Jekyll QuickStart

###1.创建GitHub博客

**(1)创建一个新的仓库**


进入你的https://github.com网址，创建一个新的仓库命名为

**USERNAME.github.io**

**(2)安装Jekyll-Bootstrap**

打开终端，进入你想要存放博客的文件夹，输入以下命令：

	git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.io
	cd USERNAME.github.io 
	git remote set-url origin git@github.com:USERNAME/USERNAME.github.io.git 
	git push origin master
	
这几句的意思是，从github上克隆jekyll-bootstrap到本地，然后设置远程仓库为自己建的USERNAME.github.io，将本地内容push到远程仓库。

**(3)打开博客**


几分钟后，你便可以通过网址`http://USERNAME.github.com`访问博客了


###2.本地运行Jekyll

你需要安装`Jekyll ruby gem`才能本地预览你的博客。注意`gem`的相关依赖环境也会被安装。

	$ gem install jekyll
	
记住把`USERNAME`改为你的GitHub上的用户名。

	$ cd USERNAME.github.io
	$ jekyll serve
	
你现在可以通过http://localhost:4000/访问你的博客了。 


###3.创建文章

创建文章可以通过rake指令：

	$ rake post title="Hello World"
	
rake指令会自动创建当前日期的Markdown格式的文件，保存在_posts文件夹下。

###4.创建页面

创建页面可以通过rake指令：

	$ rake page name="about.md"
	
创建一个嵌套的页面：

	$ rake page name="pages/about.md"
	
创建一个页面可以只通过路径：

	$ rake page name="pages/about"
	# 上面指令会创建文件：./pages/about/index.html
	

###5.发布

当你新添了文章或者改变了主题或者其他文件，你可以通过以下指令，上传到Github：

	$ git add.
	$ git commit -m "Add new content"
	$ git push origin master
	
当你成功上传以后，Github会自动部署你的blog，如果有错误你将会收到邮件提醒。

##博客主题

###1.寻找主题

你可以查看官方最新的主题，可以通过以下网址预览。

[Launch Theme Explorer](http://themes.jekyllbootstrap.com)

或者直接在GitHub上查看主题包：

<https://github.com/jekyllbootstrap>

###2.安装主题

通过rake指令和主题的git链接地址来安装主题

	$ rake theme:install git="https://github.com/jekyllbootstrap/theme-the-program.git"
	
如果你从别的方式获得主题包，比如下载了压缩包，你可以把它放到`./_theme_packages文件夹下，使用它的名字来安装：

	$ rake theme:install name="THEME-NAME"
	
###3.转换主题

一旦你安装了主题，你可以通过rake指令装换主题：

	$ rake theme:switch name="the-program"
	
###4.定制主题

主题文件存放在`./_includes/themes/THEME-NAME`下。平时修改的时候最好修改主题文件而不是修改`_layouts`下的文件。因为当你切换主题的时候，`_layouts`下的文件会被重写。


###5.代码高亮
使用 google-code-prettify 来实现代码的高亮显示， prettify 非常小巧且配置简单，使用它来实现代码的高亮显示是个不错的选择。下边我们简单看看 prettify.js 的使用方法：

**引入 jQuery 文件和 prettify.js 文件**

	<scripttype="text/javascript"src="jquery-1.6.1.min.js"></script>
	<scriptsrc="prettify.js"type="text/javascript"></script>

**调用 prettify.js 实现代码高亮**

在 body 标签上添加调用方法，如下：

	<bodyonload="prettyPrint()">
	</body>
	将你需要高亮显示的代码片断放在<pre>标记里，如下：
	<preclass="prettyprint">
    @*你的代码片断*@
	</pre>
	
**使用 jQuery 小技巧实现优化**

上述方法可以实现代码的高亮，但每次手动为<pre>标签添加"prettyprint"类，显示有些麻烦。使用下边的代码片断来解决这个问题并替换掉 body 的"onload"的事件，实现分离：

	$(window).load(function(){
     	$("pre").addClass("prettyprint");
     	prettyPrint();
	})
	
[google代码样式下载地址](http://jmblog.github.io/color-themes-for-google-code-prettify/)

##Jekyll基本结构

Jekyll的核心其实就是一个文本的转换引擎，用你最喜欢的标记语言写文档，可以是Markdown、Textile或者HTML等等，再通过layout将文档拼装起来，根据你设置的URL规则来展现，这些都是通过严格的配置文件来定义，最终的产出就是web页面。
基本的Jekyll结构如下：

	|-- _config.yml  
	|-- _includes  
	|-- _layouts  
	|   |-- default.html  
	|   `-- post.html  
	|-- _posts  
	|   |-- 2014-12-23-why-every-programmer-should-play-nethack.md 
	|   `-- 2014-12-24-merryChrismas.md 
	|-- _site  
	`-- index.html  
	
简单介绍一下他们的作用：

**_config.yml**

配置文件，用来定义你想要的效果，设置之后就不用关心了。

**_includes**

可以用来存放一些小的可复用的模块，方便通过{ % include file.ext %}（去掉前两个{中或者{与%中的空格，下同）灵活的调用。这条命令会调用_includes/file.ext文件。

**_layouts**

这是模板文件存放的位置。模板需要通过YAML front matter来定义，后面会讲到，{ { content }}标记用来将数据插入到这些模板中来。

**_posts**


你的动态内容，一般来说就是你的博客正文存放的文件夹。他的命名有严格的规定，必须是2014-12-22-artical-title.MARKUP这样的形式，MARKUP是你所使用标记语言的文件后缀名，根据_config.yml中设定的链接规则，可以根据你的文件名灵活调整，文章的日期和标记语言后缀与文章的标题的独立的。

**_site**


这个是Jekyll生成的最终的文档，不用去关心。最好把他放在你的.gitignore文件中忽略它。
