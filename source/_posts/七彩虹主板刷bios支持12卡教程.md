
---
title: 七彩虹主板刷bios支持12卡教程
banner: https://images.unsplash.com/photo-1540829917886-91ab031b1764?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDN8fG1vdGhlcmJvYXJkfGVufDB8fHx8MTYxNTc3NDUzOQ&ixlib=rb-1.2.1&q=80&w=1080
cateogries: 
---
<!--kg-card-begin: markdown--><p>最近七彩虹的12卡矿板 B250A-BTC 很火,因为价格便宜也比较好用(比那该死的映泰好用),但是板子初始设置最多3张显卡.需要自己更新bios设置才可以支持12路显卡</p>
<p>所以以下教大家如何手动刷七彩虹主板的bios</p>
<p>首先打开七彩虹的官网  <code>colorful.cn</code>  选择 <code>产品中心</code><br>
<img src="/images/2021/03/341615743639_.pic.jpg" alt="341615743639_.pic" loading="lazy"></p>
<p>然后选择Intel芯片组-&gt;B250,选到后找到自己板子的型号(所有型号都可以按这个操作来找)</p>
<p>点击进去后选择<code>技术支持</code>并选到BIOS<br>
<img src="/images/2021/03/351615743639_.pic.jpg" alt="351615743639_.pic" loading="lazy"><br>
我这里选择的是1003版本,如果你的cpu比较高端,可以选择1004版本</p>
<p>下载biso文件并解压到电脑里,这里我方便找到所以解压到了<strong>D盘</strong>根目录下</p>
<p>接下来,在开始菜单里找到PowerShell,<span style='color:#FF2F2E'>右键</a>并选择以管理员身份打开</p>
<p><img src="/images/2021/03/361615743639_.pic.jpg" alt="361615743639_.pic" loading="lazy"></p>
<p>PowerShell相当于win下面的一个命令行软件,接下来按照步骤操作<br>
<img src="/images/2021/03/371615743639_.pic.jpg" alt="371615743639_.pic" loading="lazy"></p>
<ol>
<li>首先输入 <code>D:</code> ,意味着跳转到D盘</li>
<li>输入cd进入1004解压后的windos64文件夹(如果你是整个文件夹拷贝进来的,那么输入 <code>cd .\1004\windos64</code> )</li>
<li>执行如下命令</li>
</ol>
<pre><code>.\FPTW64.exe /F .\xxx.bin /bios
</code></pre>
<p>这里xxx.bin 指文件夹里bin的文件名,我这里是25ABTC24所以我是25ABTC24.bin</p>
<p>懒人技巧: 输入文件开头(比如<code>25</code>)然后按tab,会自动根据当前文件夹内容进行补全,如果一次不中再按几下直到选到想要的文件</p>
<p>接来下按回车执行这个命令,界面会开始执行,到100%的时候重启系统即可</p>
<p>感谢阅读</p>
<!--kg-card-end: markdown-->
    