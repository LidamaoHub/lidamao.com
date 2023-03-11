
---
title: Flask 应用配置 Nginx 和 uwsgi
banner: /images/2015/12/San-Francisco--U-S-.jpg
cateogries: 
---
<!--kg-card-begin: markdown--><h2 id="">安装</h2>
<pre><code class="language-bash">$ sudo apt-get install nginx
$ sudo apt-get install python-pip
$ sudo pip install flask uwsgi
</code></pre>
<h2 id="flask">创建一个 Flask 应用</h2>
<pre><code class="language-bash">$ cd ~
$ mkdir blog
$ cd blog
$ touch views.py uwsgi.ini blog.sock
</code></pre>
<p>文件内容：</p>
<p><code>views.py</code></p>
<pre><code class="language-python">from flask import Flask
app = Flask(__name__)

@app.route('/')
def blog():
    return 'Hello, world!'

if __name__ == '__main__':
     app.run(host='0.0.0.0')
</code></pre>
<p><code>uwsgi.ini</code></p>
<pre><code>[uwsgi]
#application's base folder
base = /home/shaobing/blog

#socket file's location
socket = /home/shaobing/blog/blog.sock

#permissions for the socket file
chmod-socket = 666

#the variable that holds a flask application inside the module imported at line #6
wsgi-file = views.py
callable = app

#location of log files
logto = uwsgi.log
</code></pre>
<h2 id="nginx">Nginx 配置</h2>
<pre><code class="language-bash">$ sudo vim /etc/nginx/sites-available/blog
文件内容，在server_name 填上域名或IP（pengfei.com）(域名已解析到此服务器IP)
server {
    listen 80;
    server_name pengfei.com;

    location / {
        include uwsgi_params;
        uwsgi_pass unix:/home/shaobing/blog/blog.sock;
    }
}
$ sudo ln -s /etc/nginx/sites-avialable/blog /etc/nginx/sites-enabled/
$ sudo service nginx reload
</code></pre>
<h2 id="uwsgi">启动 uwsgi</h2>
<pre><code class="language-bash">$ cd ~/blog
$ uwsgi --ini uwsgi.ini &amp;
</code></pre>
<p>在浏览器打开 <code>http://pengfei.com</code> 即可访问。</p>
<!--kg-card-end: markdown-->
    