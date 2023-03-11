
---
title: 让你的服务器免输入密码(SSH-KEY连接)
banner: /images/2015/12/tumblr_nv26r7rANX1u60tx6o1_1280.jpg
cateogries: 
date: 2015-12-22 05:57:56
---
<!--kg-card-begin: markdown--><p>常常使用ssh登陆远程服务器的朋友们可能常常有这样的烦恼</p>
<p><strong>我们需要在登陆的时候输入密码</strong></p>
<p><img src="/images/2015/12/-----2015-07-17---2-49-22.png" alt="" loading="lazy"><br>
闪动的光标,没法看到的输入内容,过时不候的Timeout等等不友好的交互对于一个急性子懒人来说简直是烦到骨头痒痒,常常让人怀疑这台远程服务器是不是自己亲生的(显然不是)</p>
<p>当然这也有一个解决办法,就是通过使用SSHkey来实现的免密码登陆</p>
<p>接下来你需要准备</p>
<ul>
<li>一台本地电脑(就是你目前手里这台)</li>
<li>一台远程服务器</li>
</ul>
<p>接下来需要先得到本机公钥,一般情况下电脑本机是自带ssh的,你可以在命令行工具中输入<code>ssh -V</code>如果显示了版本号那说明ssh存在.如果本机不存在你可以尝试着<code>brew install ssh</code>(mac) 或者<code>sudo apt-get install ssh</code>(ubuntu)其他系统请百度一下~</p>
<p>如果上一步发现本机存在ssh的话,那么接下来查看一下我们是否创建了公钥,在根目录中<code>cd .ssh</code>查看是否有ssh的隐藏文件夹并且查看文件夹内容</p>
<p>(由于我已经创建过我的公钥,所以完成所有结果后是这样的)<br>
<img src="/images/2015/12/-----2015-07-19---10-48-51.png" alt="" loading="lazy"></p>
<p>如果你的ssh文件夹不存在就请在根目录下<code>mkdir .ssh</code>创建一个.ssh文件夹</p>
<p>然后输入<code>ssh-keygen</code>命令,系统会提示你rsa文件存储目录,直接回车确认</p>
<p>系统会提示要求你输入两次密码(每当你需要使用你的公钥时都需要输入这个密码),建议直接留空回车</p>
<p>如果一切顺利,此时你的.ssh文件夹中应该已经存在xxx_rsa和xxx_rsa.pub两个文件了,.pub文件顾名思义就是我们所要的公钥文件</p>
<p><strong>接下来同样的步骤检测远程服务端是否存在ssh程序</strong></p>
<p>如果存在的话暂且不用生成公钥等操作,直接在.ssh文件夹中<code>vi authorized_keys</code>并写入本机xxx_rsa.pub中的内容(把本级公钥内容复制到远程服务器上)</p>
<p>一般的公钥内容类似</p>
<pre><code>ssh-rsa hhhhhhhhhhhhhhhhhcaibugaosunigongyuehhhhhhhhhhmsHOSpWeqns7o3tle0Ln1GMmPpdFph/owa7vj5/JYSOCBX8c+gGFyJeAMHGTs1fnHhGZRl5mzu8mWIv+qJnDxRmE/jBtuNXzSrPeZ2Cz86U+DfWtXVRyEl9XoIotX+GZ/zPxvPoMoItWD3UL6aA8McCX/PE7BLFA4B1Nl+mefTVpHH39AqcyqkcAJxntoqeNU3IwaM7sx/J7ONrFxp9Z3fjVR root@lee-perfect
</code></pre>
<p>这种格式</p>
<p>保存文件后执行<code>chmod 600 authorized_keys</code>来提升这个文件的权限</p>
<p>完成一切后你可以试着在本机登录服务器,如果一切顺利,你将会怀念输入密码的日子</p>
<p>还没完....</p>
<p>如果想进一步偷懒(比如登陆远程服务器仍现需要输入一大串字符类似我的<code>ssh root@xxx.xxx.xxx.xxx</code>),你还可以尝试使用快捷操作</p>
<p>在你的终端中输入<code>vi ~/.bashrc</code>此时因为我使用zsh(绝对神器建议安装<code>brew install zsh</code>)所以我输入<code>vi ~/.zshrc</code></p>
<p>这里打开了zsh的配置页面(同理bash的配置页面)</p>
<p><img src="/images/2015/12/-----2015-07-19---11-11-26.png" alt="" loading="lazy"></p>
<p>在最后一行加入<code>alias myubuntu=&quot;ssh xx.xx.xxx.xxx&quot;</code>并保存文件(其中myubuntu即为你的快捷命令,可以自定义.特别注意快捷命令和=之间没有空格)</p>
<p>然后推出编辑器并执行命令<code>. ~/.bashrc</code>(我的是zshrc)</p>
<p>终于大功告成,下次登录远程服务器只需要输入&quot;myubuntu&quot;即可直接登陆</p>
<p><img src="/images/2015/12/20150420155818068-1-2.jpg" alt="" loading="lazy"></p>
<!--kg-card-end: markdown-->
    