title: Android设备微信及部分浏览器性能分析
---

# Android设备微信及部分浏览器性能分析

------
## 相关知识点
### Chromium与Chrome
本段内容来自：<a href="http://www.zhihu.com/question/19705312">http://www.zhihu.com/question/19705312</a>
#### Chromium：
    1.Google Chrome的基础，是一个开源项目;
    2.拥有诸多尖端优势;
    3.拥有众多的版本包括Windows、Mac及Linux，几乎每天都在进行更新;
    4.该版本不稳定
#### Google Chrome dev：
    1.以Chromium最新版本为基础;
    2.每星期更新;、
    3.相对稳定，类似同类产品的“测试版”;
    4.最新的功能和特性都有体现;
#### Google Chrome beta：
    1.基于稳定的开发版本
    2.每月更新
    3.最新的功能和特性基本定型;
    4.可以正常使用，很少会出错;
#### Google Chrome Stable(稳定版)：
    1.基于最稳定的Google Chrome beta;
    2.新功能基本固定，经过数月测试;
    3.每隔几个月发布
    4.版本稳定
### WebView
    A View that displays web pages. This class is the basis upon which you can roll your own web browser or simply display some online content within your Activity. It uses the WebKit rendering engine to display web pages and includes methods to navigate forward and backward through a history, zoom in and out, perform text searches and more.
上面一段文字来自<a href="http://developer.android.com/reference/android/webkit/WebView.html">Android开发者网站</a>，从这段文字对WebView的描述可以知道，Android设备提供了可以供Android apps调用的WebView，可以用来显示网页。它使用WebKit内核渲染引擎来解析和显示页面。但是要注意的的是WebView使用的WebKit内核并非完整的WebKit内核（缺省的WebKit内核），也就是说它使用的WebKit内核与Chromium(Chrome)使用的WebKit内核并不是完全一样的。随着需求的变化，高版本的Android也在不断增加和优化内置WebKit的性能以及对HTML5的支持。在最新的Android 4.4 Kitkat版本中，原本基于Android WebKit的WebView实现被换成基于Chromium的WebView实现。基于Chromium的 Webview提供更广的HTML5,CSS3,Javascript支持，在目前最新Android 系统版本5.0上基于chromium 37，Webview提供绝大多数的HTML5特性支持。Webkit JavaScript引起采用WebCore Javascript 在Android 4.4上换成了V8能直接提升JavaScript性能。另外Chromium 支持远程调试(Chrome DevTools)。

## Android各版本的WebView
本段内容来源:<a href="http://blog.csdn.net/typename/article/details/40425275">http://blog.csdn.net/typename/article/details/40425275</a>
根据Google公布的Android 各个系统版本市场占有率(Google Android dashboards), Android 4.0及其以上系统将近90%左右，发展趋势必将是未来市面上几乎是Android 4.0以上系统。下文主要关注Android 4.0及以上系统WebView的实现，从Android WebView实现的Framework层大致可以分为三段Android 4.0系列，Android 4.1---4.3系列，Android 4.4及其以上系列。
WebKit for WebView VS Chromium for WebView性能比对（测试环境 小米2. CM Browser. Android 4.1.1 VS 4.4.3）
<table>
<tr>
    <th></th>
    <th>Webkit for Webview</th>
    <th>Chromium for Webview</th>
    <th>备注</th>
</tr>
<tr>
    <td>HTML5</td>
    <td>278</td>
    <td>434</td>
    <td><a href='http://html5test.com/'>http://html5test.com/</a></td>
</tr>
<tr>
    <td>远程调试</td>
    <td>不支持</td>
    <td>支持</td>
    <td>Android 4.4及以上支持</td>
</tr>
<tr>
    <td>内存占用</td>
    <td>小</td>
    <td>大</td>
    <td>相差20-30M左右</td>
</tr>
<tr>
    <td>WebAudio</td>
    <td>不支持</td>
    <td>支持</td>
    <td>Android 5.0及以上支持</td>
</tr>
<tr>
    <td>WebGL</td>
    <td>不支持</td>
    <td>支持</td>
    <td>Android 5.0及以上支持</td>
</tr>
<tr>
    <td>WebRTC</td>
    <td>不支持</td>
    <td>支持</td>
    <td>Android 5.0及以上支持</td>
