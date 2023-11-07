---
title: Fluid主题美化
abbrlink: 86b04e37
date: 2023-10-14 23:11:42
banner_img: 
index_img: https://raw.githubusercontent.com/aaow/blog_img/main/2023/202311060048328.jpg
categories: Hexo博客
tags: 
- hexo
---

# Fluid主题美化

"站点配置" 指的 Hexo 博客目录下的 _config.yml，_

"主题配置" 指的是 theme/fluid/_config.yml 或者 _config.fluid.yml ，注意区分；



## 添加网站运行时间

页脚添加网站运行时间，只需要在主题配置中的 footer: content 添加：

```html
footer:
  content: '
    <div>
      <span id="timeDate">载入天数...</span>
      <span id="times">载入时分秒...</span>
      <script src="/js/duration.js"></script>
    </div>
  '
```
之后在主题目录下创建 source/js/duration.js，内容如下：
```js
var now = new Date();
function createtime() {
    var grt= new Date("03/03/2021 12:00:00");//在此处修改你的建站时间，格式：月/日/年 时:分:秒
    now.setTime(now.getTime()+250);
    days = (now - grt ) / 1000 / 60 / 60 / 24; dnum = Math.floor(days);
    hours = (now - grt ) / 1000 / 60 / 60 - (24 * dnum); hnum = Math.floor(hours);
    if(String(hnum).length ==1 ){hnum = "0" + hnum;} minutes = (now - grt ) / 1000 /60 - (24 * 60 * dnum) - (60 * hnum);
    mnum = Math.floor(minutes); if(String(mnum).length ==1 ){mnum = "0" + mnum;}
    seconds = (now - grt ) / 1000 - (24 * 60 * 60 * dnum) - (60 * 60 * hnum) - (60 * mnum);
    snum = Math.round(seconds); if(String(snum).length ==1 ){snum = "0" + snum;}
    document.getElementById("timeDate").innerHTML = "本站已安全运行 "+dnum+" 天 ";
    document.getElementById("times").innerHTML = hnum + " 小时 " + mnum + " 分 " + snum + " 秒";
}
setInterval("createtime()",250);
```
将建站时间修改为你自己的建站时间，即可在页脚加入网站运行时间。



## 添加动态彩带

