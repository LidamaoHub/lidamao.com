
---
title: python学习之PIL库学习---制作ASCII风格图片
banner: /images/2015/12/New-York--U-S---1-.jpg
cateogries: 
---
<!--kg-card-begin: markdown--><p>今天闲来无事在网上发现了一种奇特的图片---ascii图片,如图<br>
<img src="/images/2015/12/20150126115322843.png" alt="" loading="lazy"><br>
放大可以发现,其实这张图完全是由字符按照不同灰度构成的(如图)<br>
<img src="/images/2015/12/20150126115427592.png" alt="" loading="lazy"><br>
这项技术在7-8年前很时髦,于是我准备研究一下</p>
<p>原理:我们可以发现虽然这个图片完全由文字生成,但基本的图案还是很清晰的,原理是人眼通过明暗的不同可以看出图片的大概轮廓</p>
<p>可以简单地把一个文字想象成一个白色的方块上占有一定面积的黑色图案,&quot;@&quot;在这个方块上黑色占的面积显然大于&quot;.&quot;中黑色占的面积</p>
<p>就是通过不同字符中黑色所占的面积达小,我们可以模拟出图像的灰度,从而形成图像的轮廓</p>
<h3 id="">开写:</h3>
<p>这次依旧使用python来写代码,我们需要用到的库只有PIL(python image labpython图像库)</p>
<p>ubuntu中可以直接简单安装<br>
<code>sudo apt-get install python-imaging  </code></p>
<p>接下来是主体代码</p>
<pre><code>import Image  
from PIL import Image,ImageDraw,ImageFont  
color_str = '@MNH$OC?7&gt;!:-;.'#这里是代表明暗的字符库  
  
def pic_to_ascii(img):  
  
    img_name = img.split('.')  
    img = Image.open(img).convert('L')#这里一步是使用pil库把图片处理成黑白  
    pix = img.load()#获取图像上每一个像素  
    width,height = img.size  
    pic =[]  
    for h in xrange(height):           
        img_wid = []  
        for w in xrange(width):  
            img_wid.append(color_str[int(int(pix[w,h])*len(color_str)/255 )-1])#由于灰度图片只有255个色阶，所以这里一步是换算出每个像素点所代表字符库中的字符  
        pic.append(img_wid)  
&lt;span style=&quot;white-space:pre&quot;&gt;    &lt;/span&gt;  
    pic_new = Image.new('RGB',(len(pic[0])*10,len(pic)*10),(255,255,255))#这里pil库生成一个根据原图长宽×10来换算出的新图（10是每个字符的大小）  
    draw = ImageDraw.Draw(pic_new)  
    font = ImageFont.truetype('/home/lee/.local/share/fonts/arial.ttf',10)#这里选一个本地的字体文件  
  
    for y in range(len(pic)):  
        for x in range(len(pic[0])):  
            draw.text((x*10,y*10),pic[y][x],(255,0,0),font=font)#最关键一步，在每一个位置上写一个字符，形成一张新图  
  
  
    pic_new.save(img_name[0]+&quot;_ascii.jpg&quot;,'JPEG')  
  
    print &quot;saved!&quot;  
  
  
def main():  
    pic_name = raw_input(&quot;please input the name of the picture:&quot;)  
    pic_to_ascii(pic_name)  
  
if __name__ == '__main__':  
        main()  
</code></pre>
<p>大功告成,此时只需要在app.py文件夹放入目标文件并在文件中定义其名字,即可完成字符画转换</p>
<!--kg-card-end: markdown-->
    