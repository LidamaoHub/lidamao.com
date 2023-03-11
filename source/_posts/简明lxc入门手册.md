
---
title: 简明lxc入门手册
banner: /images/2015/12/fc6c8ec317f9a609f0f2262a56b3ac37_b.jpg
cateogries: 
date: 2015-12-25 05:30:42
---
<!--kg-card-begin: markdown--><p>最新常常尝试新的软件导致我的云服务器(Ubuntu)被装的乱七八糟不得不没事干重装,所以考虑尝试使用lxc提供的虚拟化服务来make things clean and clear</p>
<h5 id="lxc">lxc的安装</h5>
<p>在Ubuntu环境下可以直接通过apt-get来安装</p>
<pre><code class="language-bash">$ sudo apt-get install lxc
</code></pre>
<p>接下来检查一下当前环境下内核是否支持lxc</p>
<pre><code class="language-bash">$ lxc-checkconfig 
</code></pre>
<p>一般情况下会显示一列enable(如下图)</p>
<p><img src="/images/2015/12/-----2015-12-24---3-02-21.png" alt="" loading="lazy"></p>
<p>说明lxc符合你的环境,可以接下来进一步操作</p>
<h5 id="">创建新的容器</h5>
<p>创建好lxc环境后我们就可以开始使用它了,第一步是新建一个新的容器</p>
<pre><code class="language-bash">$ sudo lxc-create -n &lt;你的新容器名字&gt; -t ubuntu 
</code></pre>
<p>如你所见上面那个步骤是新建一个ubuntu系统的虚拟容器,</p>
<hr>
<p>特别注意,由于国内特殊的网络环境,以上的创建步骤很可能无法成功,这是因为创建过程中需要被下载的ubuntu镜像被墙了.</p>
<p>你需要修改一下lxc的默认ubuntu镜像下载地址来搞定</p>
<pre><code class="language-bash">$ vi /etc/default/lxc
</code></pre>
<p>我们这里使用网易的ubuntu镜像地址,在这个配置文件中粘贴</p>
<pre><code class="language-bash">MIRROR=&quot;http://mirrors.163.com/ubuntu/&quot;
</code></pre>
<p>然后重试上面步骤一般即可成功创建</p>
<hr>
<p>当然你也可以创建一下其他的环境,lxc是通过脚本模板的方式来提供这部分的操作</p>
<pre><code class="language-bash">$ cd /usr/share/lxc/templates
$ ls  
</code></pre>
<p>会显示如下<br>
<img src="/images/2015/12/-----2015-12-24---3-07-46.png" alt="" loading="lazy"></p>
<p>如你所见如果我们想安装一个centos的虚拟环境那直接把上一步命令中ubuntu环城centos即可</p>
<p>如果需要特定的系统版本号,则如下操作</p>
<pre><code class="language-bash">$ sudo lxc-create -n &lt;你的新容器名字&gt; -t ubuntu -- --release 版本号 
</code></pre>
<p>接下来就是漫长的等待(大约20分钟左右)会显示创建完成(如下),此时你的第一台容器完美诞生了~</p>
<pre><code>##
# The default user is 'ubuntu' with password 'ubuntu'!
# Use the 'sudo' command to run tasks as root in the container.
##
</code></pre>
<p>默认情况下容器被放到 /var/lib/lxc/&lt;容器名&gt; 这个目录下，容器的根文件系统放在 /var/lib/lxc/&lt;容器名&gt;/rootfs 目录下。</p>
<h5 id="lxc">lxc管理</h5>
<p>在安装好之后我们先查看一下状态,输入</p>
<pre><code class="language-bash">$ sudo lxc-ls --fancy 
</code></pre>
<p>会显示</p>
<pre><code>NAME  STATE    IPV4  IPV6  AUTOSTART  
------------------------------------
lee   STOPPED  -     -     NO         
</code></pre>
<p>则说明你已经创建好自己的容器,但是他并没有启动也没有分配IP,你可以输入</p>
<pre><code class="language-bash">$ sudo lxc-start -n &lt;容器名&gt; -d 
</code></pre>
<p>然后再次查看状态,会显示</p>
<pre><code>NAME  STATE    IPV4       IPV6  AUTOSTART  
-----------------------------------------
lee   RUNNING  10.0.3.34  -     NO         
</code></pre>
<p>显示成功启动而且分配了<code>10.0.3.34 </code>这个地址,我们可以像任何普通远程服务器那样去登陆他(当然只限你远程服务器本机登陆,因为是内网)</p>
<pre><code class="language-bash">$ sudo ssh ubuntu@10.0.3.34
password: ubuntu
</code></pre>
<p>随意折腾吧!</p>
<p>你也可以使用lxc命令停止和删除和克隆(需要先停止)容器</p>
<pre><code>$ sudo lxc-stop -n &lt;container-name&gt;
$ sudo lxc-destroy -n &lt;container-name&gt; 
sudo lxc-clone -o &lt;container-name&gt; -n &lt;new-container-name&gt;
</code></pre>
<p><img src="/images/2015/12/20150420155818068-1-3.jpg" alt="" loading="lazy"></p>
<!--kg-card-end: markdown-->
    