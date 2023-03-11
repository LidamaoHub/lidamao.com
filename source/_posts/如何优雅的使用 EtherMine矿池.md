
---
title: 如何优雅的使用 EtherMine矿池
banner: https://images.unsplash.com/photo-1563986768711-b3bde3dc821e?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDI3fHxldGhlcmV1bXxlbnwwfHx8fDE2MzQ5NjIxNDc&ixlib=rb-1.2.1&q=80&w=1080
cateogries: 
date: 2021-10-23 04:09:25
---
<!--kg-card-begin: markdown--><p>最近随着大量矿池出海,有很多朋友迁移到了 ethermine 矿池,随即遇到了很多问题,今天我来介绍下如何优雅的解决他们</p>
<p>Q1:如何绑定钱包匿名挖矿<br>
E 池由于是完全匿名挖矿(不同于星火需要注册),所以你需要证明你有提币地址的绝对控制权,所以此时不建议把收款地址设为交易所,而是使用 metamask 来管理一个热钱包账号</p>
<p>首先,下载 metamask(小狐狸)<br>
<img src="/images/2021/10/metamask.png" alt="metamask" loading="lazy"><br>
网址是<a href="https://metamask.io/download.html">metamask</a></p>
<ol>
<li>建议在桌面环境下载</li>
<li>下载需要翻墙(如果你不会翻墙建议就不用往下看了)</li>
</ol>
<blockquote>
<p>下载好后按照引导创建新账号,此时注意,钱包提供的一串12个单词的助记词,是你所有资产的核心,一定要认真的用笔抄写在确定安全的本子上,这串助记词丢失意味着你所有资产都会丢失,期间不能使用任何电子产品拍照或者复制</p>
</blockquote>
<p>配置好后,点击浏览器的插件栏小狐狸图标,就有如下界面<br>
<img src="/images/2021/10/wallet1.png" alt="wallet1" loading="lazy"></p>
<blockquote>
<p>点击复制你的地址,并将这个地址使用到所有的矿机上</p>
</blockquote>
<p>接下来打开 <a href="https://ethermine.org/miners/%E4%BD%A0%E7%9A%84%E5%9C%B0%E5%9D%80">https://ethermine.org/miners/你的地址</a> 等待算力上来,成功匿名挖矿</p>
<p>Q2:如何设置提币数量<br>
<img src="/images/2021/10/e1.png" alt="e1" loading="lazy"><br>
如上图所示,默认情况下 E 池提现基准是1ETH(我这里改成了0.3ETH),如果你的算力比较小,又想直接走主网的话,这是需要点击<code>Gas Price Limit ...</code>那行话来进行设置</p>
<p>首先确定右上角小狐狸是否绑定成功,且当前小狐狸账号是否和挖矿地址相同</p>
<p><img src="/images/2021/10/set.png" alt="set" loading="lazy"></p>
<p>填写第一行起提金额,第二行容忍多高 gas</p>
<p>第二行为 GasPrice ,目前主网平均 price 为80左右,如果不是很着急的情况建议设置80,否则可以设的更高(设更高也会按照当前主网价格转账).</p>
<p>这个值越高,转账费用越高,一般 eth 主网的转账价格为8刀左右</p>
<p>此时点击<code>Sign</code>通过你的小狐狸钱包签名(证明自己是当前账户的实际控制人,这个操作不花钱)即可保存</p>
<p>Q3:如何使用更便宜的 L2方案 Polygon<br>
如上上图所示,我们可以看到提币方案不仅有 MainNet,还有 L2</p>
<p>这里解释下,L2相当于 mainnet 的一个侧链分支,使用这个侧链手续费几乎可以忽略不计,这样无论多少次转账都不会花费 MainNet 那样高昂的手续费</p>
<p>设置 L2并签名后,你的 ETH 将会以 <code>WETH</code> 的形式发放在Polygon链上(添加 polygon 链请点击<code>switch metamask to the polygon network</code></p>
<p>此时小狐狸顶部网络选择框就有了 polygon 主网的选项了,之后切换不同的网络可以通过这个选择器</p>
<p>接下来需要点击<code>add the weth token</code>,将 weth 添加到你的钱包里(然后才可以看到余额)</p>
<p>Q4:如何将 polygon 上的资产转到交易所?</p>
<p>如果你所是用的交易所支持 polygon 转账 eth,那你可以直接转账</p>
<p>如果不支持,那么可以先在链上将 weth 卖成 usdt,再将 usdt 跨链到 heco/bsc 等主流低手续费链上兵法送到交易所</p>
<p>链上交易weth 可以使用寿司🍣 SWAP(记得切换到 polygon 链)</p>
<p><a href="https://app.sushi.com/zh_CN/swap">https://app.sushi.com/zh_CN/swap</a></p>
<p><img src="/images/2021/10/sushi.png" alt="sushi" loading="lazy"></p>
<p>交易好的 USDT 可以在 anyswap 进行跨链操作</p>
<p><a href="https://anyswap.exchange/#/router">https://anyswap.exchange/#/router</a></p>
<p><img src="/images/2021/10/any.png" alt="any" loading="lazy"></p>
<p>比如此处是将 polygon 上的 usdt 资产转移到 bsc 上</p>
<p>交易结束后切换小狐狸到BSC 上即可看到跨链好的 usdt</p>
<p>本篇结束</p>
<!--kg-card-end: markdown-->
    