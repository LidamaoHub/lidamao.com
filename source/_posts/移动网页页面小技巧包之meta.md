
---
title: 移动网页页面小技巧包之meta
banner: /images/2017/01/2016021673214_112.jpg
cateogries: 
date: 2017-01-02 15:03:26
---
<!--kg-card-begin: markdown--><p>前段时间自己做了『点个赞社区』这个产品，所以接触到移动端的一些小技巧，现在总结如下</p>
<p>一 meta标签篇</p>
<p>移动端开发有很多问题要考虑，包括页面显示和一些特殊的因素，这里就引进我们的<code>&lt;meta&gt;</code>标签</p>
<pre><code class="language-html">&lt;meta name=&quot;viewport&quot; content=&quot;width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no&quot; /&gt; 
</code></pre>
<p>这条主要功能是设置视口宽度等于设备宽度，并禁止缩放。</p>
<pre><code class="language-html">&lt;meta name=&quot;format-detection&quot; content=&quot;telephone=no&quot; /&gt;
</code></pre>
<p>这条禁止将页面中的数字识别为电话号码。</p>
<blockquote></blockquote>
<p><code>&lt;meta&gt;</code>标签由<code>name</code>和<code>content</code>构成，<code>name</code>规定了这条<code>&lt;meta&gt;</code>标签的作用，<code>content</code>规定了视觉区域的参数，内容以『，』隔开</p>
<p>以下是name和对应的content的一些详细解释：</p>
<p>① viewport -- 视觉大小</p>
<p><code>width</code>规定了视觉范围宽度，一般选择<code>device-width</code>即可以机器宽度作为宽度</p>
<p><code>initial-scale</code>规定用户打开页面后的默认拉伸（同理还有<code>maximum-scale</code>和<code>minimum-scale</code>最大最小拉伸）</p>
<p><code>user-scalable=no</code>规定了用户不能拉伸页面，我认为这是很重要的一条。因为很多浏览器为了包容没有响应式设计的老网站而自作聪明的在用户点击<code>&lt;input&gt;</code>时改变页面拉伸（这特么是最骚的），对于精心设计排版的移动网站完全是核打击</p>
<p>② keywords -- 关键字</p>
<pre><code>&lt;meta name=&quot;keywords&quot; content=&quot;李大猫,可爱,技术&quot;&gt;
</code></pre>
<p>告诉搜索引擎你的页面是干嘛的，方便浏览器收录</p>
<p>③ description -- 内容简述</p>
<pre><code>&lt;meta name=&quot;description&quot; content=&quot;颠沛流离的诗人看到海&quot;&gt;
</code></pre>
<p>添加description是为了在搜索引擎上表明这个页面的描述（类似副标题或者简述）</p>
<p>④ apple-mobile-web-app-capable -- Safari隐藏地址栏</p>
<p>这条小技巧只适用于ios上的safari，如果你的app想做成webapp的样子可以考虑这条效果</p>
<p>同时可以使用</p>
<pre><code>&lt;meta name=&quot;apple-mobile-web-app-status-bar-style&quot; content=&quot;black&quot; /&gt;
</code></pre>
<p>来定义显示手机信号、时间、电池的顶部导航栏的颜色。默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。这个主要是根据实际的页面设计的主体色为搭配来进行设置。</p>
<p>ios7以前系统默认会对图标添加特效（圆角及高光），如果不希望系统添加特效，则可以用apple-touch-icon-precomposed.png代替apple-touch-icon.png</p>
<p>以下是针对ox不同设备，选择一个最优icon。默认iphone的大小为60px，ipad为76px，retina屏乘以2倍。</p>
<pre><code>&lt;link rel=&quot;apple-touch-icon&quot; href=&quot;touch-icon-iphone.png&quot;&gt;
&lt;link rel=&quot;apple-touch-icon&quot; sizes=&quot;76x76&quot; href=&quot;touch-icon-ipad.png&quot;&gt;
&lt;link rel=&quot;apple-touch-icon&quot; sizes=&quot;120x120&quot; href=&quot;touch-icon-iphone-retina.png&quot;&gt;
&lt;link rel=&quot;apple-touch-icon&quot; sizes=&quot;152x152&quot; href=&quot;touch-icon-ipad-retina.png&quot;&gt;
</code></pre>
<p>注：ios7不再为icon添加特效，ios7以前则默认为icon添加特效，除非icon有关键字-precomposed.png为后缀。</p>
<p>同样基于apple-mobile-web-app-capable设置为yes，还可以用WebApp设置一个类似NativeApp的启动画面。</p>
<pre><code>&lt;link rel=&quot;apple-touch-startup-image&quot; href=&quot;/startup.png&quot;&gt;
</code></pre>
<p>和apple-touch-icon不同，apple-mobile-web-app-capable不支持sizes属性，所以使用media来控制retina和横竖屏加载不同的启动画面。</p>
<pre><code>// iPhone
&lt;link href=&quot;apple-touch-startup-image-320x460.png&quot; media=&quot;(device-width: 320px)&quot; rel=&quot;apple-touch-startup-image&quot; /&gt;

// iPhone Retina
&lt;link href=&quot;apple-touch-startup-image-640x920.png&quot; media=&quot;(device-width: 320px) and (-webkit-device-pixel-ratio: 2)&quot; rel=&quot;apple-touch-startup-image&quot; /&gt;

// iPhone 5
&lt;link rel=&quot;apple-touch-startup-image&quot; media=&quot;(device-width: 320px) and (device-height: 568px) and (-webkit-device-pixel-ratio: 2)&quot; href=&quot;apple-touch-startup-image-640x1096.png&quot;&gt;

// iPad portrait
&lt;link href=&quot;apple-touch-startup-image-768x1004.png&quot; media=&quot;(device-width: 768px) and (orientation: portrait)&quot; rel=&quot;apple-touch-startup-image&quot; /&gt;

