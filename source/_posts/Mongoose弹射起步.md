
---
title: Mongoose弹射起步
banner: /images/2016/04/tumblr_nrv28s88PA1tdscmao1_1280.gif
cateogries: 
date: 2016-05-01 17:43:39
---
<!--kg-card-begin: markdown--><blockquote>
<p>弹射起步系列主要是讲解某些知识浅显的快速入门</p>
</blockquote>
<h5 id="">简介</h5>
<p>如果你看到这篇文章的话,说明你一定是正在寻找<strong>Node操作Mongodb</strong>的快速入门</p>
<p>那么废话不多说直入正文(本文大部分内容翻译自Mongoose官方文档,所以沿袭官方文档以一个blog作为例子)</p>
<h5 id="">安装</h5>
<p>首先你需要有一台完整的安装了<code>Node.js</code>和<code>npm</code>的电脑</p>
<p>然后<code>npm install mongoose</code></p>
<p>完成安装</p>
<h5 id="">基础知识</h5>
<h6 id="">连接</h6>
<p>首先你需要连接到本地服务器的Mongo数据库才能进行接下来的操作,如下</p>
<pre><code class="language-javascript">var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');
//由于是弹射起步,所以先简单粗暴的不加安全验证如此链接吧
</code></pre>
<p>一般情况下你需要创建一个**<code>Model</code>**来保存你即将存取的数据的结构,如下</p>
<pre><code>var cat_shema = new mongoose.Schema({
  name: String,
  friends: [String],
  age: Number,
}); 
//定义cat的Shema
var Cat = mongoose.model('Cat',cat_shema);
//将该Schema发布为Model
</code></pre>
<p>这里规定了<code>Model</code>的名称为Cat,那么对应的<code>collection</code>就是cats,并且规定了一些数据结构(一目了然)</p>
<h5 id="">增删改查</h5>
<p><strong>新增</strong><br>
创建一个kitty对象来赋值</p>
<pre><code>var Cat = mongoose.model('Cat');
var kitty = new Cat({ name: 'Zildjian', friends: ['tom', 'jerry']});
kitty.age = 3;
</code></pre>
<p>接下来调用save即可保存</p>
<pre><code>kitty.save(function (err) {
 console.log('save status:',err ? 'Angry!' : '吼啊!');
});
</code></pre>
<p><strong>查询</strong></p>
<pre><code>var Cat = mongoose.model('Cat');
//find参数：
//1.&lt;Object&gt;mongodb selector
//2.&lt;Function&gt;err:错误信息，results：查询结果
Cat.find({}, function (err,results) {
        if(err){
            console.log('error message',err);
            return;
        }
        console.log('results',results);
    });
//此处是不包含条件的基础查询
</code></pre>
<p>条件<code>or</code>查询</p>
<pre><code>var Cat = mongoose.model('Cat');
var condition ={
    $or: [
        {name: '华莱士'},
        {name: '麦克'}
    ]
};
Cat.find(condition, function (err, results) {
    if(err){
        console.log('condition error',err);
        return;
    }
    console.log(results);
});
</code></pre>
<p>这时返回值为</p>
<pre><code>[{name: '华莱士', friends: ['蛤', '董先森']}]
</code></pre>
<p>我们可以看出此处的查询语句为</p>
<pre><code> $or: [
        {name: '华莱士'},
        {name: '麦克'}
    ]
</code></pre>
<p>可以看出<code>mongoose</code>所使用的查询语句和mongoDb官方的查询语句很相似,此处可以套用</p>
<p><strong>更改</strong><br>
在我们或的查询结果后</p>
<pre><code>var Cat = mongoose.model('Cat');
var condition ={
    $or: [
        {name: '蛤蛤'}
    ]
};

Cat.findOne(condition, function (err, result) {
    if(err){
        console.log('condition error',err);
        return;
    }
//如果没有发生错误的话,此处会获得查询结果result
//我们将其参数改变
result.name = &quot;江主席&quot;
//然后保存
result.save(function(err){
    if(err){console.log(err)}
  })
});
//即可完成更新
</code></pre>
<p><strong>删除</strong></p>
<p>使用remove()方法即可删除查询结果</p>
<!--kg-card-end: markdown-->
    