# Hugo搭建个人博客


想在网上写点东西。相关的平台里微信公众号和知乎不支持markdown，写作体验一般；csdn商业化太严重；之前在cnblogs上发，但今年3月份cnblogs被要求全面整改，据说是因为被恶意发布违规信息和恶意投诉。加上这些平台都需要审核，没有自建博客自由，就考虑自己搭个博客。

##### 博客框架

市面上的开源博客框架大致分为两类。一类是动态博客，如wordpress、typecho、halo等；一类是静态博客，如hexo、Jekyll、hugo等。

###### 动态博客

wordpress和typecho基于php，halo基于Java（springboot+freemarker）。构建这类博客需要买一个服务器和一个域名，在服务器上安装相关语言环境、web server（apache、nginx）和database，安装博客框架并初始化网站配置，最后将域名解析到服务器ip地址上就算完成了。

这类博客一般都有网站后台，可以发布文章和管理站点，功能比较齐全。

###### 静态博客

hexo基于node.js,Jekyll基于ruby,hugo基于go。相比动态博客有以下优点：

* 免费。直接部署到github page上。理论上也可以部署到服务器上，只需要一个web server。
* 速度快。不经过php和Java等后台处理，不用数据库。
* 安全。由于静态网站的简洁，免疫很多web攻击方式。

静态网站的缺点是功能弱，和用户的交互能力不强。适合专注于内容的网站，例如博客。

##### Hugo

hexo和hugo我都试了一下，感觉hugo要更简单，速度更快一些。最后选定了Hugo进行搭建。

首先要安装git和go语言环境，配置环境变量；下载hugo并解压，将可执行文件放在 'go安装目录/bin' 文件夹下,因为已经配了go的环境变量，这样就不用再配置hugo的环境变量。

###### 创建站点

~~~go
hugo new site 站点名称
如 hugo new site spacelion
~~~

创建完会后，在spacelion文件夹会生成以下文件结构：

~~~go
.
├── archetypes # 存放生成博客的模版
├── content # 存放 markdown 文件
├── data # 存放 Hugo 处理的数据
├── layouts # 存放布局文件
├── resources # 存放一些资源
├── static # 存放静态文件 图片 CSS JS文件
├── themes # 存放主题
└── config.toml # 存放 hugo 配置文件 支持 JSON YAML TOML 三种格式配置文件
~~~

###### 下载主题

选择一款喜欢的主题下载到themes文件夹下

~~~go
git clone 主题git地址 目录
如 git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
~~~

###### 站点配置

配置config.toml，供参考：

~~~toml
baseURL = "https://absolutevalentine.github.io/"
# [en, zh-cn, fr, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "SpaceLion"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3
  [[menu.main]]
    identifier = "about"
    pre = ""
    post = ""
    name = "关于"
    url = "/about/"
    title = ""
    weight = 4

