---
title: Hugo配置记录
date: 2025-01-22T19:36:31+08:00
categories:
  - blog
tags:
  - blog
author: Delusion
description: hugo建站,配置,papermod,hugo,blog
slug: hugo-configuration
dir: misc
share: true
---
## 环境

MacBook M1Pro，macOS 15.1.1

## hugo初始化配置
### hugo安装
按照[hugo官网](https://gohugo.io/)给出的quick start进行安装。按照官网所说，我们需要安装`git`,`go`和`Dart Sass`。其中`git`是必备的，另外两个可以按需选择。我所选择的主题[PaperMod主题](https://github.com/adityatelange/hugo-PaperMod)似乎并没有使用`Sass`，所以我并没有安装，另外go我也没有安装。

我选择使用`Homebrew`来安装`hugo`(~~什么，作为一个mac用户你还不知道homebrew！？现在马上关闭本文去了解homebrew！~~)

```shell
brew install hugo
```

输入以下命令来验证`hugo`是否被安装成功。

```shell
hugo version
```

如果有版本提示，那么恭喜，你已经成功的安装上了`hugo`

### 创建网站目录

寻找一个合适的文件夹，在文件夹的目录下打开`shell`输入以下命令，创建一个站点文件夹。

```shell
hugo new site <blog-name> # 最后一个参数替换成你想要的站点文件夹名
```

接下来进入到你刚刚创建的文件夹内，后面我们将会在这个文件里面做大量的配置工作，我这里使用vscode进行编辑，你可以使用你喜爱或熟悉的文本编辑工具进行配置。

首先我们先进行`git`仓库的配置。

```shell
git init
git add .
git commit -m "first commit"
```

接着再添加一下`.gitignore`

```shell
touch .gitignore
```

将以下文件复制进去，这里是直接复制`PaperMod`作者的配置。

```
# Compiled Object files, Static and Dynamic libs (Shared Objects)
*.o
*.a
*.so

# Folders
_obj
_test

# Architecture specific extensions/prefixes
*.[568vq]
[568vq].out

*.cgo1.go
*.cgo2.c
_cgo_defun.c
_cgo_gotypes.go
_cgo_export.*

_testmain.go

*.exe
*.test

/public
.DS_Store
.hugo_build.lock
resources/_gen/


```

接下来，我们安装`PaperMod`主题，这里是官网的[安装说明](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/)，我们使用推荐的`git submodule`

```shell
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

这时候我们可以发现目录下的`themes/PaperMod`多了主题文件。如果你不熟悉`git`的子模块，这里还需有一些别的需要注意的地方，后面我们再提。

### 增加必要页面

在`content`下创建三个文件，分别是`about.md`，`archives.md`，`search.md`。

依次往下面三个文件中写入以下内容

```md
---
title: "关于"
layout: "about"
url: "/about/"
summary: about
---
这里就可以贴个人简介了
```

```md
---
title: "Archive"
layout: "archives"
url: "/archives/"
summary: archives
---

```

```md
---
title: "Search" # in any language you want
layout: "search" # is necessary
# url: "/archive"
# description: "Description for Search"
summary: "search"
placeholder: "请输入一些东西"
---

```

## PaperMod主题配置

如果你想要一个最基本的配置，依然可以去阅读[官网](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation/)给出的案例。

我的配置就不一一详解了，大部分配置项我都写了注释，不懂的可以直接丢给GPT询问。

### 个人配置

相比于原主题，我加入了顶部栏常驻，数学公式渲染，代码块日夜主题适配，hover效果，盘古之白，图片放大支持，首页显示tag，最后编辑时间支持，侧边目录。

首先我们仍需要按照官网所说，将博客根目录下的`hugo.toml`改成`hugo.yaml`，这里直接使用下面的命令，因为之后我们的配置文件会覆盖的复制进去。

```shell
mv hugo.toml hugo.yaml
```

我的配置文件。

```yaml
// hugosite/hugo.yaml
# ~~~~~~~~~
# hugo 本身的配置
# ~~~~~~~~~
baseURL: "https://hugo.forsakendelusion.online/" # 主站的 URL
title: Delusion's Blog # 站点标题
# copyright: "[©2024-2025 Delusion's Blog](https://hugo.forsakendelusion.online/)" # 网站的版权声明，通常显示在页脚，但这里我使用更改footer模板的方式实现
theme: PaperMod # 主题
languageCode: zh-cn # 语言

enableInlineShortcodes: true # shortcode，类似于模板变量，可以在写 markdown 的时候便捷地插入，官方文档中有一个视频讲的很通俗
hasCJKLanguage: true # 是否有 CJK 的字符
enableRobotsTXT: true # 允许生成 robots.txt
buildDrafts: false # 构建时是否包括草稿
buildFuture: false # 构建未来发布的内容
buildExpired: false # 构建过期的内容
enableEmoji: true # 允许 emoji
pygmentsUseClasses: true
defaultContentLanguage: zh # 顶部首先展示的语言界面
defaultContentLanguageInSubdir: false # 是否要在地址栏加上默认的语言代码
enableGitInfo: true # 启用git信息，用于更新“最后编辑于”的信息，lastmod参数


languages:
  zh:
    languageName: "中文" # 展示的语言名
    weight: 1 # 权重
    taxonomies: # 分类系统
      category: categories
      tag: tags
    # https://gohugo.io/content-management/menus/#define-in-site-configuration
    menus:
      main:
        - name: 首页
          pageRef: /
          weight: 2 # 控制在页面上展示的前后顺序
        - name: 归档
          pageRef: archives/
          weight: 3
        - name: 分类
          pageRef: categories/
          weight: 4
        - name: 标签
          pageRef: tags/
          weight: 5
        - name: 搜索
          pageRef: search/
          weight: 6
        - name: 关于
          pageRef: about/
          weight: 7

# ~~~~~~~~~
# 主题的配置(基本上是)
# ~~~~~~~~~
pagination.pagerSize: 8 # 每页展示的文章数量，属于hugo设置

params:
  env: production # to enable google analytics, opengraph, twitter-cards and schema.
  description: "Theme PaperMod - https://github.com/adityatelange/hugo-PaperMod"
  author: Delusion
  defaultTheme: light # 默认是亮色背景
  ShowShareButtons: false # 关闭分享的按钮
  ShowReadingTime: true # 展示预估的阅读时长
  displayFullLangName: true # 展示全名
  ShowPostNavLinks: true # 展示文章导航链接，就是下一页上一页的那个
  ShowBreadCrumbs: true # 是否展示标题上方的面包屑
  ShowCodeCopyButtons: true # 是否展示复制代码的按钮
  ShowRssButtonInSectionTermList: true # RSS 相关
  ShowAllPagesInArchive: true # 在归档页面展示所有的页面
  ShowPageNums: true # 展示页面的页数
  ShowToc: true # 展示文章详情页的目录
  TocOpen: true
  comments: true # 评论
  # images: ["https://forsakendelusion.online/wp-content/uploads/2024/07/715156.png"] # 缺省的图片，比如，博客的封面
  DateFormat: "2006-01-02" # 这个时间是作者自己写的，只能这样写
  fancybox: true # 启用图片放大功能
  hideFooter: false
  footer:
    hideCopyright: false  # 控制版权信息显示

  # 首页的文章上方的一些信息
  homeInfoParams:
    # 首页的 profile 内容
    Title: "你好 👋"
    # 首页的 profile 内容
    Content: >
      Welcome to my Blog!此博客为本人的学习记录以及各种工具的使用记录，希望对您有所帮助。如果您有任何问题，欢迎在评论区留言，我会尽快回复。
      
  # 社交帐号的按钮
  socialIcons:
    - name: github
      title: Follow my Github
      url: "https://github.com/ForsakenDelusion"
    # - name: X
    #   title: Follow my Twitter
    #   url: "https://x.com/sonnycalcr"
    # - name: Bilibili
    #   title: 关注我的 B 站帐号
    #   url: "https://space.bilibili.com/3493138859559908"
    # - name: Youtube
    #   title: Follow my Youtube Channel
    #   url: "https://youtube.com/@sonnycalcr"
    # - name: Telegram
    #   title: Contact Me
    #   url: "https://t.me/sonnycalcr"

  # 搜索 这里直接按照官方给出的样例配置
  fuseOpts:
      isCaseSensitive: false # 是否大小写敏感
      shouldSort: true # 是否排序
      location: 0
      distance: 1000
      threshold: 0.4
      minMatchCharLength: 0
      # limit: 10 # refer: https://www.fusejs.io/api/methods.html#search
      keys: ["title", "permalink", "summary", "content"]
      includeMatches: true
  # 设置网站的标签页的图标，即 favicon
  assets:
      favicon: "/assets/favicon/favicon.ico"
      favicon16x16: "/assets/favicon/favicon.ico"
      favicon32x32: "/assets/favicon/favicon.ico"
      apple_touch_icon: "/assets/favicon/favicon.ico"
      safari_pinned_tab: "/assets/favicon/favicon.ico"
      disableFingerprinting: true    # 可选，但有时候有帮助    


  # 评论的设置，需要根据个人设置修改
  giscus:
    repo: "" # 
    repoId: ""
    category: ""
    categoryId: ""
    mapping: "pathname"
    strict: "0"
    reactionsEnabled: "1"
    emitMetadata: "0"
    inputPosition: "top"
    lightTheme: "light"
    darkTheme: "dark"
    lang: "zh-CN"
    crossorigin: "anonymous"

# https://github.com/adityatelange/hugo-PaperMod/wiki/Features#search-page
outputs:
  home:
    - HTML # 生成的静态页面
    - RSS # 这个其实无所谓
    - JSON # necessary for search, 这里的配置修改好之后，一定要重新生成一下

markup:
  goldmark:
    renderer:
      unsafe: true # 可以 unsafe，有些 html 标签和样式可能需要
  extensions:
    passthrough:
      delimiters:
        block:
        - - \[
          - \]
        - - $$
          - $$
        inline:
        - - \(
          - \)
      enable: true
  highlight:
    disableHLJS: true
    anchorLineNos: false # 不要给行号设置锚标
    codeFences: true # 代码围栏
    guessSyntax: true
    pygmentsUseClasses: true
    noClasses: false # TODO: 不知道干啥的，暂时没必要了解，不影响展示
    lineNos: false # 代码行 设置成true总是有奇奇怪怪的bug
    lineNumbersInTable: false # 不要设置成 true，否则如果文章开头是代码的话，摘要会由一大堆数字(即代码行号)开头文章
    # 这里设置 style 没用，得自己加 css
    # style: "github-dark"
    # style: monokai

frontmatter:
    date:
    - date
    - publishDate
    - lastmod
    lastmod:
    - lastmod
    - :git
```

接着我们去[这个网站](https://favicon.io/)去生成一下图标。将生成的东西放在根目录下的`/assets/favicon/`下，这样就为我们的网站添加了图标。

### giscus评论系统

采用[# PaperMod 集成 Giscus 评论](https://www.tofuwine.cn/posts/610b75f5/)这篇文章的方案

然后我们还需要为网站添加评论系统。我这里使用了`giscus`作为我的评论系统。首先在网站根目录下的`layouts\partials\`下新建一个`comments.html`文件，然后将以下代码复制进去（这里给出的是适配深浅色主题的方案）,然后根据[引导](https://giscus.app/zh-CN)生成数据，填入`hugo.yaml`中，替换掉我的数据。

```html
<div id="tw-comment"></div>
<script>
    const getStoredTheme = () => localStorage.getItem("pref-theme") === "dark" ? "{{ .Site.Params.giscus.darkTheme }}" : "{{ .Site.Params.giscus.lightTheme }}";
    const setGiscusTheme = () => {
        const sendMessage = (message) => {
            const iframe = document.querySelector('iframe.giscus-frame');
            if (iframe) {
                iframe.contentWindow.postMessage({giscus: message}, 'https://giscus.app');
            }
        }
        sendMessage({setConfig: {theme: getStoredTheme()}})
    }

    document.addEventListener("DOMContentLoaded", () => {
        const giscusAttributes = {
            "src": "https://giscus.app/client.js",
            "data-repo": "{{ .Site.Params.giscus.repo }}",
            "data-repo-id": "{{ .Site.Params.giscus.repoId }}",
            "data-category": "{{ .Site.Params.giscus.category }}",
            "data-category-id": "{{ .Site.Params.giscus.categoryId }}",
            "data-mapping": "{{ .Site.Params.giscus.mapping }}",
            "data-strict": "{{ .Site.Params.giscus.strict }}",
            "data-reactions-enabled": "{{ .Site.Params.giscus.reactionsEnabled }}",
            "data-emit-metadata": "{{ .Site.Params.giscus.emitMetadata }}",
            "data-input-position": "{{ .Site.Params.giscus.inputPosition }}",
            "data-theme": getStoredTheme(),
            "data-lang": "{{ .Site.Params.giscus.lang }}",
            "data-loading": "lazy",
            "crossorigin": "anonymous",
            "async": "",
        };

        // 动态创建 giscus script
        const giscusScript = document.createElement("script");
        Object.entries(giscusAttributes).forEach(
                ([key, value]) => giscusScript.setAttribute(key, value));
        document.querySelector("#tw-comment").appendChild(giscusScript);

        // 页面主题变更后，变更 giscus 主题
        const themeSwitcher = document.querySelector("#theme-toggle");
        if (themeSwitcher) {
            themeSwitcher.addEventListener("click", setGiscusTheme);
        }
        const themeFloatSwitcher = document.querySelector("#theme-toggle-float");
        if (themeFloatSwitcher) {
            themeFloatSwitcher.addEventListener("click", setGiscusTheme);
        }
    });
</script>
```



在 `layouts/partials/extend_head.html` 添加如下内容

```html
{{ partial "math.html" . }}
```

### 在主页显示文章tag信息

参考[# Hugo PaperMod 主题精装修](https://yunpengtai.top/posts/hugo-journey/#%e6%96%87%e7%ab%a0%e5%88%86%e7%b1%bb)和[# 初始化 & 设置 PaperMod 主题的基础功能](https://aikenh.cn/posts/%E5%88%9D%E5%A7%8B%E5%8C%96%E8%AE%BE%E7%BD%AEpapermod%E4%B8%BB%E9%A2%98%E7%9A%84%E5%9F%BA%E7%A1%80%E5%8A%9F%E8%83%BD/#tag-on-meta-info-%e6%96%87%e7%ab%a0%e7%9a%84%e5%85%83%e4%bf%a1%e6%81%af%e4%b8%ad%e6%98%be%e7%a4%ba-tag)
（这一步现在不用添加，在下面添加最后编辑时间支持的时候直接复制整个文件的配置就好了）修改 `layouts/partials/post_meta.html` 中的 `author` 部分如下：

```html
{{- $author := (partial "author.html" .) }}
{{- $tags := (partial "tags.html" .) }}
{{- if $tags }}
    {{- $scratch.Add "meta" (slice $ author $tags) -}}
{{- else}}
    {{- $scratch.Add "meta" (slice $ author) -}}
{{- end}}
```

在`layouts/partials/tags.html`中填入

```html
{{- $tags := .Params.tags -}}
{{- if $tags -}}
  {{- $lastIndex := sub (len $tags) 1 -}}
  {{- range $index, $tag := $tags -}}
    <a href="/tags/{{ $tag | urlize }}"> {{ $tag }}</a>
    {{- if ne $index $lastIndex }}&nbsp;·&nbsp;{{ end -}}
  {{- end -}}
{{- end -}}

```

### 添加最后编辑时间支持

`layouts/partials/post_meta.html`中填入

```html
{{/* 创建一个新的 scratch 对象用于存储元信息 */}}
{{- $scratch := newScratch }}

{{/* 获取文章的发布时间和最后修改时间 */}}
{{ $date := .Date }}
{{ $lastmod := .Lastmod }}

{{/* 添加发布日期信息 */}}
{{- if not .Date.IsZero -}}
{{- $scratch.Add "meta" (slice (printf "<span title='%s'>%s</span>" 
    (.Date) 
    (.Date | time.Format (default "January 2, 2006" site.Params.DateFormat)))) }}
{{- end }}

{{/* 添加最后修改日期信息 
     只在最后修改日期存在且与发布日期不是同一天时显示 */}}
{{- if and (not .Lastmod.IsZero) (ne ($lastmod.Format "2006-01-02") ($date.Format "2006-01-02")) -}}
{{- $scratch.Add "meta" (slice (printf "编辑于<span title='%s'>%s%s</span>" 
    (.Lastmod) 
    (i18n "updated") 
    (.Lastmod | time.Format (default "January 2, 2006" site.Params.DateFormat)))) }}
{{- end }}

{{/* 如果启用了阅读时间显示，添加预计阅读时间 */}}
{{- if (.Param "ShowReadingTime") -}}
{{- $scratch.Add "meta" (slice (i18n "read_time" .ReadingTime | default (printf "%d min" .ReadingTime))) }}
{{- end }}

{{/* 如果启用了字数统计显示，添加文章字数 */}}
{{- if (.Param "ShowWordCount") -}}
{{- $scratch.Add "meta" (slice (i18n "words" .WordCount | default (printf "%d words" .WordCount))) }}
{{- end }}

{{/* 获取作者信息和标签信息 */}}
{{- $author := (partial "author.html" .) }}
{{- $tags := (partial "tags.html" .) }}

{{/* 处理作者和标签的显示逻辑 
     - 如果不隐藏作者，显示作者信息
     - 如果有标签，显示标签信息 */}}
{{- if not (.Param "hideAuthor") -}}
    {{- if $tags }}
        {{- $scratch.Add "meta" (slice $author $tags) -}}
    {{- else}}
        {{- $scratch.Add "meta" (slice $author) -}}
    {{- end}}
{{- else}}
    {{- if $tags }}
        {{- $scratch.Add "meta" (slice $tags) -}}
    {{- end}}
{{- end }}

{{/* 使用 · 符号连接所有元信息并输出 */}}
{{- with ($scratch.Get "meta") }}
{{- delimit . "&nbsp;·&nbsp;" | safeHTML -}}
{{- end -}}
```

### 侧边目录

复制模板文件 `~/themes\PaperMod\layouts\partials\toc.html` 到站点根目录下，替换内容并添加样式：

```html
{{- $headers := findRE "<h[1-6].*?>(.|\n])+?</h[1-6]>" .Content -}}
{{- $has_headers := ge (len $headers) 1 -}}
{{- if $has_headers -}}
<aside id="toc-container" class="toc-container wide">
    <div class="toc">
        <details {{if (.Param "TocOpen") }} open{{ end }}>
            <summary accesskey="c" title="(Alt + C)">
                <span class="details">{{- i18n "toc" | default "Table of Contents" }}</span>
            </summary>
            <div class="inner">
                {{- $largest := 6 -}}
                {{- range $headers -}}
                {{- $headerLevel := index (findRE "[1-6]" . 1) 0 -}}
                {{- $headerLevel := len (seq $headerLevel) -}}
                {{- if lt $headerLevel $largest -}}
                {{- $largest = $headerLevel -}}
                {{- end -}}
                {{- end -}}
                {{- $firstHeaderLevel := len (seq (index (findRE "[1-6]" (index $headers 0) 1) 0)) -}}
                {{- $.Scratch.Set "bareul" slice -}}
                <ul>
                    {{- range seq (sub $firstHeaderLevel $largest) -}}
                    <ul>
                        {{- $.Scratch.Add "bareul" (sub (add $largest .) 1) -}}
                        {{- end -}}
                        {{- range $i, $header := $headers -}}
                        {{- $headerLevel := index (findRE "[1-6]" . 1) 0 -}}
                        {{- $headerLevel := len (seq $headerLevel) -}}
                        {{/* get id="xyz" */}}
                        {{- $id := index (findRE "(id=\"(.*?)\")" $header 9) 0 }}
                        {{- /* strip id="" to leave xyz, no way to get regex capturing groups in hugo */ -}}
                        {{- $cleanedID := replace (replace $id "id=\"" "") "\"" "" }}
                        {{- $header := replaceRE "<h[1-6].*?>((.|\n])+?)</h[1-6]>" "$1" $header -}}
                        {{- if ne $i 0 -}}
                        {{- $prevHeaderLevel := index (findRE "[1-6]" (index $headers (sub $i 1)) 1) 0 -}}
                        {{- $prevHeaderLevel := len (seq $prevHeaderLevel) -}}
                        {{- if gt $headerLevel $prevHeaderLevel -}}
                        {{- range seq $prevHeaderLevel (sub $headerLevel 1) -}}
                        <ul>
                            {{/* the first should not be recorded */}}
                            {{- if ne $prevHeaderLevel . -}}
                            {{- $.Scratch.Add "bareul" . -}}
                            {{- end -}}
                            {{- end -}}
                            {{- else -}}
                            </li>
                            {{- if lt $headerLevel $prevHeaderLevel -}}
                            {{- range seq (sub $prevHeaderLevel 1) -1 $headerLevel -}}
                            {{- if in ($.Scratch.Get "bareul") . -}}
                        </ul>
                        {{/* manually do pop item */}}
                        {{- $tmp := $.Scratch.Get "bareul" -}}
                        {{- $.Scratch.Delete "bareul" -}}
                        {{- $.Scratch.Set "bareul" slice}}
                        {{- range seq (sub (len $tmp) 1) -}}
                        {{- $.Scratch.Add "bareul" (index $tmp (sub . 1)) -}}
                        {{- end -}}
                        {{- else -}}
                    </ul>
                    </li>
                    {{- end -}}
                    {{- end -}}
                    {{- end -}}
                    {{- end }}
                    <li>
                        <a href="#{{- $cleanedID -}}" aria-label="{{- $header | plainify -}}">{{- $header | safeHTML -}}</a>
                        {{- else }}
                    <li>
                        <a href="#{{- $cleanedID -}}" aria-label="{{- $header | plainify -}}">{{- $header | safeHTML -}}</a>
                        {{- end -}}
                        {{- end -}}
                        <!-- {{- $firstHeaderLevel := len (seq (index (findRE "[1-6]" (index $headers 0) 1) 0)) -}} -->
                        {{- $firstHeaderLevel := $largest }}
                        {{- $lastHeaderLevel := len (seq (index (findRE "[1-6]" (index $headers (sub (len $headers) 1)) 1) 0)) }}
                    </li>
                    {{- range seq (sub $lastHeaderLevel $firstHeaderLevel) -}}
                    {{- if in ($.Scratch.Get "bareul") (add . $firstHeaderLevel) }}
                </ul>
                {{- else }}
                </ul>
                </li>
                {{- end -}}
                {{- end }}
                </ul>
            </div>
        </details>
    </div>
</aside>
<script>
    let activeElement;
    let elements;
    window.addEventListener('DOMContentLoaded', function (event) {
        checkTocPosition();
        elements = document.querySelectorAll('h1[id],h2[id],h3[id],h4[id],h5[id],h6[id]');
         // Make the first header active
         activeElement = elements[0];
         const id = encodeURI(activeElement.getAttribute('id')).toLowerCase();
         document.querySelector(`.inner ul li a[href="#${id}"]`).classList.add('active');
     }, false);
    window.addEventListener('resize', function(event) {
        checkTocPosition();
    }, false);
    window.addEventListener('scroll', () => {
        // Check if there is an object in the top half of the screen or keep the last item active
        activeElement = Array.from(elements).find((element) => {
            if ((getOffsetTop(element) - window.pageYOffset) > 0 && 
                (getOffsetTop(element) - window.pageYOffset) < window.innerHeight/2) {
                return element;
            }
        }) || activeElement
        elements.forEach(element => {
             const id = encodeURI(element.getAttribute('id')).toLowerCase();
             if (element === activeElement){
                 document.querySelector(`.inner ul li a[href="#${id}"]`).classList.add('active');
             } else {
                 document.querySelector(`.inner ul li a[href="#${id}"]`).classList.remove('active');
             }
         })
     }, false);
     function checkTocPosition() {
        const width = document.body.scrollWidth;
        
        // 动态获取文章和目录的宽度
        const main = document.querySelector('.main').offsetWidth; // 获取文章的实际宽度
        const toc = document.querySelector('.toc').offsetWidth;   // 获取目录的实际宽度
        const gap = parseInt(getComputedStyle(document.body).getPropertyValue('--gap'), 15);
    
        // 判断是否有足够的空间来显示目录（TOC）
        if (width - main - toc - gap * 2 > 0) {
            document.getElementById("toc-container").classList.add("wide");
        } else {
            document.getElementById("toc-container").classList.remove("wide");
        }
    }
    function getOffsetTop(element) {
        if (!element.getClientRects().length) {
            return 0;
        }
        let rect = element.getBoundingClientRect();
        let win = element.ownerDocument.defaultView;
        return rect.top + win.pageYOffset;   
    }
    
    
</script>
{{- end }}
```

在`assets/css/extended/toc.css`下

```css
:root {
    --margin-width: 300px;
    --toc-width: 300px;
    --gap: 20px; /* 添加 gap 变量来控制两边的间距 */
}

.main {
    max-width: 100%; /* 允许宽度填满父容器 */
    margin-inline-start: auto;
    margin-inline-end: auto;
    margin-left: var(--margin-width); /* 保持默认宽度 */
    margin-right: var(--margin-width); /* 保持默认宽度 */
    line-height: var(--header-height * 0.5);
    transition: margin 0.3s ease; /* 平滑过渡 */
}


.toc {
    margin: 0 2px 40px 2px;
    border: 1px solid var(--border);
    background: var(--entry);
    border-radius: var(--radius);
    padding: 0.4em;
}

.toc-container.wide {
    position: absolute;
    height: 100%;
    left: calc((var(--margin-width) - var(--gap) * 2) * -1);
    top: calc(var(--gap) * 2);
    width: var(--toc-width);
}

.wide .toc {
    position: sticky;
    top:45px ;
    border-radius: 15px;
    width: 80%;
    height: auto;
    margin: 45px 2px 40px 2px;
}

.toc details summary {
    cursor: zoom-in;
    margin-inline-start: 20px;
    padding: 12px 0;
}

.toc details[open] summary {
    font-weight: 500;
}

.toc-container.wide .toc .inner {
    margin: 0;
}

.active {
    font-size: 110%;
    font-weight: 600;
}

.toc ul {
    list-style-type: circle;
}

.toc .inner {
    margin: 0 0 0 20px;
    padding: 0px 15px 15px 30px;
    font-size: 16px;
    max-height: 83vh;
    overflow-y: auto;
}

.toc .inner::-webkit-scrollbar-thumb {
    background: var(--border);
    border: 7px solid var(--theme);
    border-radius: var(--radius);
}

.toc li ul {
    margin-inline-start: calc(var(--gap) * 0.5);
    list-style-type: none;
}

.toc li {
    list-style: none;
    font-size: 0.95rem;
    padding-bottom: 5px;
}

.toc li a:hover {
    color: var(--secondary);
}

/* 媒体查询: 当视口宽度小于 900px 时，取消两边固定宽度 */
@media (max-width: 900px) {
    .main {
        margin-left: 10px; /* 缩小两边间距 */
        margin-right: 10px; /* 缩小两边间距 */
    }

}
```

## 额外功能

以下功能都需要修改`extend_head.html`这个文件，所以我们放在一起说。

首先在`layouts/partials/extended_head.html`中填入我已经修改好的文件，后文我会逐个介绍。

```html
{{- /* Head custom content area start */ -}}

<!-- >>> 数学公式 >>> -->

{{ partial "数学公式" . }}

<!-- >>> 图片放大 >>> -->

<!-- >>> 图片放大 >>> -->

{{if .Page.Site.Params.fancybox }}
<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.css" />
<script src="https://cdn.jsdelivr.net/gh/fancyapps/fancybox@3.5.7/dist/jquery.fancybox.min.js"></script>
{{ end }}

<!-- <<< 图片放大 <<< -->

<!-- >>> 盘古之白 >>> -->

{{- $highlight := resources.Get "js/pangu.min.js" }}
<script>
  (function (u, c) {
    var d = document,
      t = "script",
      o = d.createElement(t),
      s = d.getElementsByTagName(t)[0];
    o.src = "{{ $highlight.RelPermalink }}";  <!-- 使用Hugo变量 -->
    if (c) {
      o.addEventListener("load", function (e) {
        c(e);
      });
    }
    s.parentNode.insertBefore(o, s);
  })("{{ $highlight.RelPermalink }}", function () {
    pangu.spacingPage();
  });
</script>


<!-- <<< 盘古之白 <<< -->

{{- /*     Insert any custom code (web-analytics, resources, etc.) - it will appear in the <head></head> section of every page. */ -}}
{{- /*     Can be overwritten by partial with the same name in the global layouts. */ -}}
{{- /* Head custom content area end */ -}}


```

### 数学公式支持

采用[初始化设置papermod主题的基础功能](https://aikenh.cn/posts/%E5%88%9D%E5%A7%8B%E5%8C%96%E8%AE%BE%E7%BD%AEpapermod%E4%B8%BB%E9%A2%98%E7%9A%84%E5%9F%BA%E7%A1%80%E5%8A%9F%E8%83%BD/#get-wider-space-for-content-%e6%8b%93%e5%ae%bd%e6%ad%a3%e6%96%87%e5%8c%ba%e5%9f%9f)这篇文章的方案。

因为先前给的`hugo,yaml`中已经包含一部分数学公式渲染支持步骤，所以我们只需在`layouts/partials/`中添加`math.html`

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.css"
  integrity="sha512-fHwaWebuwA7NSF5Qg/af4UeDx9XqUpYpOGgubo3yWu+b2IQR4UeQwbb42Ti7gVAjNtVoI/I9TEoYeu9omwcC6g=="
  crossorigin="anonymous" referrerpolicy="no-referrer" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.js"
  integrity="sha512-LQNxIMR5rXv7o+b1l8+N1EZMfhG7iFZ9HhnbJkTp4zjNr5Wvst75AqUeFDxeRUa7l5vEDyUiAip//r+EFLLCyA=="
  crossorigin="anonymous" referrerpolicy="no-referrer"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/contrib/auto-render.min.js"
  integrity="sha512-iWiuBS5nt6r60fCz26Nd0Zqe0nbk1ZTIQbl3Kv7kYsX+yKMUFHzjaH2+AnM6vp2Xs+gNmaBAVWJjSmuPw76Efg=="
  crossorigin="anonymous" referrerpolicy="no-referrer"></script>

<script>
  document.addEventListener("DOMContentLoaded", () =>
    renderMathInElement(document.body, {
      delimiters: [
        { left: '$$', right: '$$', display: true },
        { left: '$', right: '$', display: false }
      ],
      throwOnError: false
    })
  );
</script>
```

### 盘古之白

就是为中文和英文之间加入一个空格，提升阅读体验。

这里采用[# Hugo PaperMod 主题精装修](https://yunpengtai.top/posts/hugo-journey/)这篇文章的方案。首先将`pangu.min.js`文件下载下来放进`assets/js/`目录下。如果你不会下载，请自行`Google`或者问ai。剩下的操作我们在上面已经完成了。

### 图片放大支持

参考[# Hugo博客添加图片点击放大功能](https://tom2almighty.github.io/hugo-blog-papermod/posts/hugo-fancybox/)

在站点目录新建文件 `~/layouts/_default/_markup/render-image.html`，写入内容：
```html
{{if .Page.Site.Params.fancybox }}
<div class="post-img-view">
<a data-fancybox="gallery" href="{{ .Destination | safeURL }}">
<img src="{{ .Destination | safeURL }}" alt="{{ .Text }}" {{ with .Title}} title="{{ . }}"{{ end }} />
</a>
</div>
{{ end }}

```

如果不想要这个功能只需要在`hugo.yaml`修改fancybox的配置 `true` 改为 `false` 即可。

如果想一直使用，`header` 或 `footer` 中 `{{if .Page.Site.Params.fancybox }}` 和 `{{ end }}` 去掉即可。

## 个性化设置

前面提到过留了一个问题后面说，我们的`PaperMod`是以子模块的方式被安装的，这就会导致一个问题，当我们想个性化主题的时候，会发现一个非常难受的点，因为远程仓库关联的是官方的`PaperMod`，所以你修改完了同步不过去，当然解决办法也很多，比如新建一个复制于官方仓库的个人仓库维护，或者添加两个远程仓库之类的方法。但我这里所说的是较为模块化的方法，那就是将`themes/PaperMod/`下的对应文件复制到博客根目录下的相同文件夹内进行修改的方法。比如原本的文件在`themes/PaperMod/assets/css/common/footer.css`，我们就将其复制到`css/common/footer.css`这样。这里我直接给出我的个人仓库修改的东西。大家也可以来我的[github仓库](https://github.com/ForsakenDelusion/blog)进行查看

### 深浅色主题适配

`css/core/theme-vars.css`

```css
:root {
    --gap: 24px;
    --content-gap: 20px;
    --nav-width: 1024px;
    --main-width: 720px;
    --header-height: 60px;
    --footer-height: 60px;
    --radius: 10px;
    --theme: rgb(255, 255, 255);
    --entry: rgb(255, 255, 255);
    --primary: rgb(30, 30, 30);
    --secondary: rgb(108, 108, 108);
    --tertiary: rgb(214, 214, 214);
    --content: rgb(31, 31, 31);
    --code-block-bg: #faf4ed;
    --code-bg: rgb(245, 245, 245);
    --border: rgb(238, 238, 238);

    .post-content pre code {
        color:rgb(125, 102, 102);
    }
}

.dark {
    --theme: rgb(29, 30, 32);
    --entry: rgb(46, 46, 51);
    --primary: rgb(218, 218, 219);
    --secondary: rgb(155, 156, 157);
    --tertiary: rgb(65, 66, 68);
    --content: rgb(196, 196, 197);
    --code-block-bg: #282a36;
    --code-bg: rgb(55, 56, 62);
    --border: rgb(51, 51, 51);

    .post-content pre code {
        color:rgb(213, 213, 214);
    }
}

.list {
    background: var(--code-bg);
}

.dark.list {
    background: var(--theme);
}

```

### hover效果

`css/extended/hover.css`

```css
/* 以下是新增的hover效果 */

/* Light theme hover effects */
.logo a:hover {
    transition: 0.15s;
    color: #666666;  /* 浅灰色用于亮色主题 */
}

svg:hover {
    transition: 0.15s;
}

.social-icons a[href*='github']:hover svg {
    color: #666666 !important;  /* 浅灰色用于亮色主题 */
}

.social-icons a[href*='linkedin']:hover svg {
    color: #0a66c2 !important;  /* LinkedIn 蓝色保持不变 */
}

/* Dark theme hover effects */
body.dark .logo a:hover {
    transition: 0.15s;
    color: #a0a0a0;  /* 更亮的灰色用于暗色主题 */
}

body.dark svg:hover {
    transition: 0.15s;
}

body.dark .social-icons a[href*='github']:hover svg {
    color: #a0a0a0 !important;  /* 更亮的灰色用于暗色主题 */
}

body.dark .social-icons a[href*='linkedin']:hover svg {
    color: #3b82f6 !important;  /* 暗色主题下稍微更亮的蓝色 */
}

#moon:hover {
    transition: 0.15s;
    color: #1772b4;
  }
  
  #sun:hover {
    transition: 0.15s;
    color: #f4a83a;
  }

  #menu a:hover {
    transition: 0.15s;
    color: grey;
  }

```

### 顶部导航栏常驻效果

`css/common/header.css`

```css
.nav {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    /* max-width: calc(var(--nav-width) + var(--gap) * 2); 注释此行以适配顶部导航栏*/
    margin-inline-start: auto;
    margin-inline-end: auto;
    line-height: var(--header-height * 0.5); /* 适配移动端的高度 */
}

.nav a {
    display: block;
}

.logo,
#menu {
    display: flex;
    margin: auto var(--gap);
}

.logo {
    flex-wrap: inherit;
}

.logo a {
    font-size: 24px;
    font-weight: 700;
}

.logo a img, .logo a svg {
    display: inline;
    vertical-align: middle;
    pointer-events: none;
    transform: translate(0, -10%);
    border-radius: 6px;
    margin-inline-end: 8px;
}

button#theme-toggle {
    font-size: 26px;
    margin: auto 4px;
}

body.dark #moon {
    vertical-align: middle;
    display: none;
}

body:not(.dark) #sun {
    display: none;
}

#menu {
    list-style: none;
    word-break: keep-all;
    overflow-x: auto;
    white-space: nowrap;
}

#menu li + li {
    margin-inline-start: var(--gap);
}

#menu a {
    font-size: 16px;
}

#menu .active {
    font-weight: 500;
    border-bottom: 2px solid currentColor;
}

.lang-switch li,
.lang-switch ul,
.logo-switches {
    display: inline-flex;
    margin: auto 4px;
}

.lang-switch {
    display: flex;
    flex-wrap: inherit;
}

.lang-switch a {
    margin: auto 3px;
    font-size: 16px;
    font-weight: 500;
}

.logo-switches {
    flex-wrap: inherit;
}
```

`css/extended/nav-fixed.css`

```css
.header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 99;
    background-color: var(--theme);  /* 使用主题默认背景色 */
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);  /* 添加轻微阴影效果 */
    background-color: rgba(255, 255, 255, 0.3); /* 半透明背景 */
    backdrop-filter: blur(5px); /* 毛玻璃效果 */
    -webkit-backdrop-filter: blur(5px); /* 兼容 Safari */
}

/* 为了防止内容被固定导航栏遮挡，需要为 main 元素添加上边距 */
.main {
    padding-top: 60px;  /* 根据你的导航栏实际高度调整这个值 */
}

body.dark {
    .header {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        z-index: 99;
				box-shadow: 0 2px 2px rgba(255, 255, 255, 0.1);  /* 添加轻微阴影效果 */

        background-color: rgba(29, 30, 32, 0.3); /* 半透明背景 */
        backdrop-filter: blur(5px); /* 毛玻璃效果 */
        -webkit-backdrop-filter: blur(5px); /* 兼容 Safari */
    }
}
```

`css/includes/chroma-styles.css`

```css
/* Generated using: hugo gen chromastyles --style=rose-pine-dawn */

/* Background */ .bg { color:#575279;background-color:#faf4ed; }
/* PreWrapper */ .chroma { color:#575279;background-color:#faf4ed; }
/* Other */ .chroma .x {  }
/* Error */ .chroma .err { color:#b4637a }
/* CodeLine */ .chroma .cl {  }
/* LineLink */ .chroma .lnlinks { outline:none;text-decoration:none;color:inherit }
/* LineTableTD */ .chroma .lntd { vertical-align:top;padding:0;margin:0;border:0; }
/* LineTable */ .chroma .lntable { border-spacing:0;padding:0;margin:0;border:0; }
/* LineHighlight */ .chroma .hl { background-color:#e1dbd5 }
/* LineNumbersTable */ .chroma .lnt { white-space:pre;-webkit-user-select:none;user-select:none;margin-right:0.4em;padding:0 0.4em 0 0.4em;color:#7f7f7f }
/* LineNumbers */ .chroma .ln { white-space:pre;-webkit-user-select:none;user-select:none;margin-right:0.4em;padding:0 0.4em 0 0.4em;color:#7f7f7f }
/* Line */ .chroma .line { display:flex; }
/* Keyword */ .chroma .k { color:#286983 }
/* KeywordConstant */ .chroma .kc { color:#286983 }
/* KeywordDeclaration */ .chroma .kd { color:#286983 }
/* KeywordNamespace */ .chroma .kn { color:#907aa9 }
/* KeywordPseudo */ .chroma .kp { color:#286983 }
/* KeywordReserved */ .chroma .kr { color:#286983 }
/* KeywordType */ .chroma .kt { color:#286983 }
/* Name */ .chroma .n { color:#d7827e }
/* NameAttribute */ .chroma .na { color:#d7827e }
/* NameBuiltin */ .chroma .nb { color:#d7827e }
/* NameBuiltinPseudo */ .chroma .bp { color:#d7827e }
/* NameClass */ .chroma .nc { color:#56949f }
/* NameConstant */ .chroma .no { color:#ea9d34 }
/* NameDecorator */ .chroma .nd { color:#797593 }
/* NameEntity */ .chroma .ni { color:#d7827e }
/* NameException */ .chroma .ne { color:#286983 }
/* NameFunction */ .chroma .nf { color:#d7827e }
/* NameFunctionMagic */ .chroma .fm { color:#d7827e }
/* NameLabel */ .chroma .nl { color:#d7827e }
/* NameNamespace */ .chroma .nn { color:#d7827e }
/* NameOther */ .chroma .nx {  }
/* NameProperty */ .chroma .py { color:#d7827e }
/* NameTag */ .chroma .nt { color:#d7827e }
/* NameVariable */ .chroma .nv { color:#d7827e }
/* NameVariableClass */ .chroma .vc { color:#d7827e }
/* NameVariableGlobal */ .chroma .vg { color:#d7827e }
/* NameVariableInstance */ .chroma .vi { color:#d7827e }
/* NameVariableMagic */ .chroma .vm { color:#d7827e }
/* Literal */ .chroma .l { color:#ea9d34 }
/* LiteralDate */ .chroma .ld { color:#ea9d34 }
/* LiteralString */ .chroma .s { color:#ea9d34 }
/* LiteralStringAffix */ .chroma .sa { color:#ea9d34 }
/* LiteralStringBacktick */ .chroma .sb { color:#ea9d34 }
/* LiteralStringChar */ .chroma .sc { color:#ea9d34 }
/* LiteralStringDelimiter */ .chroma .dl { color:#ea9d34 }
/* LiteralStringDoc */ .chroma .sd { color:#ea9d34 }
/* LiteralStringDouble */ .chroma .s2 { color:#ea9d34 }
/* LiteralStringEscape */ .chroma .se { color:#286983 }
/* LiteralStringHeredoc */ .chroma .sh { color:#ea9d34 }
/* LiteralStringInterpol */ .chroma .si { color:#ea9d34 }
/* LiteralStringOther */ .chroma .sx { color:#ea9d34 }
/* LiteralStringRegex */ .chroma .sr { color:#ea9d34 }
/* LiteralStringSingle */ .chroma .s1 { color:#ea9d34 }
/* LiteralStringSymbol */ .chroma .ss { color:#ea9d34 }
/* LiteralNumber */ .chroma .m { color:#ea9d34 }
/* LiteralNumberBin */ .chroma .mb { color:#ea9d34 }
/* LiteralNumberFloat */ .chroma .mf { color:#ea9d34 }
/* LiteralNumberHex */ .chroma .mh { color:#ea9d34 }
/* LiteralNumberInteger */ .chroma .mi { color:#ea9d34 }
/* LiteralNumberIntegerLong */ .chroma .il { color:#ea9d34 }
/* LiteralNumberOct */ .chroma .mo { color:#ea9d34 }
/* Operator */ .chroma .o { color:#797593 }
/* OperatorWord */ .chroma .ow { color:#797593 }
/* Punctuation */ .chroma .p { color:#797593 }
/* Comment */ .chroma .c { color:#9893a5 }
/* CommentHashbang */ .chroma .ch { color:#9893a5 }
/* CommentMultiline */ .chroma .cm { color:#9893a5 }
/* CommentSingle */ .chroma .c1 { color:#9893a5 }
/* CommentSpecial */ .chroma .cs { color:#9893a5 }
/* CommentPreproc */ .chroma .cp { color:#9893a5 }
/* CommentPreprocFile */ .chroma .cpf { color:#9893a5 }
/* Generic */ .chroma .g {  }
/* GenericDeleted */ .chroma .gd { color:#b4637a }
/* GenericEmph */ .chroma .ge { font-style:italic }
/* GenericError */ .chroma .gr {  }
/* GenericHeading */ .chroma .gh {  }
/* GenericInserted */ .chroma .gi { color:#56949f }
/* GenericOutput */ .chroma .go {  }
/* GenericPrompt */ .chroma .gp {  }
/* GenericStrong */ .chroma .gs { font-weight:bold }
/* GenericSubheading */ .chroma .gu { color:#907aa9 }
/* GenericTraceback */ .chroma .gt {  }
/* GenericUnderline */ .chroma .gl {  }
/* TextWhitespace */ .chroma .w {  }



body.dark {
    
/* Generated using: hugo gen chromastyles --style=dracula */

/* Background */ .bg { color:#f8f8f2;background-color:#282a36; }
/* PreWrapper */ .chroma { color:#f8f8f2;background-color:#282a36; }
/* Other */ .chroma .x {  }
/* Error */ .chroma .err {  }
/* CodeLine */ .chroma .cl {  }
/* LineLink */ .chroma .lnlinks { outline:none;text-decoration:none;color:inherit }
/* LineTableTD */ .chroma .lntd { vertical-align:top;padding:0;margin:0;border:0; }
/* LineTable */ .chroma .lntable { border-spacing:0;padding:0;margin:0;border:0; }
/* LineHighlight */ .chroma .hl { background-color:#3d3f4a }
/* LineNumbersTable */ .chroma .lnt { white-space:pre;-webkit-user-select:none;user-select:none;margin-right:0.4em;padding:0 0.4em 0 0.4em;color:#7f7f7f }
/* LineNumbers */ .chroma .ln { white-space:pre;-webkit-user-select:none;user-select:none;margin-right:0.4em;padding:0 0.4em 0 0.4em;color:#7f7f7f }
/* Line */ .chroma .line { display:flex; }
/* Keyword */ .chroma .k { color:#ff79c6 }
/* KeywordConstant */ .chroma .kc { color:#ff79c6 }
/* KeywordDeclaration */ .chroma .kd { color:#8be9fd;font-style:italic }
/* KeywordNamespace */ .chroma .kn { color:#ff79c6 }
/* KeywordPseudo */ .chroma .kp { color:#ff79c6 }
/* KeywordReserved */ .chroma .kr { color:#ff79c6 }
/* KeywordType */ .chroma .kt { color:#8be9fd }
/* Name */ .chroma .n {  }
/* NameAttribute */ .chroma .na { color:#50fa7b }
/* NameBuiltin */ .chroma .nb { color:#8be9fd;font-style:italic }
/* NameBuiltinPseudo */ .chroma .bp {  }
/* NameClass */ .chroma .nc { color:#50fa7b }
/* NameConstant */ .chroma .no {  }
/* NameDecorator */ .chroma .nd {  }
/* NameEntity */ .chroma .ni {  }
/* NameException */ .chroma .ne {  }
/* NameFunction */ .chroma .nf { color:#50fa7b }
/* NameFunctionMagic */ .chroma .fm {  }
/* NameLabel */ .chroma .nl { color:#8be9fd;font-style:italic }
/* NameNamespace */ .chroma .nn {  }
/* NameOther */ .chroma .nx {  }
/* NameProperty */ .chroma .py {  }
/* NameTag */ .chroma .nt { color:#ff79c6 }
/* NameVariable */ .chroma .nv { color:#8be9fd;font-style:italic }
/* NameVariableClass */ .chroma .vc { color:#8be9fd;font-style:italic }
/* NameVariableGlobal */ .chroma .vg { color:#8be9fd;font-style:italic }
/* NameVariableInstance */ .chroma .vi { color:#8be9fd;font-style:italic }
/* NameVariableMagic */ .chroma .vm {  }
/* Literal */ .chroma .l {  }
/* LiteralDate */ .chroma .ld {  }
/* LiteralString */ .chroma .s { color:#f1fa8c }
/* LiteralStringAffix */ .chroma .sa { color:#f1fa8c }
/* LiteralStringBacktick */ .chroma .sb { color:#f1fa8c }
/* LiteralStringChar */ .chroma .sc { color:#f1fa8c }
/* LiteralStringDelimiter */ .chroma .dl { color:#f1fa8c }
/* LiteralStringDoc */ .chroma .sd { color:#f1fa8c }
/* LiteralStringDouble */ .chroma .s2 { color:#f1fa8c }
/* LiteralStringEscape */ .chroma .se { color:#f1fa8c }
/* LiteralStringHeredoc */ .chroma .sh { color:#f1fa8c }
/* LiteralStringInterpol */ .chroma .si { color:#f1fa8c }
/* LiteralStringOther */ .chroma .sx { color:#f1fa8c }
/* LiteralStringRegex */ .chroma .sr { color:#f1fa8c }
/* LiteralStringSingle */ .chroma .s1 { color:#f1fa8c }
/* LiteralStringSymbol */ .chroma .ss { color:#f1fa8c }
/* LiteralNumber */ .chroma .m { color:#bd93f9 }
/* LiteralNumberBin */ .chroma .mb { color:#bd93f9 }
/* LiteralNumberFloat */ .chroma .mf { color:#bd93f9 }
/* LiteralNumberHex */ .chroma .mh { color:#bd93f9 }
/* LiteralNumberInteger */ .chroma .mi { color:#bd93f9 }
/* LiteralNumberIntegerLong */ .chroma .il { color:#bd93f9 }
/* LiteralNumberOct */ .chroma .mo { color:#bd93f9 }
/* Operator */ .chroma .o { color:#ff79c6 }
/* OperatorWord */ .chroma .ow { color:#ff79c6 }
/* Punctuation */ .chroma .p {  }
/* Comment */ .chroma .c { color:#6272a4 }
/* CommentHashbang */ .chroma .ch { color:#6272a4 }
/* CommentMultiline */ .chroma .cm { color:#6272a4 }
/* CommentSingle */ .chroma .c1 { color:#6272a4 }
/* CommentSpecial */ .chroma .cs { color:#6272a4 }
/* CommentPreproc */ .chroma .cp { color:#ff79c6 }
/* CommentPreprocFile */ .chroma .cpf { color:#ff79c6 }
/* Generic */ .chroma .g {  }
/* GenericDeleted */ .chroma .gd { color:#f55 }
/* GenericEmph */ .chroma .ge { text-decoration:underline }
/* GenericError */ .chroma .gr {  }
/* GenericHeading */ .chroma .gh { font-weight:bold }
/* GenericInserted */ .chroma .gi { color:#50fa7b;font-weight:bold }
/* GenericOutput */ .chroma .go { color:#44475a }
/* GenericPrompt */ .chroma .gp {  }
/* GenericStrong */ .chroma .gs {  }
/* GenericSubheading */ .chroma .gu { font-weight:bold }
/* GenericTraceback */ .chroma .gt {  }
/* GenericUnderline */ .chroma .gl { text-decoration:underline }
/* TextWhitespace */ .chroma .w {  }
}

```

## 部署到Github Pages（已弃用）

这里就不详细赘述了，按照 Hugo 的[文档](https://gohugo.io/hosting-and-deployment/hosting-on-github/)指导来操作即可。

## 部署到Vercel

因为`GitHub Pages`在国内访问速度过于慢的原因，我将静态托管迁移到`Vercel`。因为`Vercel`自身域名被墙的原因，建议有自己域名的同学们更换。

首先关闭掉`GitHub Pages`，然后注册`vercel`账户，直接导入你的`blog`仓库，然后再部署页面的框架选择`hugo`，这里注意一下，需要设置环境变量`HUGO_VERSION`为你构建时的`hugo`版本，我的为`0.141.0`。然后再按照`vercel`给你的提示设置域名解析。

使用`Vercel`构建，不仅访问速度快，发布构建的速度也远超`GitHub Pages`

## 结尾

支持配置应该就结束了，如果有疑问或者错误，欢迎评论。后续还会分享一些配合obsidian的工作流。

感谢以下文章

[Hugo + PaperMod + Github Pages 搭建一个完善的个人博客(以 Windows11 为例)](https://sonnycalcr.github.io/posts/build-a-blog-using-hugo-papermod-github-pages)
[Hugo 主题配置](https://lifeislife.cn/posts/hugo%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE/)
[初始化 & 设置 PaperMod 主题的基础功能](https://aikenh.cn/posts/%E5%88%9D%E5%A7%8B%E5%8C%96%E8%AE%BE%E7%BD%AEpapermod%E4%B8%BB%E9%A2%98%E7%9A%84%E5%9F%BA%E7%A1%80%E5%8A%9F%E8%83%BD)
[Hugo+PaperMod 双语博客搭建 Home-Info+Profile Mode](https://www.yunyitang.me/hugo-papermod-blog/)
[Hugo PaperMod 主题精装修](https://yunpengtai.top/posts/hugo-journey/#%e6%96%87%e7%ab%a0%e5%88%86%e7%b1%bb)
[Hugo Stack 主题添加「最后修改于」](https://shitao5.org/posts/hugo-stack/)
[Hugo 部署方案](https://www.nikunokoya.com/posts/hugo_deploy/)
[PaperMod 集成 Giscus 评论](https://www.tofuwine.cn/posts/610b75f5/)
[Hugo博客添加图片点击放大功能](https://tom2almighty.github.io/hugo-blog-papermod/posts/hugo-fancybox/)
