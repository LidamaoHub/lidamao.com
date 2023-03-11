
---
title: 前端黑魔法--纯css折角头像的实现
banner: /images/2015/12/Lucerne--Switzerland--1-.jpg
cateogries: 
date: 2015-12-22 05:54:51
---
<!--kg-card-begin: markdown--><p>今天公司的设计师提交了一个文艺的需求，设计一款文艺头像，如下图<br>
<img src="/images/2015/12/20150417203159225.jpg" alt="" loading="lazy"><br>
要求是在不切图的情况下（服务器端保存原图）制作折角头像，在鼠标滑过时边框颜色产生变化如下图。<br>
<img src="/images/2015/12/20150417203604988.jpg" alt="" loading="lazy"><br>
细想之下这个要求简直不科学，因为图片的border怎么会有这种奇怪形状？！</p>
<p>但是有一个可以偷懒的办法是，整个图片背景色是固定的（如图中是白色）</p>
<p>以下是我的解决方案：</p>
<p>①最底层是头像文件，有一圈10px的红色border<br>
②右下角罩一个三角形，使用z-index来使其浮在图片上边<br>
③再在最上边罩一个跟底色通过色的三角形，使两个三角形大小差值（露出的那块面积）正好看起来像右下角的border</p>
<p>整体看起来就像下图（就像一个三明治）</p>
<p><img src="/images/2015/12/20150417211229257.jpg" alt="" loading="lazy"><br>
那么就会存在两个问题</p>
<p>①使用图片来遮罩未免有些low，而且如果过后设计告诉我要调整border粗细或者遮罩面积，还得重新作图<br>
②如何实现当鼠标滑过时头像border和中间层三角形同时变色？</p>
<p>首先解决第一个问题，在网上找了一圈后发现，很多花里胡哨的图片都能够通过border来绘制<br>
如图，其实一个元素的border是这样的<br>
<img src="/images/2015/12/20150417205559841.jpg" alt="" loading="lazy"><br>
这是一个50px<em>50px的方块，它border的四个角都是以斜线的方式分割，如果把这个图形设定为0px</em>0px，border都为100px时，是下面这样的<br>
<img src="/images/2015/12/20150417210125076.jpg" alt="" loading="lazy"><br>
所以如果我想要一个右下角三角形，那么我只需不设左边上边border，如图</p>
<p><img src="/images/2015/12/20150417210330323.jpg" alt="" loading="lazy"><br>
嘿嘿，成功！</p>
<p>接下来解决下一个问题，同时改变</p>
<p>网上搜索答案为</p>
<p><code>.main:hover .cover {}</code></p>
<p>其中main为照片的父元素，cover为中间层遮罩</p>
<pre><code>.main:hover .cover {} 
.main:hover img {}
</code></pre>
<p>即可一个hover多个变化</p>
<p>p.s.：切记元素和“{}”间有个空格</p>
<p>接下来是实现代码</p>
<pre><code>&lt;html&gt;

&lt;head&gt;
    &lt;style&gt;
        div {
            box-sizing: border-box;
            -moz-box-sizing: border-box;
            Firefox -webkit-box-sizing: border-box;
        }

        .container {
            margin-left: auto;
            margin-right: auto;
            margin-top: 100px;
        }

        .main {
            position: relative;
            width: 120px;
            height: 120px;
        }

        .cover {
            width: 0;
            height: 0;
            border: 20px solid transparent;
            border-bottom-color: red;
            border-right-color: red;
            top: -50px;
            position: absolute;
            top: 80px;
            left: 80px;
            z-index: 2;
        }

        .main:hover .cover {
            border-bottom-color: blue;
            border-right-color: blue;
        }

        .imgg {
            width: 100px;
            height: 100px;
            background-image: url(head.jpg);
            background-size: 120px;
            position: absolute;
            border: 10px solid red;
        }

        .main:hover .imgg {
            border-color: blue;
        }

        .cover-real {
            content: '';
            width: 0;
            height: 0;
            border: 13px solid transparent;
            border-bottom-color: white;
            border-right-color: white;
            top: -50px;
            position: absolute;
            top: 96px;
            left: 96px;
            z-index: 3;
        }
    &lt;/style&gt;

    &lt;link href=&quot;style.css&quot; rel=&quot;stylesheet&quot;&gt;

&lt;/head&gt;

&lt;body&gt;
    &lt;div class=&quot;main container&quot;&gt;
        &lt;img src=&quot;head.jpg&quot; class=&quot;imgg&quot;&gt;
        &lt;div class=&quot;cover&quot;&gt;&lt;/div&gt;
        &lt;div class=&quot;cover-real&quot;&gt;&lt;/div&gt;
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>
<p>谢谢观看</p>
<p><img src="/images/2015/12/20150417211531371.jpg" alt="" loading="lazy"></p>
<!--kg-card-end: markdown-->
    