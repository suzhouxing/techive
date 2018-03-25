---
title: Hexo 中使用数学公式
date: 2017-09-03 10:29:59
categories:
- Hexo
tags:
- 技术
- 博客
- Hexo
- 数学公式
- MathJax
- LaTeX
- Bug
---
为了在 Hexo 生成的网站里面显示 LaTeX 书写风格的数学公式, 尝试了不少方法.
比如官方的 `hexo-math`, 还有别人提到的 `hexo-renderer-mathjax`, 以及 `hexo-renderer-pandoc`.
但是始终不能正确显示数学公式.
最后用安装 `hexo-renderer-pandoc` 并在每个用到了数学公式的 markdown 文件里添加

```html
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.3/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
```

的方式实现了数学公式的渲染.
但是这个方案显然难以令人满意, 我还是更希望有个自动化的方法.



# 定位问题

折腾了半天, 最后发现是 `hexo-inject` 的问题.
不知道是因为 `hexo-inject` 的 bug 还是其他原因, 生成的 html 代码里面会出现一些 `<!-- hexo-inject:begin --><!-- hexo-inject:end -->`.
在其他地方出现都没什么问题, 因为这段代码在 html 中是注释, 但是在 `<script></script>` 中出现时就会被当成无法解析的 javascript 代码.
于是渲染数学公式时就悲剧了.
另外, 嵌入 Google Analytic 的代码好像也会出现类似的问题, 但是很奇怪百度统计居然没问题.

后来在 `hexo-math` 和 `hexo-inject` 的 issue 里看到 hexo 的维护者说 `hexo-inject` 跟 `hexo-renderer-jade` 冲突了.



# 解决方案

## 删除有 bug 的插件

执行以下命令删除 `hexo-inject` 插件, 并默默祈祷没有别的插件或者主题依赖这个插件.

```bash
npm uninstall hexo-inject --save
```

## 嵌入 MathJax 的代码

在 `themes/maupassant/layout/_partial/head.jade` 的末尾添加以下代码.

```jade
script(type='text/javascript', src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.3/MathJax.js?config=TeX-MML-AM_CHTML", async)
```

其中 `maupassant` 的位置填你自己选择的主题名字.
`head.jade` 是嵌入上面的脚本的位置, 不一定非要在 head 里, 而且后缀可能不同主题的不一样 (使用了不同的引擎).
我这里选择了和主题原有文件一致的后缀, 因为主题已经声明了其依赖的插件, 不用担心无法解析的问题.



# 结果

下面是配置成功之后的显示效果:

```latex
$$
\min \sum_{i=0}^{+\infty} \frac{\exp{x^2}}{\sqrt{y}}
$$
```

$$
\min \sum_{i=0}^{+\infty} \frac{\exp{x^2}}{\sqrt{y}}
$$



# 总结

值得一提的是, `hexo-math` 所依赖的插件 `hexo-inject` 似乎已经被作者废弃了, 感觉 `hexo-math` 也因此成了坑.
而且就算 `hexo-inject` 的 bug 修复了, `hexo-math` 还有下标需要给下划线加反斜杠转义的问题, 仍然不能和 LaTeX 公式无缝对接.

`hexo-renderer-mathjax` 作者也多年没有更新, 现在 MathJax 的 CDN 已经不再提供服务, 但是作者一直没有更新可用的地址.

`hexo-renderer-pandoc` 还依赖第三方软件, 不能使用 npm 统一管理, 确实麻烦了一点.
但是作为一个经常用 Markdown 写初稿或者项目文档, 最后再用 LaTeX 整理论文的人, Typora 和 Pandoc 之类的工具基本上是装机必备, 好像没啥影响.

最后还是不得不感慨开源项目的最大缺点, 就是很难保证可持续性 -- 依赖的工具更新了它可能不能及时更新已适应新版本, 或者用户发现了 bug 也很难及时修复.
感觉还是有大企业的维护开源项目才能产出最良心最好用的软件.



# 参考资料

[1] https://blog.yuanbin.me/posts/2014/05/play-mathjax-with-pandoc.html
