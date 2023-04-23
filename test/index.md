# test


<!--more-->

1，hugo网站的目录结构

- config.toml
  站点全局的参数配置文件**（包含指定主题对应的各种参数，在这里可以修改网站页面的布局）**
- archetypes
  Hugo的markdown文件中前置数据Front Matter定义的结构，默认使用的是default.md文件，可以自定义，然后在config.toml中指定自定义的结构文件，打开default.md文件。至于喜欢哪种格式，可以在config.toml中进行配置，默认使用的是yaml格式。通过执行hugo new 命令生成的markdown文件，头部默认会有这段渲染之后的Front Matter，一般我们会把draft属性去掉，draft草稿的意思，有这个属性的md文件不会渲染输出， 当然通过hugo –buildDrafts可以强制输出。**（markdown文章的模版,包括文章前缀注释写法，可以为文章在博客中的导览提供信息）**

- content
  存放网页内容的目录，即我们编写的markdown文件都存放在此目录，此目录是Hugo的默认源目录，在E:/website/second-blog下执行命令 `hugo new post/hugo_introduce.md`之后， 则会在content目录下生成子目录post，post中有一个hugo_introduce.md文件。**（文章内容的存放地点，我使用的是loveit主题，会包含一些其他的结构）**

- data
  data目录用来存放数据文件，一般是json文件，Hugo提供了相关命令可以从data目录下读取相关的文件数据，然后渲染到HTML页面中，将业务数据与模板分离。**（数据文件，很重要但是不用管）**

- layouts
  存放自定义的模板文件，Hugo优先使用layouts目录下的模板，未发现再去themes目录下查找。**（自定义模板）**

- static
  存放静态文件，比如css、js、img等文件目录，Hugo在渲染时，会直接将static目录下的文件直接复制到public目录下，不会做任何渲染。**（一些静态文件）**

- themes
  存放网站主题，可以下载多个主题，themes目录下的每个子目录代表了一个主题，可以通过在config.toml中通过参数theme指定主题，即theme目录下的子目录名字， 也可以在执行hugo命令渲染时通过增加flag参数–theme=xx指定。**（所用主题）**

- assets # 静态文件，比如文章中的图片/视频文件、css等, 将来其下的子目录和文件会在生成时候会自动复制到 public 目录中. **（我一般将网站固定的一些图片放在这里，文章的图片和文章放一起）**

- resources # 通过hugo命令一起生成的资源文件，貌似是临时文件**（不用管）**

  ![Apple-Devices-Preview](Apple-Devices-Preview.png)


