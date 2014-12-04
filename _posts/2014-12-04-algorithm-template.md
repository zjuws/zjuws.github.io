---
layout: post
title: "algorithm template"
description: "算法"
category: 算法
tags: [算法]
---
{% include JB/setup %}
#目录

<link rel="stylesheet" href="http://yandex.st/highlightjs/6.2/styles/googlecode.min.css">
 
<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script src="http://yandex.st/highlightjs/6.2/highlight.min.js"></script>
 
<script>hljs.initHighlightingOnLoad();</script>
<script type="text/javascript">
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

##头文件

{% highlight cpp %}

#include <cstdio>
#include <cstring>
#include <cmath>
#include <queue>
#include <stack>
#include <vector>
#include <string>
#include <map>
#include <set>
#include <iostream>
#include <algorithm>
using namespace std;
typedef unsigned long long LL;
const int inf=(1<<30);
const int N=110000;
const double eps=1e-8;
const LL mod = 1000000007;
	
{% endhighlight %} 


- - - -

 
##并查集
1. 合并查找:将2个集合合并成1个集合.(并到一个集合的根上)
2. 种类并查集:将不同种类的放到不同的层次上.
3. 判正误:利用种类并查集分层,然后每输入一句话,就判断是否矛盾.###模板
* 种类并查集
{% highlight cpp %}
int findRoot(int x){
	if(Tree[x]==x) return x;
	int fx=Tree[x];
	Tree[x]=findRoot(Tree[x]);
	f[x]=f[fx]+f[x];
	return Tree[x];
}
void merge(int x,int y，int c) {
    int fx = findRoot(x);
    int fy = findRoot(y);
    if(fx==fy) {
    	if(abs(f[y]-f[x])!=c) return 0;
    	return 1;
    }else {
        Tree[fx] = fy;
        f[fx] = (f[y] - f[x] + c);
        return 1;
    }
}

{% endhighlight %} 
###应用
1. [PAT 1090. Highest Price in Supply Chain](http://www.patest.cn/contests/pat-a-practise/1090)
2. [POJ 食物链](http://www.patest.cn/contests/pat-a-practise/1090)

