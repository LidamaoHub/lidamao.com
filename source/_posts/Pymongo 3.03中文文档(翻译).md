
---
title: Pymongo 3.03中文文档(翻译)
banner: /images/2015/12/Istanbul--Turkey--1-.jpg
cateogries: 
date: 2015-12-22 05:58:18
---
<!--kg-card-begin: markdown--><h2 id="">前言</h2>
<p>这个导览将要告诉你如何使用pymongo这个模块来操纵MongoDB数据库</p>
<h4 id="">先决条件</h4>
<p>在开始前, 首先你需要安装pymongo模块.</p>
<p>一般情况下,你可以在终端中输入</p>
<pre><code>pip install pymongo
</code></pre>
<p>来轻松地安装(当然你也可以通过其他方式来安装)</p>
<p>(本导览是建立在你首先安装了python环境和mongo客户端前提上,如果你不确认你是否安安装前两者,请分别输入</p>
<pre><code>python -V
mongo -V
```来查看他们是否已经被正确的安装)

##正题
在你的py程序中,你首先需要

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>import pymongo<br>
from pymongo import MongoClient<br>
client = MongoClient()</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>

上面的代码会帮助您连接默认的host和端口,当然我们也可以自定义链接地址和端口如下

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>client = MongoClient('localhost', 27017)</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
这里是连接localhost的27017端口(mongo的默认端口)

或者使用MongoDb的Url格式:

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>client = MongoClient('mongodb://localhost:27017/')</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>

译者注:默认状态下Mongo客户端是没有加密的,但是如果你需要通过账户登录,你可以如下方式:

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>client = MongoClient('mongodb://账号:密码@localhost:27017/')</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
---


**选择数据库**

连接数据库非常简单,如果你需要连接&quot;test_datebase&quot;数据库,只需如下:

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>db = client.test_database</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
当然如果你想相连接一堆数据库,那么你也可以使用变量名来连接数据库,如下所示(和上面方式差不多):

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>db = client['test-database']</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
**选择集合**

集合是数据库documents的集合(有点类似于mysql里面的table),选择集合的方法其实和选择数据库的方式并无二致


</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>collection = db.test_collection</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
或者

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>collection = db['test-collection']</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
在MongoDb中有个特点,无论是数据库还是集合,都是半自动创建(这是译者造的词,不知道是否准确望斧正)

也就是说你并不需要类似mysql那样提前设计数据库和表

而是当你在数据库中**第一次写入数据**才会**根据路径判断**是否存在容纳这条数据的数据库和集合,如果没有,那么创建

---
####**文件**
在MongoDb里面,文件的存储方式很类似JSON格式(译者注:所以叫做BSON格式),如下

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>import datetime<br>
post = {&quot;author&quot;: &quot;鹏飞&quot;,<br>
...         &quot;text&quot;: &quot;欢迎来&quot;pengfei.ga&quot;学习&quot;,<br>
...         &quot;tags&quot;: [&quot;mongodb&quot;, &quot;python&quot;, &quot;pymongo&quot;],<br>
...         &quot;date&quot;: datetime.datetime.utcnow()}</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>
####**插入数据**

我们可以使用'insert_one()'命令来向集合中插入一条数据
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts = db.posts<br>
post_id = posts.insert_one(post).inserted_id<br>
post_id</p>
</blockquote>
</blockquote>
</blockquote>
<p>ObjectId('...')</p>
<pre><code>
如果一条数据被成功的插入到集合中,那么这条数据中会自动生成一个&quot;_id&quot;的key,这个key的值在一个集合中永远是独一无二的


(如上文所说的半自动创建方式,我们这里吧数据插入到并没有创建过的posts里面,再次查看,数据库里已经自动创建了posts这个集合)

我们可以这样来查看本数据库中的所有集合:

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>db.collection_names(include_system_collections=False)</p>
</blockquote>
</blockquote>
</blockquote>
<p>[u'posts']</p>
<pre><code>---
####**find_one()获取单条数据 **

我们可以使用find_one()这个函数来从MongoDb数据库中进行基础的query操作,这个函数返回一条符合**查询条件**的数据(当你知道你要查的数据只有一条或者你指向获得符合查询条件的第一条数据时,这个函数就很方便),如下:

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts.find_one()  #注意此时没有规定查询条件,所以默认是查询所有数据中的第一条</p>
</blockquote>
</blockquote>
</blockquote>
<p>{u'date': datetime.datetime(...),&quot;text&quot;: &quot;欢迎来&quot;pengfei.ga&quot;学习&quot;,u'_id': ObjectId('...'), u'author': u'鹏飞', u'tags': [u'mongodb', u'python', u'pymongo']}</p>
<pre><code>
题外话:这条返回值里面的&quot;_id&quot;其实就是我们刚才insert函数的返回值

find_one()函数当然也支持条件查询,比如我想搜索特定的author那么如下:
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts.find_one({&quot;author&quot;: &quot;鹏飞&quot;})<br>
{u'date': datetime.datetime(...), &quot;text&quot;: &quot;欢迎来&quot;pengfei.ga&quot;学习&quot;, u'_id': ObjectId('...'), u'author': u'鹏飞', u'tags': [u'mongodb', u'python', u'pymongo']}</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>如果我们搜索一位叫做&quot;Eliot&quot;的author,那mongo会返回nnothing

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts.find_one({&quot;author&quot;: &quot;Eliot&quot;})</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>我们也可以通过 _id 来查询:

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>post_id</p>
</blockquote>
</blockquote>
</blockquote>
<p>ObjectId(...)</p>
<blockquote>
<blockquote>
<blockquote>
<p>posts.find_one({&quot;_id&quot;: post_id})</p>
</blockquote>
</blockquote>
</blockquote>
<p>{u'date': datetime.datetime(...), u'text': u'My first blog post!', u'_id': ObjectId('...'), u'author': u'Mike', u'tags': [u'mongodb', u'python', u'pymongo']}</p>
<pre><code>请注意此处的此处的 ObjectId并不是string格式的:
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>post_id_as_str = str(post_id)<br>
posts.find_one({&quot;_id&quot;: post_id_as_str}) # No result</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>在一个web项目中,我们会常常使用从request里面获得的ObjectId来传到服务器查询,切记在查询钱把ObjectId转换一下格式

</code></pre>
<p>from bson.objectid import ObjectId</p>
<p>def get(post_id):<br>
# string =&gt; ObjectId:<br>
document = client.db.collection.find_one({'_id': ObjectId(post_id)})</p>
<pre><code>


哦对了别忘了Mongo的数据BSON格式只支持Utf-8哦,所以在web应用中切记高清UTF-8和UniCode的关系



####**Insert大量数据**

为了讲解更多花式搜索技巧,我们需要insert多一点数据,我们可以用到&quot;insert_many()&quot;这个函数,它会吧存在于list里面的每一个document插入到集合中去
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>new_posts = [{&quot;author&quot;: &quot;Mike&quot;,<br>
...               &quot;text&quot;: &quot;Another post!&quot;,<br>
...               &quot;tags&quot;: [&quot;bulk&quot;, &quot;insert&quot;],<br>
...               &quot;date&quot;: datetime.datetime(2009, 11, 12, 11, 14)},<br>
...              {&quot;author&quot;: &quot;Eliot&quot;,<br>
...               &quot;title&quot;: &quot;MongoDB is fun&quot;,<br>
...               &quot;text&quot;: &quot;and pretty easy too!&quot;,<br>
...               &quot;date&quot;: datetime.datetime(2009, 11, 10, 10, 45)}]</p>
</blockquote>
</blockquote>
</blockquote>
<h3 id="documentlist">插入的document是一个list</h3>
<blockquote>
<blockquote>
<blockquote>
<p>result = posts.insert_many(new_posts)<br>
result.inserted_ids</p>
</blockquote>
</blockquote>
</blockquote>
<p>[ObjectId('...'), ObjectId('...')] #返回值也是一个list</p>
<pre><code>
####**在集合中查询多条数据**

我们可以使用&quot;find()&quot;函数来在集合中一次查询多条记录,比如如下:
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>for post in posts.find():<br>
print post</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code></code></pre>
<p>... {u'date': datetime.datetime(...), u'text': u'My first blog post!', u'_id': ObjectId('...'), u'author': u'Mike', u'tags': [u'mongodb', u'python', u'pymongo']}</p>
<p>{u'date': datetime.datetime(2009, 11, 12, 11, 14), u'text': u'Another post!', u'_id': ObjectId('...'), u'author': u'Mike', u'tags': [u'bulk', u'insert']}</p>
<p>{u'date': datetime.datetime(2009, 11, 10, 10, 45), u'text': u'and pretty easy too!', u'_id': ObjectId('...'), u'author': u'Eliot', u'title': u'MongoDB is fun'}</p>
<pre><code>就跟上面说的find_one()函数,当我们不谢搜索条件时默认返回所有document

</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>for post in posts.find({&quot;author&quot;: &quot;鹏飞&quot;}):<br>
print post</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>则会返回:
</code></pre>
<p>{u'date': datetime.datetime(...), u'text': u'欢迎来&quot;pengfei.ga&quot;学习!' u'author': u'鹏飞', u'tags': [u'mongodb', u'python', u'pymongo']}</p>
<p>{u'date': datetime.datetime(2009, 11, 12, 11, 14), u'text': u'Another post!', u'_id': ObjectId('...'), u'author': u'鹏飞',tags': [u'bulk', u'insert']}</p>
<pre><code>####**一些数量的获取办法**
如果我们只是想获取某个集合中有多少条记录,那么我们只需使用count()函数,返回值是一个记录的数量:
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts.count()<br>
3</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>在条件搜索中也能这样使用:
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts.find({&quot;author&quot;: &quot;鹏飞&quot;}).count()<br>
2</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>####**Query的花式技巧**

在MongoDB中有很多不同的query方式, 比如我们按照时间来搜索(小于某个时间),然后结果使用author来排序:
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>d = datetime.datetime(2009, 11, 12, 12)<br>
for post in posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;):<br>
print post</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>返回
</code></pre>
<p>{u'date': datetime.datetime(2009, 11, 10, 10, 45), u'text': u'and pretty easy too!', u'_id': ObjectId('...'), u'author': u'Eliot', u'title': u'MongoDB is fun'}</p>
<p>{u'date': datetime.datetime(2009, 11, 12, 11, 14), u'text': u'Another post!', u'_id': ObjectId('...'), u'author': u'鹏飞', u'tags': [u'bulk', u'insert']}</p>
<pre><code>我们在这里使用了&quot;$lt&quot; 这个符号来代表&quot;小于&quot;然后使用sort()来通过author参数排序


####**添加索引**
如果我们想使上文的搜索结果更加迅速,我们可以添加一个&quot;date&quot;和&quot;author&quot;的&quot;复合索引&quot;,在开始之前,我们可以使用explain()这个函数来看一下我们普通的搜索(不使用索引)是怎么做到的吧
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;).explain()[&quot;cursor&quot;]<br>
u'BasicCursor'<br>
posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;).explain()[&quot;nscanned&quot;]<br>
3</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>我们可以看到这次搜索使用&quot;BasicCursor &quot;并且在集合中搜索了3次(我们集合中就3条数据)

现在我们来添加一个&quot;复合索引&quot;然后看看会发生什么事情
</code></pre>
<blockquote>
<blockquote>
<blockquote>
<p>from pymongo import ASCENDING, DESCENDING<br>
posts.create_index([(&quot;date&quot;, DESCENDING), (&quot;author&quot;, ASCENDING)])<br>
u'date_-1_author_1'<br>
posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;).explain()[&quot;cursor&quot;]<br>
u'BtreeCursor date_-1_author_1'<br>
posts.find({&quot;date&quot;: {&quot;$lt&quot;: d}}).sort(&quot;author&quot;).explain()[&quot;nscanned&quot;]<br>
2</p>
</blockquote>
</blockquote>
</blockquote>
<pre><code>这次只搜索了2次

(译者注:更多索引方式可以参考http://docs.mongodb.org/manual/core/indexes/)


英文原文地址:http://api.mongodb.org/python/current/tutorial.html</code></pre>
<!--kg-card-end: markdown-->
    