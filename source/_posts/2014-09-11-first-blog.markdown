---
layout: post
title: "基于GitHub + Octopress博客搭建与MarkDown语法初试"
date: 2014-09-11 00:53:12 +0800
comments: true
categories: Octopress
---
通过GitHub + Octopress搭建了这个静态博客。替换了默认模板的favicon。希望可以坚持写下去。主要是记录独立游戏的开发过程，希望能够对自己有益，如果恰巧也能对他人有益，就更好了。

现在还不太了解Markdown的语法，默认的模板也不太喜欢。文章的排版和博客样式统统都要改改改。不管内容，首先还是要好看。
***
第二日更新： 

在[献给写作者的 Markdown 新手指南](http://www.jianshu.com/p/q81RER)中介绍了主要的一些Markdown语法，看起来很有帮助。原来MarkDown的语法如此简单直接。 

#### 例如插入图片，使用[Image Tag](http://octopress.org/docs/plugins/image-tag/)：  

{% raw %}
```
{% img /images/A-Team-Logo.png 200 %}
```
{% endraw %}

**其中图片是放在images路径下。**

{% img /images/A-Team-Logo.png 200 %} 

#### 插入代码的方法，使用[Sharing Code Snippets](http://octopress.org/docs/blogging/code/)

{% raw %}
	```
	{% img /images/A-Team-Logo.png 200 %}
	```
{% endraw %}

***  

第三日更新：  
  
今天尝试了修改博客的样式和布局，增加了About和Categories.


>参考资料：  
1. [利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)  
2. [象写程序一样写博客：搭建基于github的博客](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/)  
3. [献给写作者的 Markdown 新手指南](http://www.jianshu.com/p/q81RER)  
4. [Markdown语法示例](http://equation85.github.io/blog/markdown-examples/)  
5. [Sharing Code Snippets](http://octopress.org/docs/blogging/code/)  
6. [灵魂机器 我的Octopress配置](http://cn.soulmachine.me/blog/20130402/)  
7. [Show Categories and Post Count in Octopress](http://vigodome.com/blog/2011/12/22/show-categories-and-post-count-in-octopress/)  
8. [Inserting Liquid Syntax Into Octopress Codeblock](http://jimpravetz.com/blog/2011/12/inserting-liquid-syntax-into-octopress-codeblock/)