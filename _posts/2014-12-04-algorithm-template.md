---
layout: post
title: "algorithm template"
description: "算法"
category: 算法
tags: [算法]
---
{% include JB/setup %}


#头文件

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

 
#并查集
1. 合并查找:将2个集合合并成1个集合.(并到一个集合的根上)
2. 种类并查集:将不同种类的放到不同的层次上.
3. 判正误:利用种类并查集分层,然后每输入一句话,就判断是否矛盾.##模板
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
##应用
1. [PAT 1090. Highest Price in Supply Chain](http://www.patest.cn/contests/pat-a-practise/1090)
2. [POJ 食物链](http://www.patest.cn/contests/pat-a-practise/1090)