将代码复制到此目录下 themes/fluid/source/js/DynamicLine.js。
```js
"object"==typeof window&&(window.Ribbons=function(){var t=window,i=document.body,n=document.documentElement,o=function(){if(1===arguments.length){if(Array.isArray(arguments[0])){var t=Math.round(o(0,arguments[0].length-1));return arguments[0][t]}return o(0,arguments[0])}return 2===arguments.length?Math.random()*(arguments[1]-arguments[0])+arguments[0]:0},s=function(o){var s=Math.max(0,t.innerWidth||n.clientWidth||i.clientWidth||0),h=Math.max(0,t.innerHeight||n.clientHeight||i.clientHeight||0);return{width:s,height:h,ratio:s/h,centerx:s/2,centery:h/2,scrollx:Math.max(0,t.pageXOffset||n.scrollLeft||i.scrollLeft||0)-(n.clientLeft||0),scrolly:Math.max(0,t.pageYOffset||n.scrollTop||i.scrollTop||0)-(n.clientTop||0)}},h=function(t,i){this.x=0,this.y=0,this.set(t,i)};h.prototype={constructor:h,set:function(t,i){this.x=t||0,this.y=i||0},copy:function(t){return this.x=t.x||0,this.y=t.y||0,this},multiply:function(t,i){return this.x*=t||1,this.y*=i||1,this},divide:function(t,i){return this.x/=t||1,this.y/=i||1,this},add:function(t,i){return this.x+=t||0,this.y+=i||0,this},subtract:function(t,i){return this.x-=t||0,this.y-=i||0,this},clampX:function(t,i){return this.x=Math.max(t,Math.min(this.x,i)),this},clampY:function(t,i){return this.y=Math.max(t,Math.min(this.y,i)),this},flipX:function(){return this.x*=-1,this},flipY:function(){return this.y*=-1,this}};var e=function(t){this._canvas=null,this._context=null,this._sto=null,this._width=0,this._height=0,this._scroll=0,this._ribbons=[],this._options={colorSaturation:"80%",colorBrightness:"60%",colorAlpha:.65,colorCycleSpeed:6,verticalPosition:"center",horizontalSpeed:150,ribbonCount:5,strokeSize:5,parallaxAmount:-.5,animateSections:!0},this._onDraw=this._onDraw.bind(this),this._onResize=this._onResize.bind(this),this._onScroll=this._onScroll.bind(this),this.setOptions(t),this.init()};return e.prototype={constructor:e,setOptions:function(t){if("object"==typeof t)for(var i in t)t.hasOwnProperty(i)&&(this._options[i]=t[i])},init:function(){try{this._canvas=document.createElement("canvas"),this._canvas.style.display="block",this._canvas.style.position="fixed",this._canvas.style.margin="0",this._canvas.style.padding="0",this._canvas.style.border="0",this._canvas.style.outline="0",this._canvas.style.left="0",this._canvas.style.top="0",this._canvas.style.width="100%",this._canvas.style.height="100%",this._canvas.style["z-index"]="-1",this._onResize(),this._context=this._canvas.getContext("2d"),this._context.clearRect(0,0,this._width,this._height),this._context.globalAlpha=this._options.colorAlpha,window.addEventListener("resize",this._onResize),window.addEventListener("scroll",this._onScroll),document.body.appendChild(this._canvas)}catch(t){return void console.warn("Canvas Context Error: "+t.toString())}this._onDraw()},addRibbon:function(){var t=Math.round(o(1,9))>5?"right":"left",i=1e3,n=200,s=0-n,e=this._width+n,a=0,r=0,l="right"===t?s:e,c=Math.round(o(0,this._height));/^(top|min)$/i.test(this._options.verticalPosition)?c=0+n:/^(middle|center)$/i.test(this._options.verticalPosition)?c=this._height/2:/^(bottom|max)$/i.test(this._options.verticalPosition)&&(c=this._height-n);for(var p=[],_=new h(l,c),d=new h(l,c),u=null,b=Math.round(o(0,360)),x=0;!(i<=0);){if(i--,a=Math.round((1*Math.random()-.2)*this._options.horizontalSpeed),r=Math.round((1*Math.random()-.5)*(.25*this._height)),(u=new h).copy(d),"right"===t){if(u.add(a,r),d.x>=e)break}else if("left"===t&&(u.subtract(a,r),d.x<=s))break;p.push({point1:new h(_.x,_.y),point2:new h(d.x,d.y),point3:u,color:b,delay:x,dir:t,alpha:0,phase:0}),_.copy(d),d.copy(u),x+=4,b+=this._options.colorCycleSpeed}this._ribbons.push(p)},_drawRibbonSection:function(t){if(t){if(t.phase>=1&&t.alpha<=0)return!0;if(t.delay<=0){if(t.phase+=.02,t.alpha=1*Math.sin(t.phase),t.alpha=t.alpha<=0?0:t.alpha,t.alpha=t.alpha>=1?1:t.alpha,this._options.animateSections){var i=.1*Math.sin(1+t.phase*Math.PI/2);"right"===t.dir?(t.point1.add(i,0),t.point2.add(i,0),t.point3.add(i,0)):(t.point1.subtract(i,0),t.point2.subtract(i,0),t.point3.subtract(i,0)),t.point1.add(0,i),t.point2.add(0,i),t.point3.add(0,i)}}else t.delay-=.5;var n=this._options.colorSaturation,o=this._options.colorBrightness,s="hsla("+t.color+", "+n+", "+o+", "+t.alpha+" )";this._context.save(),0!==this._options.parallaxAmount&&this._context.translate(0,this._scroll*this._options.parallaxAmount),this._context.beginPath(),this._context.moveTo(t.point1.x,t.point1.y),this._context.lineTo(t.point2.x,t.point2.y),this._context.lineTo(t.point3.x,t.point3.y),this._context.fillStyle=s,this._context.fill(),this._options.strokeSize>0&&(this._context.lineWidth=this._options.strokeSize,this._context.strokeStyle=s,this._context.lineCap="round",this._context.stroke()),this._context.restore()}return!1},_onDraw:function(){for(var t=0,i=this._ribbons.length;t<i;++t)this._ribbons[t]||this._ribbons.splice(t,1);this._context.clearRect(0,0,this._width,this._height);for(var n=0;n<this._ribbons.length;++n){for(var o=this._ribbons[n],s=o.length,h=0,e=0;e<s;++e)this._drawRibbonSection(o[e])&&h++;h>=s&&(this._ribbons[n]=null)}this._ribbons.length<this._options.ribbonCount&&this.addRibbon(),requestAnimationFrame(this._onDraw)},_onResize:function(t){var i=s(t);this._width=i.width,this._height=i.height,this._canvas&&(this._canvas.width=this._width,this._canvas.height=this._height,this._context&&(this._context.globalAlpha=this._options.colorAlpha))},_onScroll:function(t){var i=s(t);this._scroll=i.scrolly}},e}()),new Ribbons({colorSaturation:"60%",colorBrightness:"50%",colorAlpha:.5,colorCycleSpeed:5,verticalPosition:"random",horizontalSpeed:200,ribbonCount:3,strokeSize:0,parallaxAmount:-.2,animateSections:!0});
//# sourceMappingURL=/sm/b99565cefdee024086b6fa67cc07b93480e4c105aae2f7252ad84457dce9120b.map
```
然后在主题配置 _config.fluid.yml 中找到 custom_js 或者 custom_css，修改如下：

