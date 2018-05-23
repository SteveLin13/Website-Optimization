# 网站性能优化项目

你要做的是尽可能优化这个在线项目的速度。注意，请应用你之前在[网站性能优化课程](https://cn.udacity.com/course/website-performance-optimization--ud884/)中学习的技术来优化关键渲染路径并使这个页面尽可能快的渲染。

## 优化概述

### Part 1: 优化 index.html 的 PageSpeed Insights 得分

没有使用建议的配置，而是将网站部署在Github Page上

根据 PageSpeed Insights 的 Optimization Suggestions 进行修改

#### 图片优化：
- 使用 convert 库压缩了 pizzeria.jpg 和 profilepic.jpg

#### 优化 CSS 发送过程：
- style.css： 使用 loadCSS 函数异步加载 CSS ，不会阻止初次呈现关键内容。（未提取关键CSS、未考虑浏览器适应问题）
- print.css： 使用媒体查询，只在打印内容时适用，网页初次加载不会阻塞渲染。
- WecFont： 使用 the Web Font Loader 异步加载 Google Font

#### 使用浏览器缓存：
- 指定最佳的 Cache-Control 政策：发现 GitHub Page 本身已经有相应的政策，但是 Header 不允许修改，通过 meta 又只能指定 html 的缓存政策

#### 缩减资源（HTML、CSS 和 JavaScript）的大小
- 最好创建一个使用这些工具的构建流程，以便缩减和重命名开发文件并将其保存到正式版目录。（如Grunt或Gulp，但在配置的时候都出现报错，还未解决）


----

### Part 2: 优化 pizza.html 的 FPS（每秒帧数）

#### 帧速
对 views/js/main.js 进行的优化可使 views/pizza.html 在滚动时保持 60fps 的帧速。

Solution：通过录制 views/pizza.html 在滚动时的 Performance ，发现问题在 Layout 和 Recalculate Style 上，即发生了 Forced reflow （强制同步布局且布局抖动）。因此找到相应的代码即 main.js:503 ,发现对 scrollTop 的赋值存在大量重复，遂将其移出for循环，问题解决。

![scrollPerformanceBegin](https://ws3.sinaimg.cn/large/006tKfTcgy1frgkruxejoj30p70dpjsz.jpg)

#### 计算效率
利用 views/pizza.html 页面上的 pizza 尺寸滑块调整 pizza 大小的时间小于5毫秒，大小的调整时间在浏览器开发工具中显示。

Solution：由于不知为何打开了 devTool ，页面上的 pizza 尺寸滑块就不可见，于是根据 console.log 的信息找到相应的代码，相关的函数 changePizzaSizes() ，发现对所有 pizza 对象的选取以及对 dx 和 newwidth 的赋值都存在无谓的重复，导致 javascript 运行耗费大量时间，将相应的命令提到 for 循环外，问题解决。


你可以在 Chrome 开发者工具帮助中找到关于 FPS 计数器和 HUD 显示的有用信息。[Chrome 开发者工具帮助](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### 一些关于优化的提示与诀窍
* [web 性能优化](https://developers.google.com/web/fundamentals/performance/ "web 性能")
* [分析关键渲染路径](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "分析关键渲染路径")
* [优化关键渲染路径](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "优化关键渲染路径！")
* [避免 CSS 渲染阻塞](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "css渲染阻塞")
* [优化 JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [通过 Navigation Timing 进行检测](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api")。在前两个课程中我们没有学习 Navigation Timing API，但它对于自动分析页面性能是一个非常有用的工具。我强烈推荐你阅读它。
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">下载量越少，性能越好</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">减少文本的大小</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">优化图片</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP缓存</a>

### 使用 Bootstrap 并定制样式
这个项目基于 Twitter 旗下的 <a href="http://getbootstrap.com/">Bootstrap框架</a> 制作。所有的定制样式都在项目代码库的 `dist/css/portfolio.css` 中。

* <a href="http://getbootstrap.com/css/">Bootstrap CSS</a>
* <a href="http://getbootstrap.com/components/">Bootstrap组件</a>
