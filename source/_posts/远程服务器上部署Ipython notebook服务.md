
---
title: 远程服务器上部署Ipython notebook服务
banner: /images/2015/12/DSC_0058_2500_c.JPG
cateogries: 
---
<!--kg-card-begin: markdown--><p>IPython + Notebook 是一款基于WEB的可视化的编程IDE，至于他的详细介绍，您可以看我另一篇文章（然而并没有写Orz）</p>
<p>那么我们可以在自己的本子上部署一个服务</p>
<p><code>sudo apt-get install ipython ipython-notebook -y</code>（这里按照Ubuntu来写）</p>
<p>然后输入<code>ipython notebook</code>即自动跳转浏览器打开<br>
<img src="/images/2015/12/-----2015-07-16---1-24-23.png" alt="" loading="lazy"><br>
偶尔需要写一些代码片段时简直不能更方便</p>
<p>但是如果我们想随时随地的用这项服务怎么办？</p>
<p>其实我们可以启用一个notebook的远程服务功能</p>
<p><code>ipython profile create nbserver</code></p>
<p>这样我们创建了一个配置文件夹</p>
<p>接下来我们进入配置文件夹</p>
<pre><code>cd .ipython/profile_nbserver/
vi ipython_notebook_config.py 
</code></pre>
<p>当然我的建议是把原来文件备份为ipython_notebook_config_temp.py文件</p>
<p>接下来在ipython_notebook_config.py文件中写入</p>
<pre><code>c = get_config()

c.NotebookApp.certfile = ‘’
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'sha1:bcd259ccf.[你自己的哈希字符串]'
c.NotebookApp.port = 9999
</code></pre>
<p>如你所见我们需要给这项服务加个登录密码（因为不加密码公开访问的话，坏人可以直接使用python调用系统接口来搞破坏）</p>
<p>我们可以输入</p>
<p><code>python -c &quot;import IPython;print IPython.lib.passwd()&quot;</code></p>
<p>这时会显示</p>
<pre><code>Enter password:
Verify password:
sha1:a83146285fe2:5288dfeb3a6af16992fadce... (安全原因略去)
</code></pre>
<p>其实就相当于一个sha1加密程序你把你想要的登录密码输入两次后得到的sha1字符串，写入刚才我们看到的“c.NotebookApp.password”中即可完成</p>
<p>好，接下来我们只要在shell中输入</p>
<p><code>ipython notebook --profile=nbserver</code></p>
<p>即可远程在这台服务器的9999端口访问ipython-notebook惹</p>
<p>比如http://pengfei.ga:9999</p>
<p><img src="/images/2015/12/20150420155818068-1.jpg" alt="" loading="lazy"></p>
<!--kg-card-end: markdown-->
    