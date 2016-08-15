---
layout: post
title:  Publish GitHub Pages With Jekyll on Windows
date:   2016-08-9 
categories: github page jekyll windows
author:huaiyang
---

**此文描述的是：windows环境下使用Jekyll创建和发布GitHub Pages的建议流程。**

**我们分成两个大的步骤**

（笔者使用的Windows 10操作系统，Jekyll 的版本是3.2.1，ruby的版本是2.2.4）

## 第一步 : 在Windows下安装运行Jekyll的必要依赖

由于windows平台并非jekyll的官方支持平台，所以研发者需要另辟蹊径。好在jeklly提供了在windows平台下运行的解决方案。详细的官方文档请参见 [windows-specific docs page](https://jekyllrb.com/docs/windows/#installation)。（PS:在我写完前，请先参见此链接的教程）

### Stpe1：安装一个名为“ Chocolatey”的windows包管理工具
请参照 [Installing Chocolatey](https://chocolatey.org/install) 官网所给出的安装引导。无需多虑是否将其安装在自己喜欢的目录下，请按照默认推荐的方式安装即可。简单说就是以“**管理员**”模式运行windows的命令行——cmd.exe。然后复制下面的命令直接执行即可

{% highlight ruby %}
@powershell -NoProfile -ExecutionPolicy Bypass -Command "[System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
{% endhighlight %}

### Step2：安装Ruby
官方给出的安装方式是打开命令行（cmd.exe）直接执行
{% highlight ruby %}
choco install ruby -y
{% endhighlight %}
不过笔者执行的是，github官方给出的指示是2.x.x以上均可。
{% highlight ruby %}
choco install ruby -version 2.2.4
{% endhighlight %}

### Step3：安装Jekyll(先别动，往下看)
如果你看Jekyll的官方文档，会告诉你直接打开命令行（cmd.exe）执行下面的命令：
{% highlight ruby %}
gem install jekyll
{% endhighlight %}
然而，实际上第三步是非必要的，笔者建议使用 [Bundler](http://bundler.io/)安装并运行Jekyll！
如果你安装第三步安装了Jekyll,然后安装官方文档的命令执行:
{% highlight ruby linenos %}
jekyll build
jekyll serve
{% endhighlight %}
你会发现许多编译错误，要一个个解决并不容易。当然作为兴趣，也鼓励大家去尝试碰碰Ruby的坑。这里不再占用篇幅。

**_Bing Go!_**
<br/>
![versions of tools](/hy.blog/assets/hy/20160812001.png)

## 第二步 : 利用Bundle安装Jekyll

（以下步骤详情请参见 [Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/)）

### Step1：Clone或者Create一个代码仓库，然后创建或者检出到gh-pages分支
<span style="color:red;">**_Important!_**</span>   确保代码库根目录下有一个名为GemFile不带后缀的文件，如果没有，请手动创建一个，这里就不在详述。
<br/>
为什么要切换到gh-pages分支？这取决于你希望将此Jekyll创建的静态资源页发布为**个人/组织主页**或者是**项目主页**。笔者这里测试的是项目主页，所以发布到gh-pages分支。
为节省篇幅不再详述有关 [User, Organization, and Project Pages](https://help.github.com/articles/user-organization-and-project-pages/)的信息，请自行学习和参考。

### Step2：打开根目录下的GemFile，手动粘贴下面两行信息：
{% highlight ruby %}
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
{% endhighlight %}
当然，如果GemFile中已经有如上信息，则忽略此步骤。

### Step3：安装Bundle
So easy!在GemFile同级文件目录下执行
{% highlight ruby %}
 $ bundle install
Fetching gem metadata from https://rubygems.org/............
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Resolving dependencies...
{% endhighlight %}

### step4：生成Jekyll页面
如果你的代码库已经构建和使用过Jekyll，那么你只需运行
{% highlight ruby %}
$ bundle exec jekyll serve
or
$ bundle exec jekyll serve --draft
{% endhighlight %}

如果你的代码库尚未使用和创建过Jekyll静态资源页，那么请在代码仓库的GemFile同级目录下执行
{% highlight ruby linenos %}
$ bundle exec jekyll new . --force
$ bundle exec jekyll serve
{% endhighlight %}


