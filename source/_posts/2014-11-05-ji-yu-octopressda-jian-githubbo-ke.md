---
layout: post
title: "基于octopress搭建github博客"
date: 2014-11-05 11:01:23 +0800
comments: true
categories: github Octopress
---
发现网上很多牛人的博客都架设在[github](http://github.com)上，而我也一直在寻求一个属于自己的、可定制的、安静的环境来写博客。于是决定也来尝试在github下的博客搭建。通过查找资料并对比了一下，我决定用octopress来搭建。

##搭建过程

###0. 电脑环境

我用的电脑是Mac Mini，系统为OS X Yosemite.以下没有特别说明下，都是在此电脑配置环境下操作。虽然下面很多操作跟系统无关或者在Windows系统下操作大同小异，但不能够保证不同系统下操作完全一样，谨此提醒。

###1. 安装Git及Ruby

####1.1 安装Git
这个比较简单，一般不会出现什么差错，只需要[点击这里](http://git-scm.com)到Git官方网站按照提示下载就可以了。当然，官方网站是英语。但没关系，我想英语再差的人也能正确下载下来的。好吧，我就是一个明证。英语差如我都能，还有什么犹豫的，赶紧滴。放张图片在这里。 

![Git](http://ww4.sinaimg.cn/large/a15b4afegw1emczvwz3tnj209k0a6aam)

####1.2 安装Ruby
因为OS X Yosemite系统已经默认安装了Ruby 2.0.0，所以这步其实不需要。但是对于Mac下的非Yosemite系统和Windows用户，此步必不可少。以下简单说说在Mac下安装Ruby。打开终端（Terminal）执行如下命令:

    curl -L https://get.rvm.io | bash -s stable --ruby
    rvm install 2.0.0
    rvm use 2.0.0
    rvm rubygems latest
    
 安装完毕后，执行命令查看版本号，确保安装成功:
 
    ruby --version
    
###2. 安装Octopress

安装octopress具体步骤可以参考[官网的说明](http://octopress.org/docs/setup/)（当然啦，这也是E文。不过用点心还是可以看懂的）。好吧，简单来说就是打开终端（Terminal）然后一步步执行以下命令：

     git clone git://github.com/imathis/octopress.git octopress #复制Octopress到本地
     cd octopress         #转到本地Octopress目录 
     gem install bundler  # 安装依赖项
     bundle install       # 安装依赖的组件
     rake install         # 安装默认的Octopress主题
     
至此，本地安装Octopress完成。此时如需预览可执行命令：

     rake preview
     
然后打开你的浏览器输入: [http://localhost:4000/](http://localhost:4000/) ，现在可以开心预览了。记住啦，以后任何时候，比如对博客作了修改后想在本地预览效果，那就执行以上操作吧。



###3. 本地个性化Octopress

我参考了很多网上的关于搭建Octopress教程，它们都是把这步放到最后。但是我认为，先在本地对Octopress作一些简单而必要的设置有利于对Octopress操作的理解，而且也符合我们做事情的一般步骤。比如我们做作业，拿到一本新作业本后是不是要在作业本上写上姓名、科目和座号等，以表示这本作业的归属权？！这步就相当于我们在作业本上写上自己的名字，宣布主权。下面我们开始吧。

####3.1 安装新主题

只要接触过QQ空间或者wordpress的人都知道，可以通过换主题来让博客用上自己喜欢的style。Octopress也可以这样，比如 [这里](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)就有一些第三方主题。可以通过执行一些简单的命令来安装新主题，如下：

     cd octopress
     git clone git://github.com/macjasp/cleanpress.git .themes/cleanpress
     rake install['主题名称']
     rake generate
     
以上的命令是不是看着很熟，没错，它跟我们安装Octopress时的命令大同小异。好了，没忘记怎么预览吧？好好预览下，我们马上进入个性化设置、签名步骤啦。

####3.2 基本配置

这里只对我个人也就是本博客的设置（其实基本上包括了各方面的日常设置）作说明。

- #####域名，网站标题，作者及链接形式

先在octopress文件夹下找到 _config.yml 文件，用文本编辑器打开它，你会发现如下信息：

     url: http://yoursite.com  #这里改为你网站的域名，这个可以先不管
     title: My Octopress Blog  #这里改为你想要的网站标题，比如互联网角落
     subtitle: A blogging framework for hackers. #这里改为你的博客副标题
     author: Your Name #这里签上你的大名吧，以后它就是你的了。
     simple_search: https://www.google.com/search #默认搜索引擎，国内不翻表示很蛋疼
     description: #网站描述信息

提醒一下，上面的冒号后必须是英文冒号而且还要空格哦。继续往下看，找到：

     permalink: /blog/:year/:month/:day/:title/ 
    
修改为

     permalink: /blog/:year:month:day/:title/ 
     
这个是为了让链接层级不要太多，不然到时找起来就多麻烦，简单就是美嘛。


- #####修改Markdown文件后缀及解释器，添加MathJax支持

首先打开rakefile文件（同样是在octopress目录下），找到代码：

    new_post_ext    = "markdown"  # default new post file extension when using the new_post task
    new_page_ext    = "markdown"  # default new page file extension when using the new_page task

修改为：

    new_post_ext    = "md"  # default new post file extension when using the new_post task
    new_page_ext    = "md"  # default new page file extension when using the new_page task
    
下面来修改Markdown解释器为kramdown。相比Octopress默认的Mrakdown解释器rdiscount，kramdown支持Multi Markdown（好吧，这个我也不知道是什么）语法和LeTeX（很强大的样子，对吧），而且更快（这是重点）。好了，看步骤吧：

首先打开Gemfile文件并在最后添加一行：

     gem 'kramdown'
     
然后执行命令：

     sudo bundle install
     
再回到 _config.yml 文件，找到：

     markdown: rdiscount
     rdiscount:
     extensions:
       - autolink
       - footnotes
       - smart
       
把其中的rdiscount改为kramdown并删掉或者用#注释掉下面几行。

最后我们来添加MathJax支持，这可以让我们输入高大上的公式哦。这么高大上的功能只需要我们在 source/_includes/custom/head.html 文件内添加如下代码：

    <!-- MathJax -->
    <script type="text/x-mathjax-config">
      MathJax.Hub.Config({
        tex2jax: {
          inlineMath: [ ['$','$'], ["\\(","\\)"] ],
          processEscapes: true
        }
      });
    </script>
    <script type="text/x-mathjax-config">
       MathJax.Hub.Config({
         tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
       }
     });
    </script>
    <script type="text/x-mathjax-config">
       MathJax.Hub.Queue(function() {
           var all = MathJax.Hub.getAllJax(), i;
           for(i=0; i < all.length; i += 1) {
              all[i].SourceElement().parentNode.className += ' has-jax';
        }
      });
    </script>
    <script type="text/javascript"
       src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
    </script>

这样我们就可以输入LaTex语法的公式啦，来看我的一条高大上公式：

 $$
f'\left( x\right) = \lim _{x\rightarrow 0}\dfrac {f\left( x+\Delta x\right) - f\left( x\right)}{\Delta x}
 $$
 
 代码为：
 
         $$
         f'\left( x\right) = \lim _{x\rightarrow 0}\dfrac {f\left( x+\Delta x\right) - f\left( x\right)}{\Delta x}
         $$
         
是不是不明觉厉啊，好吧，你不孤单，我也是。

- ##### 页面设置

Octopress默认只有Blog和Archives（归档）两个页面，我们可以执行以下命令来生成更多的页面：

    sudo rake new_page[your-title]
    
你会发现执行完后，在octopress目录source文件夹下出现一个名为 your-title 的文件夹，里面会有一个名为index.md的文件。把想写的内容写进这个文件，重新部署后这个文件会生成一个名为index.html的文件，然后就可以用以下链接访问你之前写进去的内容了： http://[your_domain]/your-title/ 

- ##### 发布文章

执行以下命令后：
     
     cd octopress
     rake new_post["Post Title"]
     
会在octopress/source/_posts目录下生成一个名类似为yyyy-mm-dd-Post-Title.md的文件，用Markdown编辑器（推荐 [Mou](http://25.io/mou/)）打开进行编辑。打开后会有一些信息，不用管它，我们把文章写在它下面就是。编辑完后，使用以下命令发布：

    rake gen_deploy

- ##### 404页面及导航栏、网站底部

在source文件夹下新建一个名为404.md文件，打开并粘贴404页面代码即可。关于404页面代码，我用的是腾讯404公益页面，具体操作可参考 [这里](http://www.qq.com/404/)。

打开 source/_includes/custom/navigation.html 可以在这个文件内添加页面到导航栏。
     
     <ul class="main-navigation">
       <li><a href="{{ root_url }}/">Blog</a></li>
       <li><a href="{{ root_url }}/blog/archives">Archives</a></li>
     </ul>
     
比如需要添加页面your-title，只需要在第三行下添加代码即可，添加后代码如下：

     <ul class="main-navigation">
       <li><a href="{{ root_url }}/">Blog</a></li>
       <li><a href="{{ root_url }}/blog/archives">Archives</a></li>
       <li><a href="{{ root_url }}/your-title">your-title</a></li>
     </ul>
     
 注： "{{ root_url }}"为网站根目录。
 
 网站底部只需直接打开 source/_includes/custom/foot.html 修改即可。
 
 - ##### 评论功能
 
 Octopress默认支持Disqus，只需要到 [disqus.com](http://disqus.com)注册帐号，然后修改 _config.yml 文件中以下内容即可：
 
     # Disqus Comments
     disqus_short_name: 
     disqus_show_comment_count: false 
     
关于评论功能，也可以使用友言，具体操作可以自行搜索或者参考本文后面提供的相关链接。
     
###4. 创建github帐户

之前我们都是在本地搭建和预览博客，现在，我们可以考虑上传到网络上啦。首先当然要有高大上的Github帐户。赶紧[点我](https://github.com)去注册吧。

###5. 部署本地Octopress至github帐户

登陆Github账号，然后点击面面右边的『+ new reposltory』按钮或者点击左上角的加号再在弹出菜单中点击 New repository ，就会跳转到一个新建库（Create new repository）页面，在Repsitory name一栏填上[your_username].github.io，其中[your_username]是你Github的用户名，此处必须与你的用户名保持一致。完成后点击『Create repository』按钮提交。然后会出现一个页面，有一个SSH地址，形如

    git@github.com:[your_username]/[your_username].github.io.git 
    
下一步我们会用到该地址。

接下来我们打开终端（Terminal）执行命令：

     cd octopress
     rake setup_github_pages
     
然后把前面的SSH地址粘贴上来，回车继续执行命令：

     rake generate    
     rake deploy
     
其中第一行命令用来生成页面，第二行命令用来部署页面。

现在可以使用http://[your_username].github.io/来访问你的页面啦。赶紧试试。

最后，我们要把源文件发布到source分支下，执行以下命令即可：
       
     git add .
     git commit -m "message"
     git push origin source
     
以后我们每次对博客修改后都要最后执行下上面的命令，这样博客才能实现本地的效果。

- ####绑定域名

   这个这里就不介绍啦，Github官方有介绍的，可以参考下。

- ####发布文章
  
  采用以下命令新建文章：
  
      cd octopress
      rake new_post["标题"]
      
  以下命令发布文章：
  
       rake gen_deploy
       
       
###6. 最后说几句

这是博客的第一篇文章，不管怎么样，总算是完成了。写得虽然断断续续，但是写作的过程中，我对如何应用Octopress搭建博客有了更深一层的理解，虽然谈不上什么，但是我希望这篇文章除了作为我个人学习用Octopress来搭建博客的笔记外，也能够对后来者能有一点点的帮助。

写作这篇文章的时候，我拜读了互联网上很多的关于如何搭建的博文，获益良多，在此感谢他们。我介绍的搭建过程可能会跟很多文章介绍的步骤有些出入，但是个人认为本文的搭建过程更符合我们的习惯，至少是符合我的。

###7. 相关链接

1. [http://shengmingzhiqing.com/blog/octopress-tutorials-toc.html/](http://shengmingzhiqing.com/blog/octopress-tutorials-toc.html/)
2. [http://biaobiaoqi.github.io/blog/2013/07/10/decorate-octopress/](http://biaobiaoqi.github.io/blog/2013/07/10/decorate-octopress/)