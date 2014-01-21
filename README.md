#jekyll怎样创建一篇文章
##第一步，创建项目。
在你的电脑上，建立一个目录，作为项目的主目录。我们假定，它的名称为jekyll_demo。  
>mkdir jekyll_demo


对该目录进行git初始化。  
>cd jekyll_demo
>
>git init


然后，创建一个没有父节点的分支gh-pages。**因为github规定，只有该分支中的页面，才会生成网页文件**。  
>$ git checkout --orphan gh-pages


以下所有动作，都在该分支下完成。


##第二步，创建设置文件。
在项目根目录下，建立一个名为_config.yml的文本文件。它是jekyll的设置文件，我们在里面填入如下内容，其他设置都可以用默认选项，具体解释参见[官方网页](https://github.com/jekyll/jekyll/wiki/Configuration)

>baseurl: /jekyll_demo


目录结构变成：
<pre>
　/jekyll_demo
　　　　|--　_config.yml
</pre>


##第三步，创建模板文件。  
在项目根目录下，创建一个_layouts目录，用于存放模板文件。  
>$ mkdir _layouts


进入该目录，创建一个default.html文件，作为Blog的默认模板。并在该文件中填入以下内容。  
```html
　<!DOCTYPE html>
　　<html>
　　<head>
　　　　<meta http-equiv="content-type" content="text/html; charset=utf-8" />
　　　　<title>{{ page.title }}</title>
　　</head>
　　<body>

　　　　{{ content }}

　　</body>
　　</html>
```

Jekyll使用[Liquid](https://github.com/shopify/liquid/wiki/liquid-for-designers)模板语言，{{ page.title }}表示文章标题，{{ content }}表示文章内容，更多模板变量请参考[官方文档](https://github.com/jekyll/jekyll/wiki/Template-Data)。

目录结构变成：
<pre>
　/jekyll_demo
　　　　|--　_config.yml
　　　　|--　_layouts
　　　　|　　　|--　default.html
</pre>


##第四步，创建文章。

回到项目根目录，创建一个_posts目录，用于存放blog文章。
>mkdir _posts

进入该目录，创建第一篇文章。文章就是普通的文本文件，文件名假定为2014-01-21-hello-world.html。(注意，文件名必须为"年-月-日-文章标题.后缀名"的格式。如果网页代码采用html格式，后缀名为html；如果采用markdown格式，后缀名为md。）


在该文件中，填入以下内容：（注意，行首不能有空格）
```html
　---
　　layout: default
　　title: 你好，世界
　　---
　　<h2>{{ page.title }}</h2>
　　<p>我的第一篇文章</p>
　　<p>{{ page.date | date_to_string }}</p>
```


每篇文章的头部，必须有一个yaml文件头，用来设置一些元数据。它用三根短划线"---"，标记开始和结束，里面每一行设置一种元数据。"layout:default"，表示该文章的模板使用_layouts目录下的default.html文件；"title:你好，世界"，表示该文章的标题是"你好，世界"，如果不设置这个值，默认使用嵌入文件名的标题，即"hello world"。  
在yaml文件头后面，就是文章的正式内容，里面可以使用模板变量。{{ page.title }}就是文件头中设置的"你好，世界"，{{ page.date }}则是嵌入文件名的日期（也可以在文件头重新定义date变量），"| date_to_string"表示将page.date变量转化成人类可读的格式。  
目录结构变成：
<pre>
　/jekyll_demo
　　　　|--　_config.yml
　　　　|--　_layouts
　　　　|　　　|--　default.html 
　　　　|--　_posts
　　　　|　　　|--　2012-08-25-hello-world.html
</pre>


##第五步，创建首页。
有了文章以后，还需要有一个首页。  
回到根目录，创建一个index.html文件，填入以下内容。  
```html
---
　　layout: default
　　title: 我的Blog
　　---
　　<h2>{{ page.title }}</h2>
　　<p>最新文章</p>
　　<ul>
　　　　{% for post in site.posts %}
　　　　　　<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
　　　　{% endfor %}
　　</ul>
```

它的Yaml文件头表示，首页使用default模板，标题为"我的Blog"。然后，首页使用了{% for post in site.posts %}，表示对所有帖子进行一个遍历。这里要注意的是，Liquid模板语言规定，输出内容使用两层大括号，单纯的命令使用一层大括号。至于{{site.baseurl}}就是_config.yml中设置的baseurl变量。  
目录结构变成：  
<pre>
　/jekyll_demo
　　　　|--　_config.yml
　　　　|--　_layouts
　　　　|　　　|--　default.html 
　　　　|--　_posts
　　　　|　　　|--　2014-01-21-hello-world.html
　　　　|--　index.html
</pre>


##第六步，发布内容。

现在，这个简单的Blog就可以发布了。先把所有内容加入本地git库。  
>git add .
>
>git commit -m "first post"


然后，前往github的网站，在网站上创建一个名为jekyll_demo的库。接着，再将本地内容推送到github上你刚创建的库。注意，下面命令中的username，要替换成你的username。

>git remote add origin git@github.com:username/jekyll_demo.git
>
>git push origin gh-pages



上传成功之后，等10分钟左右，访问<http://username.github.io/jekyll_demo/>就可以看到Blog已经生成了（将username换成你的用户名）。


all right : click me <http://cinowu.github.io/jekyll_demo/>


ps:article from runyifeng's [blog](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)


##btw
在往github推送之前，可以在本地通过jekyll启动测试一下  
可能会出现编码问题，更改vi _config.yml
<pre>
encoding: utf-8
#baseurl: /jekyll_demo
</pre>

在jekyll-demo目录下执行启动  
<pre>
$ jekyll build
Configuration file: f:/jekyll_demo/_config.yml
            Source: f:/jekyll_demo
       Destination: f:/jekyll_demo/_site
      Generating... done.

Administrator@PC121106ZS04 /f/jekyll_demo (gh-pages)
$ jekyll serve
Configuration file: f:/jekyll_demo/_config.yml
            Source: f:/jekyll_demo
       Destination: f:/jekyll_demo/_site
      Generating... done.
    Server address: http://0.0.0.0:4000
  Server running... press ctrl-c to stop.
</pre>

访问<http://localhost:4000>


##最后
我们可以参考下<https://github.com/jekyll/jekyll/wiki/sites>,这个链接中列举了许多用jekyll创建的github博客


##附加几种常用博客系统说明
###Jekyll 和 Octopress
Jekyll 和 Octopress 本是同源，所以本质上差别不大。Octopress 是基于 Jekyll 的。他们的关系就像 jQuery 与 js 的关系一样。  
jekyll建议用jekyll-bootstrap框架<https://github.com/rose1988c/jekyll-bootstrap>
Jekyll本是blog系统，但与wordpress不同，Jekyll是为静态文件blog而设计的，本地安装Jekyll后，可以使用jekyll   --server开启一个blog服务，也可以自己配置服务。jekyll的厉害之处也是她的原理是可以把markdown、textile等模板语言直接转换成html，还能像使用php等语言一样使用模板、引入模块，使用插件。markdown\textile语言是非常友好和方便的，是很多开发者非常喜爱和常用的静态模板语言，用于写博客和记录信息非常清晰便利。  
Github支持Jekyll（同一开发者），在内部已经配置好了Jekyll，允许用户把Jekyll博客当作一个项目发布到Github，博客的所有文件通过git管理，永远都不会丢失，只要用户提交，Github自动会调用Jekyll，为用户把md和textile转换成可访问的html，所以用户也可以不在本地先转换格式。另外Github为用户的博客项目配置独立服务，使得可以以<http://username.github.io>方式访问，更可以设置CNAME指定另外的域名，还有很多功能，自己去发掘吧。  
可以简单点理解，Jekyll是一套博客系统，而Github提供博客托管服务  
<http://yanping.me/cn/blog/2013/08/11/github-pages-step-by-step-video/>


**ekyll 和 Octopress上传发布细微区别:**
jekyll 可以直接用git上传 .md or .html 文件不需要编译  
Otcopress是makefile将文件转化.html发布。说的有点复杂了，其实就是编译成一个静态网站。  
Octopress 文章多了编译难免很慢  



###Hexo框架（Nodo.js）  
又多了一种选择。  
<http://zespia.tw/hexo/>
<https://github.com/tommy351/hexo>


###pelican（python）
<http://www.lizherui.com/pages/2013/08/17/build_blog.html#comment-1082841744>




当你开始搭建一个个人博客的时候，会出现类似这么多的博客系统干扰你，我要坚持搞好jekyll!!!



##接下来jekyll要做的事情
- 选择主题 
- 加上第三方评论系统
- 接入新浪腾讯微博
- 加入Google Analytics
- 加入Google Webmasters
- 申请域名，更换域名

参考<http://www.lizherui.com/pages/2013/08/17/build_blog.html#comment-1082841744>