</tr>
</table>
## 微信
微信作为一个app是没有自带浏览器内核的，所以使用微信浏览网页的时候，微信是通过WebView调用Android设备内置的WebKit内核（在实际操作中发现，在Android 4.2设备安装了QQ浏览器的情况下，微信在打开网页的时候会调用QQ浏览器的内核，而不会调用Android设备内置的WebKit内核, Android 4.4设备即使安装了QQ浏览器，微信依然调用Android内置的WebKit内核而非QQ浏览器的内核，个人猜测应该是4.4版本的WebView禁用了相应接口）显示网页的。目前，大部分的Android设备自带的浏览器内核都是WebKit内核。

## 部分浏览器及微信性能比较
### Chrome浏览器
- userAgent:Mozialla/5.0 (Linux; Android 4.4.2; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Chrome/39.0.2171.59 Mobile Safari/537.36
- 浏览器内核：WebKit 537.36(Chrome 39.0.2171.59)
- html5test:474
- GPU(FishIETank):50FPS
- JS性能测试：2745.1ms(2.0%)

### Firefox浏览器
- userAgent:Mozialla/5.0 (Android; Mobile; rv:33.0) Gecko/33.0 Firefox/33.0
- 浏览器内核：Gecko 33.0
- html5test:483
- GPU(FishIETank):18FPS
- JS性能测试：---

### UC浏览器
- userAgent:Mozialla/5.0 (Linux; U; Android 4.4.2; zh-CN; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/534.30(KHTML,like Gecko) Version/4.0 UCBrowser/9.9.7.500 U3/0.8.0 Mobile Safari/534.30
- 浏览器内核：WebKit 534.30(Chrome)
- html5test:460
- GPU(FishIETank):35FPS(没有出现鱼，且场景倒置)
- JS性能测试：1911.8ms(2.0%)

### opera浏览器
- userAgent:Mozialla/5.0 (Linux; Android 4.4.2; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Chrome/38.0.2125.102 Mobile Safari/537.36 OPR/25.0.1619.84.37
- 浏览器内核：Webkit 537.36(Chrome 38.0.2125.102)
- html5test:489
- GPU(FishIETank):45FPS
- JS性能测试：2917.2ms(1.6%)

### 猎豹浏览器
- userAgent:Mozialla/5.0 (Linux; Android 4.4.2; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Version/4.0 Chrome/30.0.0.0 Mobile Safari/537.36 LieBaoFast/2.17.3
- 浏览器内核：Webkit 537.36(Chrome 30.0.0.0 Mobi)
- html5test:428
- GPU(FishIETank):14FPS
- JS性能测试：1996.3ms(2.4%)

### 海豚浏览器
- userAgent:Mozialla/5.0 (Linux; Android 4.4.2; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Chrome/30.0.0.0 Mobile Safari/537.36
- 浏览器内核：Webkit 537.36(Chrome 30.0.0.0 Mobi)
- html5test:463
- GPU(FishIETank):10FPS
- JS性能测试：2561ms(1.8%)

### QQ浏览器
- userAgent:Mozialla/5.0 (Linux; U; Android 4.4.2; zh-cn; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Version/4.0 MQQBrowser/5.4 Mobile Safari/537.36
- 浏览器内核：Webkit 537.36(Chrome)
- html5test:442
- GPU(FishIETank):13FPS
- JS性能测试：2832ms(3.8%)

### 百度浏览器
- userAgent:Mozialla/5.0 (Linux; U; Android 4.4.2; zh-cn; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/534.24(KHTML,like Gecko) Version/4.0 Mobile Safari/534.24 T5/2.0 baidubrowser/5.4.5.0 (Baidu; P1.4.4.2)
- 浏览器内核：Webkit 534.24(Chrome)
- html5test:436
- GPU(FishIETank):37FPS
- JS性能测试：1917ms(1.8%)

### Android自带浏览器

 - userAgent:Mozialla/5.0 (Linux; Android 4.4.2; HUAWEI P6-U06
   Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Version/4.0
   Chrome/30.0.0.0 Mobile Safari/537.36
- 浏览器内核：Webkit 537.36(Chrome 30.0.0.0 Mobi)
- html5test:426
- GPU(FishIETank):7FPS
- JS性能测试：3476.3ms(2.1%)

### 微信
- userAgent:Mozialla/5.0 (Linux; Android 4.4.2; HUAWEI P6-U06 Build/HuaweiP6-U06) AppleWebkit/537.36(KHTML,like Gecko) Version/4.0 Chrome/30.0.0.0 Mobile Safari/537.36 MicroMessenger/6.0.0.56_r856074.501 NetType/WIFI
- 浏览器内核：Webkit 537.36(Chrome 30.0.0.0 Mobi)
- html5test:428
- GPU(FishIETank):10FPS
- JS性能测试：1426.6ms(1.4%) 

*注1：测试环境：华为P6，Android4.4
*注2：html5test是一个在线测试浏览器对html5支持度的系统（分数越高，对HTML5的支持越好），<a href="http://html5test.com/">测试地址</a>
*注3：GPU加速测试是衡量浏览器在硬件加速方面的情况，HTML5 GPU硬件加速，可以借助GPU的效能来渲染标准的Web内容，如文字、图像、视频、SVG（可缩放矢量图形）等网络信息，减少CPU负荷，大大的提高浏览器的速度。<a href="http://ie.microsoft.com/testdrive/Performance/FishIETank/Default.html">测试地址</a>
*注4：JS性能测试可以衡量一款浏览器的JavaScript引擎性能。<a href="http://www.webkit.org/perf/sunspider/sunspider.html">测试地址</a>
<table>
    <tr>
        <th>浏览器(微信)</th>
        <th>内核</th>
        <th>html5test</th>
        <th>GPU加速</th>
        <th>JS性能测试</th>
    </tr>
    <tr>
        <td>Chrome</td>
        <td>WebKit 537.36(Chrome 39.0.2171.59)</td>
        <td>474</td>
        <td>50FPS</td>
        <td>2745.1ms(2.0%)</td>
    </tr>
    <tr>
        <td>Firefox</td>
        <td>Gecko 33.0</td>
        <td>483</td>
        <td>18FPS</td>
        <td></td>
    </tr>
    <tr>
        <td>UC</td>
        <td>WebKit 534.30(Chrome)</td>
        <td>460</td>
        <td>35FPS(没有出现鱼，且场景倒置)</td>
        <td>1911.8ms(2.0%)</td>
    </tr>
    <tr>
        <td>opera</td>
        <td>Webkit 537.36(Chrome 38.0.2125.102)</td>
        <td>489</td>
        <td>45FPS</td>
        <td>2917.2ms(1.6%)</td>
    </tr>
    <tr>
        <td>猎豹</td>
        <td>Webkit 537.36(Chrome 30.0.0.0 Mobi)</td>
        <td>428</td>
        <td>14FPS</td>
        <td>1996.3ms(2.4%)</td>
    </tr>
    <tr>
        <td>海豚</td>
        <td>Webkit 537.36(Chrome 30.0.0.0 Mobi)</td>
        <td>463</td>
        <td>10FPS</td>
        <td>2561ms(1.8%)</td>
    </tr>
    <tr>
        <td>QQ</td>
        <td>Webkit 537.36(Chrome)</td>
        <td>442</td>
        <td>13FPS</td>
        <td>2832ms(3.8%)</td>
    </tr>
    <tr>
        <td>百度</td>
        <td>Webkit 534.24(Chrome)</td>
        <td>436</td>
        <td>37FPS</td>
        <td>1917ms(1.8%)</td>
    </tr>
    <tr>
        <td>Android</td>
        <td>Webkit 537.36(Chrome 30.0.0.0 Mobi)</td>
        <td>426</td>
        <td>7FPS</td>
        <td>3476.3ms(2.1%)</td>
    </tr>
    <tr>
        <td>微信</td>
        <td>Webkit 537.36(Chrome 30.0.0.0 Mobi)</td>
        <td>428</td>
        <td>10FPS</td>
        <td>1426.6ms(1.4%)</td>
    </tr>
</table>

## 总结
根据上面的测试信息可以看出，只有Firefox使用的是Gecko内核，其他的都是用的是WebKit内核，但是所使用的WebKit内核也不是都一样。从各个运行环境对HTML5的支持情况可以看出，Opera，Firefox与Chrome的支持最好，其次是UC和海豚，最差的就是QQ，百度，猎豹，Android自带浏览器以及微信。从GPU加速可以看出Chrome，Opera的情况最好，其次是UC和百度，Firefox，猎豹，QQ，海豚，Android自带浏览器以及微信在这方面是比较差的。从JS性能测试上（这个貌似可以作弊）可以看出JS性能与环境有很大的关系（Android自带浏览器与微信之间的比较），微信，UC，百度和猎豹的JS性能比较好，其次是Opera，Chrome，海豚，QQ，最差的是Android自带浏览器。综合来看Opera，Chrome，UC是Android设备中对HTML5应用支持较好的浏览器，其次是Firefox，海豚，QQ，猎豹以及百度浏览器，Android自带浏览器以及微信表现最差。



