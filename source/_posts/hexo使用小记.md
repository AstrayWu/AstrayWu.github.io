---
title: hexo使用小记
date: 2019-04-16 03:26:21
tags:
mathjax: true
---

## 1. 常用的命令

```bash
hexo new tile_of_poster
hexo g # hexo generate
hexo s # hexo server
hexo d # hexo deploy
```

正常情况下要推送到网站使用，随后即可到www.github.io查看网页了。

```sh
hexo clean
hexo g
hexo d
```

要备份代码到github时

```sh
git push origin hexo # 因为master分支是用于备份网页文件的
```

## 2. 常见问题

1. 数学公式
   在hexo中渲染数学公式需要Mathjax，可以参考[hexo下LaTeX无法显示的解决方案](https://blog.csdn.net/crazy_scott/article/details/79293576)
   [[NexT]配置MathJax](https://hexo-guide.readthedocs.io/zh_CN/latest/theme/[NexT]%E9%85%8D%E7%BD%AEMathJax.html) 参考这里也可以，注意`per_page`配置为`true`时，要在blog中主动带上`mathjax: true`
2. hexo中支持的一些语法可以参考[Hexo Markdown 简明语法手册](https://hyxxsfwy.github.io/2016/01/15/Hexo-Markdown-%E7%AE%80%E6%98%8E%E8%AF%AD%E6%B3%95%E6%89%8B%E5%86%8C/)
3. Mathjax英文可以参考[MathJax basic tutorial and quick reference](https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference/5044)，中文翻译参考[Mathjax与LaTex公式简介](https://www.cnblogs.com/linxd/p/4955530.html)
4. Mathjax的docs，可以查到相关的语法支持情况[hMathJax TeX and LaTeX Support](http://docs.mathjax.org/en/latest/tex.html)
5. 当前markdown渲染引擎不支持todolist[Port todo list support from marked](https://github.com/sun11/hexo-renderer-kramed/pull/1)，有需要的话未来可以自己合并。
6. 使用kramed需要在文档头中加`mathjax: true`
7. Katex中不支持`\label`和`\eqref`，目前站点渲染这个也无法做到，但Mathjax应该可以做到。
8. [代码不让它显示行数序号数字怎搞](https://github.com/iissnan/hexo-theme-next/issues/211)
9. [取消hexo中的目录编号](https://segmentfault.com/q/1010000008494901)，可以使用vs中的toc功能加编号会更好些。
10. 如何备份hexo的blog源代码
    可以参考[使用hexo，如果换了电脑怎么更新博客？](https://www.zhihu.com/question/21193762)
    如果已经完成了blog的编写，可以在github上面新建一个`hexo`分支，并设置为默认分支，随后将远程分支下载下来，并用blog的源文件全部替换掉，在push即可。这样就可以将源代码备份到Github上面的hexo分支，而网页文件存储在master分支。
11. [NexT 自带的语法高亮插件支持的语言列表](https://blog.cat73.org/20160805/2016080501.next-highlight-langs/) 注意`shell`应该使用`sh`会比较统一一些。
12. [TypeError: Cannot set property 'lastIndex' of undefined github](https://github.com/hexojs/hexo/issues/2380)
13. [用Markdown写Hexo博客时如何转义竖杠 | ？](https://www.zhihu.com/question/37542455) 使用HTML字符修改
14. [如何在hexo使用图片和提供附件下载](http://abonege.github.io/2016/05/25/%E5%A6%82%E4%BD%95%E5%9C%A8hexo%E4%BD%BF%E7%94%A8%E5%9B%BE%E7%89%87/) 提供附件下载功能，其实就是开启在开启post_asset_folder后，在同名文件夹下面放附件就可以了，类似于图片

## 3. 添加gitalk评论系统

- [Hexo NexT主题中集成gitalk评论系统](https://asdfv1929.github.io/2018/01/20/gitalk/)
- [Hexo中Gitalk配置使用教程-可能是目前最详细的教程](https://iochen.com/2018/01/06/use-gitalk-in-hexo/)
- [GITALK demo](https://gitalk.github.io/)
- [OAuth Apps](https://github.com/settings/developers)

主要参考了以上两个教程，但事实上目前（2019.4）在next主题中都已经集成了gitalk的相关配置了，直接启用即可，不需要像第一个blog那样需要完整添加很多配置文件。以后可以方便添加评论然后重新记录了。

1. 先在[github](https://github.com/settings/applications/new)上注册新应用
   参数说明

    ```bash
    Application name： # 应用名称，随意
    Homepage URL： # 网站URL，如https://asdfv1929.github.io
    Application description # 描述，随意
    Authorization callback URL：# 网站URL，https://asdfv1929.github.io
    ```

2. 点击注册后，页面跳转如下，其中Client ID和Client Secret在后面的配置中需要用到，到时复制粘贴即可
3. 在主题配置文件next/_config.yml中添加如下内容：

    ```bash
    gitalk:
        enable: true
        github_id: astraywu # Github repo owner
        repo: astraywu.github.io # Repository name to store issues.
        client_id: Client ID # Github Application Client ID
        client_secret: Client Secret # Github Application Client Secret
        admin_user: astraywu # GitHub repo owner and collaborators, only these guys can initialize github issues
        distraction_free_mode: true # Facebook-like distraction free mode
    ```

## 4. next主题优化

### 4.1. 添加tag

[hexo 下的分类和表签无法显示，怎么解决？](https://www.zhihu.com/question/29017171)

1. 给文章加tag

   ```text
    title: 一个人的狂欢
    date: 2016-11-11 13:09:04
    tags: [journal,think]
   ```

2. 命令行输入`hexo n page tags`，会在`sources/tags`生成`index.md`文件
3. 编辑`index.md`文件

   ```text
    title: tags
    date: 2016-11-11 21:40:58
    type: "tags"
   ```

4. 在主题配置文件`themes\next\_config.yml`的`menu`项目下打开`tags`页

   ```yml
    menu:
        home: / || home
        #about: /about/ || user
        tags: /tags/ || tags
        #categories: /categories/ || th
        archives: /archives/ || archive
        #schedule: /schedule/ || calendar
        #sitemap: /sitemap.xml || sitemap
        #commonweal: /404/ || heartbeat
   ```

### 4.2. 浏览页面的时候显示当前浏览进度

### 4.3. 添加顶部加载条

### 4.4. 本地搜索

### 4.5. 统计功能，统计功能,显示文章字数统计,阅读时长,总字数

### 4.6. 允许代码段拷贝

这里要使用`npm install hexo-symbols-count-time --save`

### 4.7. 给代码块添加复制功能

### 4.8. 修改行间距

文章内容较多时，next原来的间距太大了，要上下来回滚动。
修改`themes\next\source\css\_variables\custom.styl`，添加

```styl
$line-height-base           = 1.5
```

## 5. ref

1. [Hexo的Next主题详细配置](https://www.jianshu.com/p/3a05351a37dc)
2. [精于心，简于形](https://theme-next.iissnan.com/) Next官网
3. [GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249) 知乎的hexo建站笔记
4. [利用vscode插件与git hook提升hexo编写部署体验](https://www.jianshu.com/p/a117650f6c76)
5. [Hexo博客搭建之在文章中插入图片](https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/)
6. [绝配：hexo+next主题及我走过的坑](https://www.jianshu.com/p/21c94eb7bcd1) 搭建更多功能
7. [Hexo+Next主题优化](https://zhuanlan.zhihu.com/p/30836436?utm_source=ZHShareTargetIDMore&utm_medium=social&utm_oi=898539620818640896)
8. [Hexo-Next-主题优化(一)](https://www.jianshu.com/p/3ff20be8574c) 应该是上面的ref，带有图片演示
9. [hexo指南](https://hexo-guide.readthedocs.io/zh_CN/latest/index.html)