```yml
custom_js:
	- /js/DynamicLine.js
```



## 头部打字机颜色效果渐变

```css

/* 打字机效果渐变 -bynote.cn*/

#subtitle {
    background: linear-gradient(-45deg, #ee7752, #ce3e75, #23a6d5, #23d5ab);
    background-size: 400% 400%;
    -webkit-animation: Gradient 10s ease infinite;
    -moz-animation: Gradient 10s ease infinite;
    animation: Gradient 10s ease infinite;
    -o-user-select: none;
    -ms-user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    user-select: none;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}

#subtitle:before {
    background-color: rgba(0, 0, 0, 0);
}

```



## 连接优化

安装abbrlink插件：

```bash
npm install hexo-abbrlink --save
```

修改站点配置文件 `config.yml`

```yml
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: http://aaow.github.io
# permalink: :year/:month/:day/:title/
# permalink_defaults:
# pretty_urls:
#   trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
#   trailing_html: true # Set to false to remove trailing '.html' from permalinks
permalink: posts/:abbrlink/
abbrlink:
  alg: crc32 # support crc16(default) and crc32
  rep: hex   # support dec(default) and hex
```

### 参数说明:

alg – Algorithm：对应随机算法参数，可选值为crc16和crc32
rep – Represent：对应生成的数字表示方法，可选值为hex和dec
    

## 博客压缩

安装 neat

```bash
npm install hexo-neat --save
```

修改主站配置文件：

```yml
# hexo-neat
# 博文压缩
neat_enable: true
# 压缩html
neat_html:
  enable: true
  exclude:
# 压缩css  
neat_css:
  enable: true
  exclude:
    - '**/*.min.css'
# 压缩js
neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '**/*.min.js'
    - '**/jquery.fancybox.pack.js'
    - '**/index.js'
```

压缩的过程会极大的延长页面编译和部署的时间。如果报错，排除相关文件。