[params]
  #  LoveIt 主题版本
  version = "0.2.X"
  # 网站描述
  description = "软件,区块链,机器学习,金融,股票,基金,数字货币,量化投资"
  # 网站关键词
  keywords = ["软件","区块链","机器学习","金融","股票","基金","数字货币","量化投资"]
  # 网站默认主题样式 ("light", "dark", "auto")
  defaultTheme = "dark"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""
  #  哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ("sha256", "sha384", "sha512", "md5")
  fingerprint = ""
  #  日期格式
  dateFormat = "2006-01-02"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["/logo.png"]

  #  应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "LoveIt"
    # 是否隐藏网站图标资源链接
    noFavicon = false
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = "/images/app.svg"
    # Android 浏览器主题色
    themeColor = "#ffffff"
    # Safari 图标颜色
    iconColor = "#5bbad5"
    # Windows v8-10磁贴颜色
    tileColor = "#da532c"

  #  搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ("lunr", "algolia")
    type = "lunr"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = ""
    #  最大结果数目
    maxResultLength = 10
    #  结果内容片段长度
    snippetLength = 50
    #  搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    #  是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""

  # 页面头部导航栏配置
  [params.header]
    # 桌面端导航栏模式 ("fixed", "normal", "auto")
    desktopMode = "fixed"
    # 移动端导航栏模式 ("fixed", "normal", "auto")
    mobileMode = "auto"
    #  页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = ""
      # 标题名称
      name = "SpaceLion"
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = "<i class='far fa-kiss-wink-heart fa-fw'></i>"
      # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
      post = ""
      #  是否为标题显示打字机动画
      typeit = false

  # 页面底部信息配置
  [params.footer]
    enable = true
    #  自定义内容 (支持 HTML 格式)
    custom = ''
    #  是否显示 Hugo 和主题信息
    hugo = false
    #  是否显示版权信息
    copyright = true
    #  是否显示作者
    author = true
    # 网站创立年份
    since = 2021
    # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
    icp = ""
    # 许可协议信息 (支持 HTML 格式)
    license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  #  Section (所有文章) 页面配置
  [params.section]
    # section 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  #  List (目录或标签) 页面配置
  [params.list]
    # list 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  # 主页配置
  [params.home]
    #  RSS 文章数目
    rss = 10
    # 主页个人信息
    [params.home.profile]
      enable = true
      # Gravatar 邮箱，用于优先在主页显示的头像
      gravatarEmail = ""
      # 主页显示头像的 URL
      avatarURL = "/images/avatar.png"
      #  主页显示的网站标题 (支持 HTML 格式)
      title = "即非 是名"
      # 主页显示的网站副标题
      subtitle = "“那种里面没放肉的青椒肉丝，应该不可以叫做青椒肉丝吧。”<br>“不！可以叫的，没钱的时候就可以这样叫！”"
      # 是否为副标题显示打字机动画
      typeit = true
      # 是否显示社交账号
      social = true
      #  免责声明 (支持 HTML 格式)
      disclaimer = ""
    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 6
      #  被 params.page 中的 hiddenFromHomePage 替代
      # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
      defaultHiddenFromHomePage = false

  # 作者的社交信息设置
  [params.social]
    GitHub = "AbsoluteValentine"
    Linkedin = ""
    Twitter = ""
    Instagram = ""
    Facebook = ""
    Telegram = ""
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = "spikeqiang"
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = "SpikeQiang"
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    Email = "spacelion@foxmail.com"
    RSS = true # 

  #  文章页面配置
  [params.page]
    #  是否在主页隐藏一篇文章
    hiddenFromHomePage = false
    #  是否在搜索结果中隐藏一篇文章
    hiddenFromSearch = false
    #  是否使用 twemoji
    twemoji = false
    # 是否使用 lightgallery
    lightgallery = false
    #  是否使用 ruby 扩展语法
    ruby = true
    #  是否使用 fraction 扩展语法
    fraction = true
    #  是否使用 fontawesome 扩展语法
    fontawesome = true
    # 是否在文章页面显示原始 Markdown 文档链接
    linkToMarkdown = true
    #  是否在 RSS 中显示全文内容
    rssFullText = false
    #  目录配置
    [params.page.toc]
      # 是否使用目录
      enable = true
      #  是否保持使用文章前面的静态目录
      keepStatic = false
      # 是否使侧边目录自动折叠展开
      auto = false
    #  代码配置
    [params.page.code]
      # 是否显示代码块的复制按钮
      copy = true
      # 默认展开显示的代码行数
      maxShownLines = 30
    #  KaTeX 数学公式
    [params.page.math]
      enable = true
      # 默认块定界符是 $$ ... $$ 和 \\[ ... \\]
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # 默认行内定界符是 $ ... $ 和 \\( ... \\)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      # KaTeX 插件 copy_tex
      copyTex = true
      # KaTeX 插件 mhchem
      mhchem = true
    #  Mapbox GL JS 配置
    [params.page.mapbox]
      # Mapbox GL JS 的 access token
      accessToken = ""
      # 浅色主题的地图样式
      lightStyle = "mapbox://styles/mapbox/light-v9"
      # 深色主题的地图样式
      darkStyle = "mapbox://styles/mapbox/dark-v9"
      # 是否添加 NavigationControl
      navigation = true
      # 是否添加 GeolocateControl
      geolocate = true
      # 是否添加 ScaleControl
      scale = true
      # 是否添加 FullscreenControl
      fullscreen = true
    #  文章页面的分享信息设置
    [params.page.share]
      enable = true
      Twitter = true
      Facebook = true
      Linkedin = true
      Whatsapp = false
      Pinterest = false
      Tumblr = false
      HackerNews = false
      Reddit = true
      VK = false
      Buffer = false
      Xing = false
      Line = false
      Instapaper = false
      Pocket = false
      Digg = false
      Stumbleupon = false
      Flipboard = false
      Weibo = true
      Renren = false
      Myspace = false
      Blogger = false
      Baidu = false
      Odnoklassniki = false
      Evernote = false
      Skype = false
      Trello = false
      Mix = false
    #  评论系统设置
    [params.page.comment]
      enable = true
      # Disqus 评论系统设置
      [params.page.comment.disqus]
        # 
        enable = false
        # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
        shortname = ""
      # Gitalk 评论系统设置
      [params.page.comment.gitalk]
        # 
        enable = false
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      # Valine 评论系统设置
      [params.page.comment.valine]
        enable = true
        appId = "xxxxxxxxxxxxxxxxx"
        appKey = "xxxxxxxxxxxxx"
        placeholder = ""
        avatar = "ss"
        meta= ""
        pageSize = 10
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        #  emoji 数据文件名称, 默认是 "google.yml"
        # ("apple.yml", "google.yml", "facebook.yml", "twitter.yml")
        # 位于 "themes/LoveIt/assets/data/emoji/" 目录
        # 可以在你的项目下相同路径存放你自己的数据文件:
        # "assets/data/emoji/"
        emoji = ""
      # Facebook 评论系统设置
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        languageCode = "zh_CN"
      #  Telegram Comments 评论系统设置
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
      #  Commento 评论系统设置
      [params.page.comment.commento]
        enable = false
      #  Utterances 评论系统设置
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
    #  第三方库配置
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # 位于 "assets/"
        # 或者
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # 位于 "assets/"
        # 或者
        # someJavascript = "https://cdn.example.com/some.js"
    #  页面 SEO 配置
    [params.page.seo]
      # 图片 URL
      images = []
      # 出版者信息
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  #  TypeIt 配置
  [params.typeit]
    # 每一步的打字速度 (单位是毫秒)
    speed = 100
    # 光标的闪烁速度 (单位是毫秒)
    cursorSpeed = 1000
    # 光标的字符 (支持 HTML 格式)
    cursorChar = "|"
    # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
    duration = -1

  # 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""

  #  网站 SEO 配置
  [params.seo]
    # 图片 URL
    image = ""
    # 缩略图 URL
    thumbnailUrl = ""

  #  网站分析配置
  [params.analytics]
    enable = false
    # Google Analytics
    [params.analytics.google]
      id = ""
      # 是否匿名化用户 IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # 自行托管追踪器时的主机路径
      server = ""

  #  Cookie 许可配置
  [params.cookieconsent]
    enable = true
    # 用于 Cookie 许可横幅的文本字符串
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  #  第三方库文件的 CDN 设置
  [params.cdn]
    # CDN 数据文件名称, 默认不启用
    # ("jsdelivr.yml")
    # 位于 "themes/LoveIt/assets/data/cdn/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/cdn/"
    data = ""

  #  兼容性设置
  [params.compatibility]
    # 是否使用 Polyfill.io 来兼容旧式浏览器
    polyfill = false
    # 是否使用 object-fit-images 来兼容旧式浏览器
    objectFit = false

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 5
    endLevel = 6

