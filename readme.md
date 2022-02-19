# Readme

使用hexo 搭建博客非常方便。

不过我们需要迁移到其它电脑时，**备份源文档**非常重要，本文就是为备份 hexo 源码包的简要说明。


## 一、哪些文档需要备份

只需要备份如下文件夹及文件即可：

```shell
#有[]表示文件夹，其它为文件

---- [source] #所有的 markdown 原始文件和目录「重要」
---- [themes] #hexo 的主题，也可以不备份
---- package.json #搭建hexo，安装依赖包时需要「重要」
---- _config.yml #configure 配置文件「重要」
---- readme.md #本说明
```

## 二、全新安装 hexo 需要的包

> 安装 hexo 之前，需要先安装好 **node.js** 和 **git**，本文不详述。
>
> node.js 的 npm (node包管理器) 用于安装下面要提到的依赖包；git用于上传到github 代码仓库，以更新线上的博客 (如：https://yourblogname.github.io/)。

```shell
$ npm install hexo-browsersync --save #内容监视，.md文件有修改时浏览器端自动刷新
$ npm install hexo-deployer-git --save #发布代码到 github

# 安装 even 主题(theme) 需要的包
$ npm install hexo-renderer-scss --save #解析 sass
$ npm install hexo-renderer-swig --save #解析 swig
```

## 三、自动解析图片的发布路径

hexo-renderer-marked 3.1.0 引入了一个新选项，允许您在 Markdown 中嵌入图像而不使用 asset_img 标签插件。

首先检查是否安装了依赖包：**`hexo-renderer-marked`**，如果没有，先安装：

```shell
$ npm install hexo-renderer-marked --save
```

**配置根目录下的 `_config.yml` 文档以启用该功能**：

```yaml
#_config.yml
post_asset_folder: true
marked:
  prependRoot: true
  postAsset: true
```

启用后，图像将自动解析为其相应帖子的路径。例如，`image.jpg` 位于 `/2020/01/02/foo/image.jpg`，表示它是 `/2020/01/02/foo/` 帖子的资产图片，`![]( image.jpg)` 将被渲染为 `<img src="/2020/01/02/foo/image.jpg">`。

详见：[Embed image](https://hexo.io/docs/tag-plugins#Embed-image)、[Embedding an image using markdown](https://hexo.io/docs/asset-folders#Embedding-an-image-using-markdown)。

## 四、even 主题的一处样式问题

Hexo 会将无序列表项 (即：`ul li`) 的内容解析并放置于 `<p>` 标签内，even 主题默认情况对这种结构的处理有一点问题，无序列表的 `::marker` 和 `<p>` 标签会分行显示，解决方法如下：

```sass
//找到并修改：themes/even/source/css/_partial/_post/_content.scss
ul {
    list-style: inside; //删除这一行
    margin-left: 1em;   //增加这一行
    padding-left: 0;

    li {
      input[type="checkbox"] {
        margin-right: 5px;
      }
    }
  }
```