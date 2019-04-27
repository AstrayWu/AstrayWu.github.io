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

```shell
hexo clean
hexo g
hexo d
```

## 2. 常见问题

1. 在hexo中渲染数学公式需要Mathjax，可以参考[hexo下LaTeX无法显示的解决方案](https://blog.csdn.net/crazy_scott/article/details/79293576)
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

## 3. 参考资料

1. [Hexo的Next主题详细配置](https://www.jianshu.com/p/3a05351a37dc)
2. Next官网[精于心，简于形](https://theme-next.iissnan.com/)
3. 知乎的hexo建站笔记[GitHub+Hexo 搭建个人网站详细教程](https://zhuanlan.zhihu.com/p/26625249)
4. [利用vscode插件与git hook提升hexo编写部署体验](https://www.jianshu.com/p/a117650f6c76)
5. [Hexo博客搭建之在文章中插入图片](https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/)