# 作者配置
[author]
  name = "SpikeQiang"
  email = "spacelion@foxmail.com"
  link = ""

# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# 隐私信息配置
[privacy]
  #  Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  # 
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
~~~

###### 创建文章

~~~go
hugo new 文章名.md
推荐放到一个目录下便于管理
如 hugo new posts/firstblog.md
默认情况下，所有帖子和页面都创建为草稿。如果要呈现这些页面，请设置该属性draft: false，可以更改archetypes目录下的模板文件，这样就不用每次更改了。或将-D/--buildDrafts参数添加到hugocommand。
~~~

文章参数：

~~~toml
title: "{{ replace .Name "-" " " | title }}"  #标题
date: {{ .Date }}                             #生成日期
lastmod: {{ .Date }}                          #最后更新日期
draft: false                                  #非草稿
tags:                                         #标签
- 
- 
categories:                                   #分类
- 
author: "SpikeQiang"                          #作者
authorLink: ""                                #作者url
description: ""                               #描述
hiddenFromHomePage: false                     #首页不隐藏
hiddenFromSearch: false                       #搜索不隐藏
toc: true                                     #目录开启
autoCollapseToc: false                        #目录不自动折叠
~~~



###### 本地启动网站

~~~go
hugo serve -D -e production
-D是显示草稿，-e production是模拟生产环境，这样评论和cdn等功能可以起作用。
~~~

然后访问 http://localhost:1313，就可以访问本地博客了。hugo一个优秀的点是文件内容改变时，页面也会自动更新，方便在本地调试。

###### 部署到github page

1、需要一个github账号，然后创建一个仓库。注意：创建仓库的命名必须是你github的昵称且必须小写再加上.github.io，例如 absolutevalentine.github.io。

2、切换到博客的根目录下，cmd中输入命令 hugo。

3、然后会在根目录生成一个public文件夹

~~~go
cd public //切换到public目录
git init //将此文件夹变成git本地仓库
git remote add origin 远程git仓库地址 //关联远程仓库
git add . //将改动的文件放入暂存区中等待提交
git commit -m "注释"  //提交到本地仓库
git pull origin master //推送之前先拉代码，养成好习惯
git push origin master //推送到远程仓库
~~~

4、最后访问 absolutevalentine.github.io 即可。

##### 站点优化

###### 配置域名

由于我想换个域名，于是我去namesilo上买了个域名，其他如godaddy，国内的各大云厂商都可以购买域名，相比较namesilo上价格更便宜一些，且不用实名认证，提供免费隐私保护，反whois查询。

购买后在namesilo上将域名解析到原ip或原域名,原ip可以ping原域名得到，并在github远程仓库中配置CNAME为你购买的域名,这样就可以由你购买的域名访问网站了。

[namesilo购买和使用教程](https://zhuanlan.zhihu.com/p/33921436)

###### CDN加速

想提高网站的访问速度，利用CDN多节点缓存加速，国内很多云厂商提供CDN服务，我用的国外的CloudFlare，有免费版，外加提供免费的ssl证书，不用实名认证。

[CloudFlare免费CDN加速使用方法](https://zhuanlan.zhihu.com/p/29891330)

由于CloudFlare的CDN 节点大多在国外（免费版），国内用户访问速度不稳定，国外访问速度很快。实测国内访问速度使用CloudFlare前后差不多。比较适合主要面向国外访客的网站（如外贸站点等）；或者不在意速度，想节省源站资源；或者主要想使用它的保护功能的用户。如果对国内速度有较高要求，可以尝试国内厂商的CDN服务。

###### 其他

其他诸如SEO优化等还是萌新，下次再聊。
