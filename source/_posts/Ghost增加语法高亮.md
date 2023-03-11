
---
title: Ghost增加语法高亮
banner: /images/2015/12/Untitled.jpg
cateogries: 
date: 2015-12-22 05:57:22
---
<!--kg-card-begin: markdown--><p>刚刚迁移到Ghost博客上,感觉一切都是棒棒哒,优雅的首页简洁的后台操作(相比Wordpress那复杂的后台).但是正如世间的一切事物,Ghost也有其不够优雅的地方,比如她没办法实现代码高亮<br>
<img src="/images/2015/12/-----2015-07-17---1-06-06.png" alt="" loading="lazy"><br>
这是很多人都不能忍的</p>
<p>不过还好有&quot;highlight.js&quot;这款神器,它的官方介绍中</p>
<ul>
<li>支持 71 种编程语言的语法解析并且拥有 44 种样式</li>
<li>自动检测编程语言</li>
<li>同时为多种编程语言代码高亮</li>
<li>支持各种标签</li>
<li>与任何 js 框架兼容</li>
</ul>
<p>当然以上是直接复制来的Orz</p>
<p>这里是我修改过的highlight的代码和样式表</p>
<p><a href="http://7u2nd7.com1.z0.glb.clouddn.com/highlighthighlight.min.js">highlight.js</a><br>
<a href="http://7u2nd7.com1.z0.glb.clouddn.com/highlight_sublime.min.css">css样式表</a></p>
<p>点击即可下载,然后保存在你的服务端上或者cdn上边~</p>
<p>接着把引用代码复制在后台的插入代码中<br>
<img src="/images/2015/12/-----2015-07-17---1-37-06.png" alt="" loading="lazy"><br>
代码如下</p>
<pre><code>&lt;link href=&quot;css_add/highlight_sublime.min.css&quot; rel=&quot;stylesheet&quot;&gt;
&lt;script src=&quot;js_add/highlight.min.js&quot;&gt;&lt;/script&gt;
&lt;script &gt;hljs.initHighlightingOnLoad();&lt;/script&gt;//这里是启动hightlight用的
</code></pre>
<p>当然老规矩,css代码插入页头,js脚本插入页尾.再次刷新页面,是不是萌萌哒呢?<br>
<img src="/images/2015/12/20150420155818068-1-1.jpg" alt="" loading="lazy"></p>
<!--kg-card-end: markdown-->
    