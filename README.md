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
