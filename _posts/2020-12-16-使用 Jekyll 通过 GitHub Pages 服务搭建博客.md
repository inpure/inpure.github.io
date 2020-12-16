---
title: 使用 Jekyll 通过 GitHub Pages 服务快速搭建博客
tags: Jekyll - GitHub Pages - Markdown
---
&emsp;&emsp;这篇文章教你搭建一个免费的、便利的、拥有绝对管理权的 Blog -- Jekyll 和 GitHub Pages 使用入门。
## Jekyll 是什么？
&emsp;&emsp; [Jekyll](http://jekyllcn.com/) 是一个简单的博客形态的静态网站生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和我们的 Liquid 渲染器转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。  
- **简单**  
  不再需要数据库，不需要开发评论功能，不需要不断的更新版本——只用关心你的博客内容。
- **静态**  
  Markdown（或 Textile）、Liquid 和 HTML & CSS 构建可发布的静态网站。
- **博客支持**  
  支持自定义地址、博客分类、页面、文章以及自定义的布局设计。  

&emsp;&emsp;简单地说，Jekyll 就是一个工具，它可以把内容转换成 HTML 网页页面，有了它博主只需要专注于写文章即可，不用考虑如何排版的事情。并且 Jekyll 是GitHub Pages 默认使用的工具。

## GitHub Pages 是什么？
&emsp;&emsp;[GitHub Pages](https://docs.github.com/cn/free-pro-team@latest/github/working-with-github-pages/about-github-pages) 是面向用户、组织和项目开放的公共静态页面搭建托管服务，站点可以被免费托管在 Github 上，你可以选择使用 Github Pages 默认提供的域名 github.io 或者自定义域名来发布站点。Github Pages 支持自动利用 Jekyll 生成站点，也同样支持纯 HTML 文档，可将Jekyll 站点托管在 Github Pages 上。  
&emsp;&emsp;简单地说，可以把 Github Pages 当成一个免费的站点服务器，只需要把本地编写符合 Jekyll 规范的网站源码上传到 GitHub，它就会自动帮你生成并托管整个网站，你只要用自己喜欢的编辑器写文章就可以了，其他事情一概不用操心，都由github处理。  

&emsp;&emsp;**有了上面的介绍，博客的搭建思路就很清晰了。先在本地编写符合 Jekyll 规范的网站源码，然后上传到 Github，由 Github 生成并托管整个网站。**

## 搭建过程
&emsp;&emsp;在搭建之前，
- **1. 安装 Jekyll**  
  &emsp;&emsp;因为 Jekyll 是基于 Ruby 的静态网页生成系统，因此我们首先得安装 Ruby 环境，不同的操作系统可以去参考 [Ruby 官方安装文档](https://www.ruby-lang.org/en/documentation/installation/) 进行安装。等 Ruby 安装完毕后，打开命令提示符执行以下命令完成 Jekyll 的安装。
  >`gem install jekyll bundler`  
  
  安装过程需要几分钟时间
- **2. 使用 Jekyll 在本地创建博客站点**  
    在命令提示符中执行以下命令：
  >`jekyll new blogname`  # *“blogname” 可自定义*  
  
  &emsp;&emsp;因为 `jekyll new` 命令创建的项目默认已经使用了主题，所以这个过程会根据网络情况需要一定的时间安装相关的依赖，我们不用管。执行完以上命令后会生成一个名为 blogname 的文件夹，文件夹的目录如下：

  ![](/screenshots/blogname.png)   
  &emsp;&emsp;这就是 Jekyll 为我们初始化好的博客站点的所有文件，我们主要关注 `_posts` 文件夹和 `_config.yml` 两个地方：
  - **`_posts`文件夹**  
   &emsp;&emsp;这里就是我们编辑博客文章的地方，我们只需要找一个趁手的 Markdown 编辑器把写好的文章存放在这里即可。需要注意的是，文章的内容和标题需要按照 Jekyll 的格式进行写作。  
  文章的文件名遵循下面的格式：    

    `年-月-日-标题.markdown`  
    
    文章内容顶部必须有类似下面的 YAML 头信息(YAML front-matter)：  
    
    ```
    ---
    layout: post
    title: Document - Writing Posts
    ---
    ```
    在 `---` 之间你可以设置属性的值，可以把它们看作页面的配置，这些配置会覆盖在 _config.yml 文件中设置的全局配置。
  - **`_config.yml` 文件**  
  
    ![](/screenshots/config_yml.png)  

    &emsp;&emsp;以上就是文件里的内容，主题里的所有关键性配置都在 _config.yml 文件中，你可以根据个人的喜好和不同主题支持的功能来修改具体的内容。
- **3. 本地预览站点**  
  &emsp;&emsp;进入 blogname 目录，打开命令提示符，输入以下命令：
  >`bundle exec jekyll serve`  
  
  &emsp;&emsp;这个命令实际启动了 Jekyll 集成的开发用服务器，可以让我们使用浏览器在本地进行预览，然后就可以通过在浏览器访问 127.0.0.1:4000 查看初始界面的样子了:  

  ![](/screenshots/blogdemo.png)  
  
  &emsp;&emsp;默认的界面看起来非常的简陋也很丑，但是没关系，你可以在这些网站里根据自己的喜好找到一些美观的主题 [jekyllthemes.org](http://jekyllthemes.org/)、[jekyllthemes.io](https://jekyllthemes.io/)、[themes.jekyllrc.org](http://themes.jekyllrc.org/)  
  &emsp;&emsp;安装方法很简单，一般情况下只需要下载主题包解压后完整的，复制到你的 blogname 的目录里，并覆盖你之前的文件即可，这里不作展开。
- **4. 使用 GitHub Pages 服务，将本地博客站点部署到 Github**  
  &emsp;&emsp;默认你已经安装了Git，并且有 Github账户。首先到你的 GitHub 上创建一个新的仓库（Repository），仓库名的格式必须是 `username.GitHub.io` 把这里的`username`替换成你的 Github用户名。  

  ![](/screenshots/newrepo_1.png)  
  &emsp;&emsp;然后在本地命令行中切换到你的自定义路径下，Clone 下来你的项目（操作需要在 Mac 的 Terminal 中完成，Windows 系统可以使用 Git-bash。）这里注意这里的 path 和 username 需要根据你个人情况进行替换。  

  `cd ~/Path git clone https://GitHub.com/username/username.GitHub.io`  
  
  &emsp;&emsp;现在进入克隆下来的仓库文件夹 `username.github.io`，里面只有一个隐藏的 `.git` 文件夹，这是因为我们之前创建的是一个空的仓库。如果成有别的文件，请手动删除。然后再把我们前面已经创建好的博客站点里的内容全部剪切粘贴到仓库里来（即将 `blogname` 里的文件全部剪切粘贴到 `username.GitHub.io`）。  
  &emsp;&emsp;此时我们就可以按照 Git 提交内容的流程，将我们的创建的博客站点上传到 Github:  
  ```
  git add --all
  git commit -m "Firs Push"
  git push -u origin master
  ```
  &emsp;&emsp;OK，到这一步我们就已经大功告成了，Github 会自动帮我们生成博客网站。现在在浏览器里输入网址 username.github.io（没错，就是前面你新建的仓库名） 就可以看到完全由你掌控的博客网站了。