// iPad landscape
&lt;link href=&quot;apple-touch-startup-image-748x1024.png&quot; media=&quot;(device-width: 768px) and (orientation: landscape)&quot; rel=&quot;apple-touch-startup-image&quot; /&gt;

// iPad Retina portrait
&lt;link href=&quot;apple-touch-startup-image-1536x2008.png&quot; media=&quot;(device-width: 1536px) and (orientation: portrait) and (-webkit-device-pixel-ratio: 2)&quot; rel=&quot;apple-touch-startup-image&quot; /&gt;

// iPad Retina landscape
&lt;link href=&quot;apple-touch-startup-image-1496x2048.png&quot;media=&quot;(device-width: 1536px)  and (orientation: landscape) and (-webkit-device-pixel-ratio: 2)&quot;rel=&quot;apple-touch-startup-image&quot; /&gt;
</code></pre>
<p>⑤ robots -- 搜索引擎爬虫的索引方式</p>
<p>用来告诉爬虫哪些页面需要索引，哪些页面不需要索引，例如</p>
<pre><code>&lt;meta name=&quot;robots&quot; content=&quot;none&quot;&gt;
</code></pre>
<p>具体参数如下：</p>
<ol>
<li>none : 搜索引擎将忽略此网页，等价于noindex，nofollow。</li>
<li>noindex : 搜索引擎不索引此网页。</li>
<li>nofollow: 搜索引擎不继续通过此网页的链接索引搜索其它的网页。</li>
<li>all : 搜索引擎将索引此网页与继续通过此网页的链接索引，等价于index，follow。（默认是all）</li>
<li>index : 搜索引擎索引此网页。</li>
<li>follow : 搜索引擎继续通过此网页的链接索引搜索其它的网页。</li>
</ol>
<p>⑥ format-detection -- 页面特定元素高亮处理</p>
<p>用来处理页面上特定元素的高亮</p>
<pre><code>&lt;meta name=&quot;format-detection&quot; content=&quot;telephone=no&quot; /&gt;
&lt;meta name=&quot;format-detection&quot; content=&quot;email=no&quot; /&gt;
&lt;meta name=&quot;format-detection&quot; content=&quot;address=no&quot; /&gt;
</code></pre>
<p>我们常用的微信自带浏览器上就会把页面上的一串数字自动高亮成蓝色，让整个页面美感很差，所以我们可以使用第一条禁用掉电话识别（同理下面两条为『邮件地址』和『跳转地图』（这个不太理解）</p>
<p>⑦ revisit-after -- 爬虫重访时间</p>
<pre><code>&lt;meta name=&quot;revisit-after&quot; content=&quot;5 days&quot; &gt;
</code></pre>
<p>这里设置为5天防止爬虫给服务器造成特别大压力（反正这个blog更新很慢）</p>
<p>二 meta的http-equiv属性</p>
<p>此处主要参考了<a href="http://lxxyx.win/2016/01/09/%E5%AF%92%E5%81%87%E5%89%8D%E7%AB%AF%E5%AD%A6%E4%B9%A0(1)%E2%80%94HTML%20meta%E6%A0%87%E7%AD%BE/">Lxxyx的blog</a></p>
<p>http-equiv顾名思义，相当于http的文件头作用。用来定义http参数等，正常的格式为：</p>
<pre><code>&lt;meta http-equiv=&quot;参数&quot; content=&quot;具体的描述&quot;&gt;
</code></pre>
<p>常用设置有如下用法</p>
<p>① X-UA-Compatible -- 浏览器采取何种版本渲染当前页面</p>
<pre><code>&lt;meta http-equiv=&quot;X-UA-Compatible&quot; content=&quot;IE=edge,chrome=1&quot;/&gt; //指定IE和Chrome使用最新版本渲染当前页面
</code></pre>
<p>这里用于告知浏览器以何种版本来渲染页面（一般是最新版</p>
<p>② cache-control -- 指定请求和响应遵循的缓存机制</p>
<p>指导浏览器如何缓存某个响应以及缓存多长时间，图解如下</p>
<p><img src="/images/2017/01/907133393-56ffa468cc654_articlex.png" alt="" loading="lazy"></p>
<pre><code>&lt;meta http-equiv=&quot;cache-control&quot; content=&quot;no-cache&quot;&gt;
</code></pre>
<p>共有以下几种用法：</p>
<ol>
<li>no-cache: 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。</li>
<li>no-store: 不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）</li>
<li>public : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果</li>
<li>private : 只为单个用户缓存，因此不允许任何中继进行缓存。（比如说CDN就不允许缓存private的响应）</li>
<li>maxage : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。</li>
</ol>
<p>如果为了防止被百度的MDZZ转码技术搞坏页面，还可以设置一条</p>
<pre><code>&lt;meta http-equiv=&quot;Cache-Control&quot; content=&quot;no-siteapp&quot; /&gt;
</code></pre>
<p>③ expires -- 网页到期时间<br>
用于设定网页的到期时间，过期后网页必须到服务器上重新传输。举例：</p>
<pre><code>&lt;meta http-equiv=&quot;expires&quot; content=&quot;Sunday 26 October 2017 01:00 GMT&quot; /&gt;
</code></pre>
<p>④ refresh -- 自动刷新并指向某页面</p>
<p>这个这个功能非常常用，一般可以用来做页面统计，网页将在设定的时间内，自动刷新并调向设定的网址。举例:</p>
<pre><code>&lt;meta http-equiv=&quot;refresh&quot; content=&quot;1；URL=http://pengfei.ga/&quot;&gt; //意思是1秒后跳转向首页
</code></pre>
<!--kg-card-end: markdown-->
    