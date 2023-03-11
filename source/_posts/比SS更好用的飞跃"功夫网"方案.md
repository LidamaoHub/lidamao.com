
---
title: 比SS更好用的飞跃"功夫网"方案
banner: /images/2017/09/jeremy-bishop-340107.jpg
cateogries: 
---
<!--kg-card-begin: markdown--><h5 id="">特别注明:</h5>
<blockquote>
<p>本篇内容并非原创,完全抄袭于互联网上公开资料</p>
</blockquote>
<blockquote>
<p>本博不提供任何有偿部署服务</p>
</blockquote>
<blockquote>
<p>写文是博主喝多了随便抹键盘产生的,不代表任何政治观点</p>
</blockquote>
<p>很久之前博主都是用&quot;轻云&quot;提供的PAC文件进行代理科学上网的(很不幸后面轻云倒了)</p>
<p>只要将<code>http://qingyun.com/xxx.pac</code>复制进iphone手机中,即可直接访问外网,优点如下</p>
<ol>
<li>不需要下载任何其他软件,比SS等需要软件支持的方式好得多.</li>
<li>自动判断是否科学上网,可以一边听着虾米音乐一边看谷歌搜索</li>
<li>因为可以智能分辨网址,所以流量耗损少</li>
<li>速度贼快</li>
<li>跨平台使用(除了安卓不太方便</li>
<li>一次配置终身使用</li>
</ol>
<p>可惜之后轻云面对审查而倒闭了,我猜创始人可能是低调的技术男,最后我也没找到他们的方案,很可惜没有直接充值十年</p>
<p>最近终于摸索出他们的方式,应该是使用<code>Squid</code>进行代理科学上网.按照网上查到的方案,部署方式如下</p>
<p>首先你先去阿里云买一台不在大陆的服务器,这里推荐香港的</p>
<h4 id="ubuntu">Ubuntu方案</h4>
<p>首先需要安装<code>Squid</code></p>
<pre><code class="language-bash">apt-get update	
apt-get install squid
</code></pre>
<p>备份并改写<code>Squid</code>的设置</p>
<pre><code class="language-bash">mv /etc/squid3/squid.conf /etc/squid3/squid.conf.temp
vi /etc/squid3/squid.conf
</code></pre>
<p>写入以下内容：</p>
<pre><code class="language-bash">acl SSL_ports port 443
acl Safe_ports port 80      # http
acl Safe_ports port 21      # ftp
acl Safe_ports port 443     # https
acl Safe_ports port 70      # gopher
acl Safe_ports port 210     # wais
acl Safe_ports port 1025-65535  # unregistered ports
acl Safe_ports port 280     # http-mgmt
acl Safe_ports port 488     # gss-http
acl Safe_ports port 591     # filemaker
acl Safe_ports port 777     # multiling http
acl CONNECT method CONNECT
http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow all 
http_port 2333 #这里写入你要使用的端口,注意本条注释要删掉哦
coredump_dir /var/spool/squid3
refresh_pattern ^ftp:       1440    20% 10080
refresh_pattern ^gopher:    1440    0%  1440
refresh_pattern -i (/cgi-bin/|\?) 0 0%  0
refresh_pattern (Release|Packages(.gz)*)$      0       20%     2880
refresh_pattern .       0   20% 4320
strip_query_terms off
</code></pre>
<p>重启Squid</p>
<pre><code>service squid3 restart
</code></pre>
<p>接下来配置你自己的pac文件,这里有一个方便的小工具<code>opengfw</code></p>
<pre><code class="language-bash">git clone https://github.com/OpenGFW/OpenGFW.git
cd OpenGFW
pip install requests
python OpenGFW.py PROXY 你服务器IP 你服务器端口
</code></pre>
<p>然后把生成的OpenGFW.pac上传到你的服务器上<br>
推荐用Nginx,则上传到</p>
<pre><code class="language-bash">/usr/share/html/
</code></pre>
<p>最终你的pac地址为</p>
<pre><code>http://你的ip/OpenGFW.pac
</code></pre>
<p>然后你会惊喜的发现:</p>
<p>哈哈哈哈哈哈发现完全没法用对吧!!!!<br>
<img src="/images/2017/09/814268e3ly1fidgwagyhej206y05r3yh.jpg" alt="" loading="lazy"><br>
因为你国<code>功夫网</code>牛逼啊,光有<code>Squid</code>仍然是无法科学上网的.因为<code>Squid</code>并不会把客户端与服务器间的流量加密，你和服务端通信都是穿墙的,所以你访问谷歌等非法网站的请求在发给墙外香港服务器的时后就被<code>gfw</code>这个猥琐比偷窥到了<br>
<img src="/images/2017/09/1467265003607015.jpg" alt="" loading="lazy"><br>
就算你用https都不行,因为在通过代理与谷歌建立<code>https</code>连接最初都是用明文的<code>http</code>包,进行密钥的交换.</p>
<p>而之后即使在<code>TLS</code>信道建立完毕后,由于通过了一层代理,给代理发送的<code>http</code>头仍然是明文的</p>
<p>虽然在这层<code>http</code>中包裹了真实的加密的传向谷歌的数据,但是<code>http</code>头的 [Proxy-Connect-Hostname]这个域出卖了真实的链接地址,所以也会被墙。</p>
<h4 id="">见招拆招</h4>
<p>我们使用<code>Stunnel</code>对所有访问数据进行加密</p>
<p>我们这里使用一台国内服务器进行中继,刚才不行的原理以及中继的原理如下图<br>
<img src="/images/2017/09/-----.png" alt="" loading="lazy"><br>
而且有了中继服务器和国外服务器通信,理论上比原来直连国外服务器速度更快(虽然加密一大堆,但是比不上线路好)</p>
<h4 id="">国外服务器配置</h4>
<p>安装Stunnel</p>
<pre><code class="language-bash">apt-get install stunnel4
</code></pre>
<p>然后创建一个1000天的证书,一路回车即可</p>
<pre><code class="language-bash">openssl req -new -x509 -days 1000 -nodes -out /etc/stunnel/stunnel.pem -keyout /etc/stunnel/stunnel.pem
</code></pre>
<p>然后设置开机自动启动</p>
<pre><code>vi /etc/default/stunnel4
</code></pre>
<p>并把ENABLED 设为1,这样既可开机启动,国内服务器也要这样设置一下下</p>
<p>接下来配置stynnel</p>
<pre><code>vi /etc/stunnel/stunnel.conf 
</code></pre>
<p>粘贴如下内容</p>
<pre><code>socket = l:TCP_NODELAY=1
socket = r:TCP_NODELAY=1

pid = /var/lib/stunnel4/stunnel.pid
verify = 3
cert = /etc/stunnel/stunnel.pem
CAfile =/etc/stunnel/stunnel.pem
setuid = root
setgid = root
client=no
delay = no
sslVersion = TLSv1
debug = 7
syslog = yes
output = stunnel.log

[squid]
accept = 12345  #这里端口号也要记住,在中继服务器上要用(本行也要删掉哦
connect = 127.0.0.1:2333   #这里只要改你刚才设置的端口号即可,我们刚才是2333(注意本行要删掉
</code></pre>
<p>然后重启stunnel</p>
<pre><code class="language-bash">service stunnel4 restart
</code></pre>
<p>显示如下即可配置国内服务辣</p>
<pre><code>Restarting SSL tunnels: [stopped:/etc/stunnel/stunnel.conf] [Started:/etc/stunnel/stunnel.conf] stunnel.
</code></pre>
<h4 id="">国内中继服务器</h4>
<blockquote>
<p>国内中继服务器可以买个阿里云华南节点,建议带宽选择时选择按流量付费,这样包月便宜而且不走流量就不花钱</p>
</blockquote>
<blockquote>
<p>国内服务器其实只要配置一个解密客户端(其实如果你不嫌麻烦,本地配置一个也成,不过....太烦啦</p>
</blockquote>
<p>按照刚才的方式再配置一遍,唯一不同的地方是<code>/etc/stunnel/stunnel.conf </code>文件不同,如下</p>
<pre><code>cert = /etc/stunnel/stunnel.pem
socket = l:TCP_NODELAY=1
socket = r:TCP_NODELAY=1
verify = 2
CAfile = /etc/stunnel/stunnel.pem
client=yes
ciphers = AES256-SHA
delay = no
failover = prio
sslVersion = TLSv1
[sproxy]
accept  = 8889 #记住这串,用力最后配置使用
connect = 国外IP:12345
</code></pre>
<p>然后一个保存重启即可</p>
<pre><code>service stunnel4 restart
</code></pre>
<p>此时,你就可以利用刚才的OpenGFW.py来再次生成一个pac文件并放在国内服务器的Nginx上面啦</p>
<p>也就是说,你最后生成一个网址</p>
<pre><code>http://你的国内IP/OpenGFW.pac
</code></pre>
<h4 id="">如何使用</h4>
<p>这里列举几个主流客户端使用方案</p>
<h6 id="macbook">MacBook</h6>
<p><img src="/images/2017/09/-----2017-09-23---7-30-33.png" alt="" loading="lazy"><br>
打开「系统偏好设置」－「网络设置」<br>
<img src="/images/2017/09/-----2017-09-23---7-30-59.png" alt="" loading="lazy"><br>
进入「高级设置」<br>
<img src="/images/2017/09/-----2017-09-23---7-31-29.png" alt="" loading="lazy"></p>
<p>选择倒数第二个选项卡「代理」，勾选「自动代理配置」，然后将你的地址输入其中</p>
<p>一路保存即可</p>
<h6 id="iphone">iPhone</h6>
<blockquote>
<p>当你切换至一个全新的 Wi-Fi 环境时，需重新按以下方法设置。</p>
</blockquote>
<blockquote>
<p>而对于同一个 Wi-Fi（比如家里或者公司），你重新连上时，iOS 系统会记住你之前的设置，所以无需重复操作。</p>
</blockquote>
<p><img src="/images/2017/09/-----2017-09-23---7-35-25.png" alt="" loading="lazy"><br>
进入「设置」，然后点击「Wi-Fi」<br>
<img src="/images/2017/09/-----2017-09-23---7-36-06.png" alt="" loading="lazy"><br>
找到你使用的无线网络，点击该无线网络最右侧蓝色  进入详细设置<br>
<img src="/images/2017/09/-----2017-09-23---7-36-13.png" alt="" loading="lazy"><br>
在页面底部，找到「HTTP 代理」，选择「自动」标签，并复制你的连接进去然后左滑退出</p>
<p>大功告成</p>
<h6 id="windows">Windows</h6>
<blockquote>
<p>要求 Windows 版本高于 XP。Windows 7、8 和 10 采用不同的配置方法，请注意区分。<br>
#######win7&amp;8<br>
打开「控制面板」并选择「网络和 Internet」：<br>
<img src="/images/2017/09/-----2017-09-23---7-39-01.png" alt="" loading="lazy"><br>
点击「Internet 选项」，并切换到「连接」选项卡</p>
</blockquote>
<p>如果你用路由器，或者你的电脑通过局域网上网，选择「局域网设置」</p>
<p>如果你直接用电脑拨号连接，点击「拨号连接」旁边的「设置」</p>
<p>勾选「使用自动配置脚本」，并将你的地址粘贴进地址栏中</p>
<ol>
<li>使用路由器，或者你的电脑通过局域网上网：</li>
</ol>
<p><img src="/images/2017/09/-----2017-09-23---7-40-17.png" alt="" loading="lazy"><br>
2. 直接用电脑拨号连接：<br>
<img src="/images/2017/09/-----2017-09-23---7-40-33.png" alt="" loading="lazy"><br>
点击「确认」，再选择「应用」，完成设置。</p>
<p>#######Win10<br>
点击「开始」进入「设置」：<br>
<img src="/images/2017/09/-----2017-09-23---7-41-53.png" alt="" loading="lazy"></p>
<p>点击设置中的「网络 和 Internet」：<br>
<img src="/images/2017/09/-----2017-09-23---7-42-04.png" alt="" loading="lazy"></p>
<p>配置代理地址：</p>
<p>如果你使用路由器，或者你的电脑用过局域网连接，请打开「代理」选项卡，开启「使用安装程序脚本」，并在「脚本地址」中输入你的地址，然后点击「保存」：<br>
<img src="/images/2017/09/-----2017-09-23---7-43-22.png" alt="" loading="lazy"><br>
如果使用系统拨号直连的方式接入网络，请在完成上面 3 - a 步骤后进入「拨号」选项卡，选中您所使用的拨号并点击「高级选项」，在「自动配置」中打开「使用安装程序脚本」并输入你的地址，并保存<br>
<img src="/images/2017/09/-----2017-09-23---7-43-35.png" alt="" loading="lazy"></p>
<p>点击保存,大功告成</p>
<h4 id="">结语</h4>
<p>本文内容纯属抄袭,具体文献内容来源如下</p>
<p><a href="https://timonpeng.com/2015/09/17/%E8%87%AA%E5%BB%BAPac%E4%BB%A3%E7%90%86%E7%BF%BB%E5%A2%99%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%95%99%E7%A8%8B.html">配置Squid</a></p>
<p><a href="http://www.5941740.cn/2015/07/17/%E4%BD%BF%E7%94%A8squid-stunnel%E6%90%AD%E5%BB%BA%E8%87%AA%E5%B7%B1%E7%9A%84%E7%BF%BB%E5%A2%99%E6%9C%8D%E5%8A%A1%E5%99%A8/" >配置CentOS上stunnel</a></p>
<p><a href="https://duotai.love/manual/windows">如何在windows上使用地址</a></p>
<h4 id="">纪念轻云</h4>
<!--kg-card-end: markdown-->
    