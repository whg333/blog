---
layout: post
title: win7本地安装jekyll最新版3.0.1的问题与解决
description: "win7本地安装jekyll最新版3.0.1的问题与解决"
category: [技术]
tags: [github, jekyll, win7]
comments: true
---

## GitHub+jekyll搭建个人博客
先根据[《创建GitHub技术博客全攻略》](http://blog.csdn.net/renfufei/article/details/37725057/)去做一些前期GitHub上的测试准备工作，然后再根据[《使用 GitHub, Jekyll 打造自己的免费独立博客》](http://blog.csdn.net/on_1y/article/details/19259435)这篇文章直接fork了该博主的[StrayBirds](https://github.com/minixalpha/StrayBirds/tree/gh-pages)项目来快速搭建了自己的博客

虽说可以直接在_post目录下编写博客文章，但是如果每次都使用GitHub在线编辑md(markdown)文件，然后查看生成的html，不行再去修改的话，效率太低，且又是由于网络原因，导致在线编辑md文件延时缓慢，所以最好还是执行如下图所示维护流程（[图片源自此处](http://www.pchou.info/web-build/2013/01/03/build-github-blog-page-01.html)）：
![jekyll_flow](http://www.pchou.info/assets/img/build-github-blog-page-01-img0.png)

即先在本地写完md文件，然后本地编译预览，没问题后使用git push上去即可

本人使用Eclipse的git插件去push修改的，虽说Eclipse（JEE的Mars版）对于md也有预览html功能，但是好像不是太完美（尤其是对嵌入java代码的浏览有问题）：
![eclipse_md](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144906092208.png)

所以Win7系统下直接使用[《认识与入门 Markdown》](http://sspai.com/25137)这篇文章推荐的[MarkdownPad2](http://www.markdownpad.com/)编辑器：
![MarkdownPad2_md](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144906092225.png)

下面就把在本地搭建**jekyll最新版3.0.1**的过程说一下

## 安装Ruby和DevKit
首先是Ruby和DevKit的安装，因为使用的是Win7系统，所以使用RubyInstaller安装的Ruby和Devkit，都在[这里](http://rubyinstaller.org/downloads/)下载

使用的是该页面的推荐Ruby 2.1.X，即Ruby 2.1.7版本，然后对应的DevKit使用**DevKit-mingw64-32-4.7.2-20130224-1151-sfx.exe**这个

首先安装Ruby 2.1.7，在Win7下直接Next比较简单，注意勾选上添加Ruby进系统Path以及管理rb后缀名为Ruby文件，安装好了以后就可以在命令行使用ruby命令了

然后DevKit其实是使用7-Zip提取文件，然后cd到你指定的提取文件的存放目录，使用

>ruby dk.rb init

和

>ruby dk.rb install

安装完毕DevKit，这个时候gem命令也可以使用了

## 安装和启动jekyll
接下来就是使用
>gem install jekyll

来安装jekyll了，但是如果出现网络异常等错误的话，那就需要重新设置一下RubyGems源为[淘宝网镜像地址](https://ruby.taobao.org/)了：
![RubyGems_Net_Error1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144904868259.jpg)

![RubyGems_Net_Error2](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144904904153.jpg)

![RubyGems_taobao](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144904904138.png)

确保正确安装jekyll后可使用如下命令启动jekyll
>jekyll serve --safe --watch

## jekyll常见问题
但人生十有八九不如意，更何况身处更新换代如此频繁的IT界，所以你可能会遇到如下这几个问题

### 1、启动jekyll的警告
因为安装jekyll时没有指定版本，所以默认安装最新版，例如我安装的是3.0.1版本的
![jekyll_3.0.1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144904873329.jpg)

因为是最新版本，所以其jekyll的_config.yml配置文件的选项名字有变化，这个时候jekyll会报警告，提示过期了让你更新配置名字：
![jekyll_warm1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144904873338.jpg)

前2个过期（Deprecation）警告只需要根据提示把更新\_config.yml里面的配置名字即可，但是下面还有1个警告：没有包含（include）jekyll-paginate进Gemfile里面，解决方案是添加配置名为gems且值为jekyll-paginate进配置文件里面：

![jekyll_warm2](http://cejdh.img46.wal8.com/img46/533449_20151202165458/14490494151.png)

### 2、启动jekyll依赖包错误
正如上面提到的，警告没有了以后，在启动的时候还会由于缺少一些组件依赖包而导致的启动报错，而且jekyll内部的一些功能也必须依赖于这些插件包，想正确启动并应用就必须安装上这些依赖包：

>gem install kramdown //markdown语言解析依赖包
>
>gem install pygments.rb //代码高亮依赖包
>
>gem install liquid //模板引擎依赖包

如上的依赖包是根据[采用Jekyll + github + pygments构建个人博客的最终说明](http://www.jianshu.com/p/609e1197754c)提到的来安装的，但其实还有一个jekyll支持的功能插件未被提及到——分页

### 3、jekyll分页不显示
注意如果使用如下命令来启动jekyll
>jekyll serve --safe --watch

并不会报分页依赖包错误。。。而是正常启动jekyll成功，但是分页功能没效果！具体原因本人也不明，也不知道是不是由于safe参数隐藏了该报错？

后来是使用了如下这个命令来启动jekyll才发现该问题的（trace参数也可以不加）
>jekyll serve --trace

最初是由于分页没效果，想找一个在页面打印其paginator关联值的方法，但没找到却找到了上面那条带trace参数的监控命令。。。

报错如下图所示：
![jekyll_page1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905011657.jpg)

然后安装一下jekyll-paginate即可：
>gem install jekyll-paginate

![jekyll_page2](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905174735.jpg)

### 4、Win平台警告解决
上图还有最后一个警告：提示让把如下命令添加到Gemfile里面：
>gem 'wdm', '>= 0.1.0' if Gem.win_platform?

但其实由于对Ruby的不了解，连Gemfile的具体位置和作用都没搞明白，甚至还自己在该blog项目目录下创建了一个Gemfile文件，但还是不行。。。

后来根据第1条在配置文件\_config.yml添加的gems配置名，猜测估计就是所谓的gems的依赖项，所以把wdm添加进去_config.yml后的gems配置变为：
>gems: ["jekyll-paginate", "wdm"]

然后再次尝试启动后发现报错：
![win_error1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905704118.jpg)

然后同样安装一下wdm依赖包即可：
![win_error2](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905704134.jpg)

如此一来本地正常启动jekyll没有任何报错了：
![win_error3](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905704142.jpg)

### 5、中文目录资源地址不能访问
启动后测试分页功能以及编写博客文章等功能都是没有问题的：
![chinese_error1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144913543594.png)

但是还有一个问题是带有中文目录资源地址不能访问，这个问题在GitHub是没有的，只有在本地jekyll才会出现。。。例如点击上图博客文章《Java中的并发》，由于其分类是中文“技术”，然后访问报错：
![chinese_error2](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905808582.png)

解决方案参考了[Jekyll编译中文文件名的网页的本地预览问题。](http://www.oschina.net/question/1396651_132154?fromerr=2DiTgNyt)，就是对jekyll访问中文目录资源处理做了下UTF-8转码：
![chinese_error3](http://cejdh.img46.wal8.com/img46/533449_20151202165458/1449058142.jpg)

![chinese_error4](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144905859586.jpg)

如此一来本地jekyll也可以正常访问中文目录资源地址了：
![chinese_error5](http://cejdh.img46.wal8.com/img46/533449_20151202165458/14490580864.png)

### 6、其他注意事项
![ps](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144913481766.jpg)

## 后记（2015.12.3）
之后又做了一些完善博客的事情，主要包括

### 1、添加多说评论系统
前面也提到本博客是fork [StrayBirds](https://github.com/minixalpha/StrayBirds/tree/gh-pages)项目的，但是后来发现其实该项目只支持国外比较著名的[Disqus](http://www.disqus.com/)评论系统，但是目前访问该网站又转跳到是基于https安全访问的，导致SSL连接出错：

![disqus_error](http://cejdh.img46.wal8.com/img46/533449_20151202165458/14491344803.png)

不得已只能转向国内的评论系统了。。。国内的看了下结合jekyll较有名有[多说](http://duoshuo.com/)和[友言](http://www.uyan.cc/)，然后发现其实使用的主题[kunka](http://www.zhanxin.info/jekyll/2013-08-11-jekyll-theme-kunka.html)本身就支持多说评论，只是StrayBirds项目修改导致只支持Disqus的，期间参考了[Jekyll+多说，建立属于你的轻博客](http://www.ituring.com.cn/article/114888)，然后做出如下修改即可：修改comment.ext令其根据配置也支持多说评论：
![kunka_duoshuo](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144913448043.png)

然后最新的多说评论框div也有变化了，防止出现评论乱串等问题，添加了一些属性，例如必要的data-thread-key属性，如果没有的话，使用Chrome的控制台会看到报错：
![ds_attr](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144914760076.png)

然后最终的修改如下图所示，把上面那行的改为下面那行即可，注意此处的site.domainname是在_config.yml里面自定义的全局变量名，值为www.iclojure.com，代表本博客地址的[主域名](http://www.iclojure.com)
![ds_thread](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144914029381.png)

然后配置里面添加上多说，注意账号改为你自己的多说账号：
![](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144913448058.png)

### 2、多余或者失效的https访问
除了前面提到过的Disqus是访问是https外，后来发现StrayBirds项目使用的jquery1.8.3版本的链接竟然也是https连接新浪库房的，这些都导致打开页面的时候Chrome状态栏提示“正在建立安全连接/正在打开隧道”，最要命的是那个基于https新浪库房的jquery1.8.3在我这还是不可用的（难道是公司网络问题吗？）。。。后来直接改为百度库房的了：
>https://lib.sinaapp.com/js/jquery/1.8.3/jquery.min.js

改为
>http://libs.baidu.com/jquery/1.8.3/jquery.min.js

另外还有头像的设置，StrayBirds项目直接使用的是GitHub上的头像，很不幸，也是https访问的，有时网络不好的时候头像就空白了。。。所以我在自己博客项目目录下传个头像文件，改为http的访问即可

然后还发现了有个链接：
>netdna.bootstrapcdn.com

也是一直在做请求，查了一下才知道原来是[Font Awesome——最流行最全面最优秀的字体图标](http://www.58img.com/web/807)，博客文章的发布日期图标、分类图标等都是直接以形如
**class="icon-calendar"**或**class="icon-folder-open"**的方式编写然后在界面上展示出来的，具体支持的图标可看[这里](http://dev.styler.jp/bootstrap-plus.html)、[这里](http://www.bootcss.com/p/font-awesome/)还有[这里](http://test.h-ui.net/Hui-3.7-iconfont.shtml)

### 3、简单SEO
具体参考了[Github Pages + Jekyll搭建博客之SEO](http://zyzhang.github.io/blog/2012/09/03/blog-with-github-pages-and-jekyll-seo/)做的，包括：

1. jekyll博客头的**title、description以及tags**设置
2. 避免jekyll的**permalink**造成死链接
3. 添加**robots.txt**文件对爬虫表示友好
4. 添加**favicon.ico**令博客看起来像个样子（使用[Favicon.ico在线制作](http://www.favicon-icon-generator.com/)生成的）

### 4、添加多说分享系统
在添加了多说评论系统后，在瞎逛多说平台时发现还有多说分享系统：
![ds_share](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144914778761.png)

然后就顺便把多说分享系统也添加上去了，但需要注意的是，由于jekyll 3.0.1版本的问题，导致分享代码里面的中文会报错：
![jekyll_gbk](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144914859479.png)

然后上网查找了下[解决方案](http://www.cnblogs.com/aleda/articles/Jekyll-in-Windows-following-Chinese-encoding-problem-solutions.html)，发现都是jekyll几年前版本的，现在都已经不适应了。而且不单单在本地jekyll报错，尝试上传到GitHub后虽然博客可以照常访问，但是更新都没有生效。。。最后不得已只能把代码中的中文字符全部使用空格& nbsp;（话说这个空格写法怎么在md里面转义？）代替了，最后博客文章底部的分享和评论显示如下：
![share_discuss](http://cejdh.img46.wal8.com/img46/533449_20151202165458/14491482669.png)

**（2015.12.4 更新）**最后发现可以将其与之前的comments.ext结合：
![comments_ext](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144920456945.png)

**因为直接在html里面写中文是不行的，但是在模板/脚本文件里面却是可以的。**如此一来的话，把之前多余的判断评论系统的if else语句也整合进来，如此一来就只需要在一个地方include comments.ext即可达到复用的目的：
![include_comments](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144920457058.png)

然后删除多余的判断评论系统的if else语句：
![delete_if](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144920457002.png)

### 5、添加博客版权信息
最后类似其他博客一样在文章最后添加下版权信息，包括BY-NC-SA许可协议以及作者和日期：
![blog_copyright1](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144920457124.png)

对应的CSS文件也修改：
![blog_copyright2](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144920457193.png)

最终的显示如下图所示：
![blog_copyright3](http://cejdh.img46.wal8.com/img46/533449_20151202165458/144920457259.png)

**（2015.12.4 更新——完）**

****

**（2016.3.15 更新）**更换电脑后需要重装jekyll

### 1、如果使用ruby dk.rb install安装DevKit失败，且报错
>Invalid configuration or no Rubies listed. Please fix 'config.yml' and rerun 'ruby dk.rb

一般是在解压DevKit出来文件夹下的config.yml文件（该文件是ruby dk.rb init初始化生成的）没有配置好，配置指定一下系统的Ruby路径即可，具体可参考[这里](http://blog.csdn.net/promaster/article/details/47260399)，我的配置如下
![img20160323_0](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145867384822.png)

### 2、如果发现淘宝镜像也可能不稳定不好使了
![img2016_0](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145802095355.png)

发现淘宝镜像不想的话，可以更换成[山东理工大学的Rubygems 镜像](http://ruby.sdutlinux.org/)
![img2016_1](http://cejdh.img47.wal8.com/img47/533449_20151202165458/14580209544.png)

然后也同样使用
>gem sources -l

查看确保源地址已经更换，最后继续安装jekyll即可，如下图所示：
![img2016_2](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145802095435.png)

但最后发现默认给安装成了jekyll 2.5.3版的了。。。但应该关系不大，还是可以正常使用的，当然我们希望jekyll 3.x版本能正常兼容jekyll 2.x版本，但貌似可能会[有点问题](https://rebornix.com/engineering/2015/11/16/Jekyll3Breaks/)，但至少对我的博客没影响

![img20160323_1](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145867385021.png)

### 3、如果使用gjekyll serve --trace启动发现报错
>in `require': cannot load such file -- wdm (LoadError)

则表示没有安装wdm依赖，使用
>gem install wdm

安装一下即可

**（2016.3.15 更新——完）**

****

**（2016.3.16 更新）**为博客主页的文章显示评论数和喜欢数

### 1、多说API——获取文章评论、转发数
具体的[多说API地址请点击这里](http://dev.duoshuo.com/docs/50615732a834c63c56004257)

然后里面的描述算清楚了，假如要查看本篇博客的评论数，可在浏览器内直接请求
>http://api.duoshuo.com/threads/counts.json?short_name=**iclojure**&threads=**/articles/2015/12/02/make-blog-by-jekyll**

发现浏览器内请求成功返回
![duoshuo_2](http://cejdh.img47.wal8.com/img47/533449_20151202165458/14580744199.png)

其中粗体的是多说用户名，以及本篇博客发送给多说的page.id，即绑定到多说系统的标识id——data-thread-key，其实文章标识可以在多说后台编辑的
![duoshuo_0](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145807441968.png)

但是需要注意可能误删某篇文章或者发送给多说的位移标识id有变化，则可能会丢失掉之前的评论。。。
![duoshuo_1](http://cejdh.img47.wal8.com/img47/533449_20151202165458/14580744198.png)

### 2、跨域调用多说API
既然已经知道了多说API的地址，且在浏览器中直接访问也成功了，那咱们是不是就可以直接使用Ajax请求该json数据了呢？

其实并不是，因为有Ajax跨域访问的问题，如果直接Ajax请求上面浏览器访问的地址，会直接报跨域不被允许的错误
>**No 'Access-Control-Allow-Origin' header is present on the requested resource**

解决办法是把Ajax请求的dataType设置为jsonp，但最为关键的是其Ajax请求url不是
>http://api.duoshuo.com/threads/counts.json

而应该改为
>http://api.duoshuo.com/threads/counts.jsonp

在此感谢[多说API——获取文章评论、转发数底部被置顶的这篇例子文章](http://www.smzdwan.com/news/797.html)才知道的，否则一直使用.json后缀请求，即使设置了dataType为jsonp，却还是一直报解析错误**Uncaught SyntaxError: Unexpected token :**

### 3、在博客主页添加comments.js处理多个文章的评论数和喜欢数
思路：

一、博客主页遍历分页post时为每篇博客设置id，该id与发给多说的唯一标识id一致

二、在comments.js收集本页所有博客id，并作为参数传递给Ajax跨域请求

三、把Ajax跨域请求的响应结果再根据博客id设置评论数和喜欢数的值

具体代码如下
![duoshuo_3](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145807441993.png)

这里因为发送给多说的id是形如**/articles/2015/12/02/make-blog-by-jekyll**的格式，JQuery里面使用**$("#/articles/2015/12/02/make-blog-by-jekyll")**获取该元素是会报错的，所有不单单代码中，页面上也使用了replace去把/替换成\_，然后最后再从/转回\_来
![duoshuo_5](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145807502648.png)

最终的博客主页效果如下
![duoshuo_4](http://cejdh.img47.wal8.com/img47/533449_20151202165458/145807442002.png)

然后评论数和喜欢数都添加了**a锚点加上#号的跳转链接方式**，即点击评论数或者喜欢数，会直接跳转到该博客文章的评论位置

**（2016.3.16 更新——完）**

（完）