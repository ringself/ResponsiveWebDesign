响应式解决方案
=====================

现状
-----------
E8及以下的浏览器不支持media query，目前流行的兼容低版本对css3 media query的支持方案：使用[media-queries.js](http://code.google.com/p/css3-mediaqueries-js/) 或 [respond.js](https://github.com/scottjehl/Respond) 

<! --[if lt IE 9] > 
< script src="http://css3-mediaqueries-js.googlecode.com/svn/trunk/css3-mediaqueries.js"></ script >
< ![endif]-- >   
由于js框架实现原理导致css跨域的问题，可以通过页面代理的方式通过js解决跨域，详细见respond里面的respond.proxy.js 。
对比2个框架的实现原理后，respond.js：快速、轻量，用于为 IE6-8 以及其它不支持 CSS3 Media Queries 的浏览器提供媒体查询的 min-width 和 max-width 特性；但css3-mediaqueries.js对CSS3 Media Queries属性的支持会比较全，这里可以尝试改进下js在分析样式表上面的性能。

分析：
------------
目前中文站用户浏览器类型占比数据：



目前中文站用户浏览器尺寸数据：


IE6-8占比达到51.24%，呈逐步下降的趋势。
考虑到目前我们主要是用于PC端屏幕尺寸990-1190-1390响应，并且基于正常真实用户在打开页面以后很少去缩放窗口尺寸，IE6-8版本浏览器仅用于初始化的显示状态，不做缩放下的resize事件，高级浏览器下原生支持CSS3 Media Queries的，直接用css3的写法进行编写代码，IE6-8下则采用样式继承区分开不同尺寸下的渲染。	

实现：
--------------
>case1：用户第一次访问页面，通过js判断是否IE6-8，如果是则判断窗口尺寸，然后根据990-1190-1390的尺寸标准，在HTML标签上写入区别当前尺寸的class标识，操作cookie写入。		
990 1190 1390三个尺寸对应ie下的命名 		
max1024  max1280  max1440	


>case2：用户再次访问页面，后台通过读取cookie，进行判断后得知用户浏览器当前尺寸的信息，然后根据约定好的class标识，在返回页面源码的时候，在HTML标签上设置对应的class，这样IE6-8的用户渲染页面的时候 ，会按当前的尺寸应用合适的css样式，同时为了避免用户多开窗口，而且尺寸不一致的情况下，页面通过JS再次计算对比cookie里面的class标识信息跟当前的窗口是否一致，如果不一致，则通过JS重新进行步骤1的操作，这里需要注意的是，JS尽量提前运行，减少页面出现闪动的情况。	

Demo预览地址：http://ali-062323w.hz.ali.com/media/demo1.htm		



Css代码demo：（可以利用less或者scss动态生成css）

/* 1390 */	
.max1440 xx{}


/* 1190 */	
.max1280 xx{}


/* 990 */	
.max1024 xx{}



#####参考资料：
使用CSS3 Media Queries实现响应式设计 http://www.cnblogs.com/softlover/archive/2012/11/21/2781388.html	
帮助查看响应式设计效果书签工具 http://lab.maltewassermann.com/viewport-resizer/	
一淘的响应式小结ppt    http://yunpan.alibaba-inc.com/share/link/285HNCOl1 	
响应式布局框架解决方案 http://www.dqqd.me/which-responsive-frameworks/	
优秀响应式页面展示 http://foodsense.is/	
media query ie8-兼容实现总结 http://www.36ria.com/5960	
