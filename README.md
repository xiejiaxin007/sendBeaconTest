# sendBeaconTest
sendBeacon异常上报
在前端应用越来复杂的今天，为了监控前端应用是否正常运行，通常会在前端收集一些错误与性能等数据，最终我们会将这些数据上报到服务端
上报方式有很多种，理论上我们只要能把数据发送给服务端就好了，在浏览器中可以发送请求的方式非常多，包括不限于：xhr、fetch、script标签、img标签、link标签、CSS背景图等。
目前最常用的方式：
1、利用img标签的src属性来发送请求：动态创建img标签
因为src属性一般是用来静态资源请求的，它能够不受同源策略的影响，直接进行数据的发送：(new Image).src = `/haopv.gif?a=xx&b=xxx`

2、使用sendBeacon来进行数据上报，这个方法是从ajax请求衍生而来的
思路：如果单纯从页面刷新、跳转和关闭的时候进行ajax请求，会出现页面已经关闭了，但是请求还未发出的这种情况，而且也会影响用户体验，所以不建议使用纯ajax请求。而上面的第一种方式，也会有一定的缺点，例如数据只能进行拼接、有可能abort，传输数据量也有限制
1、异常上报，发现JS错误，可以通过window.onerror函数来进行收集
2、上报数据的时机，如果发现一次上报一次，会影响页面的的流畅度，另外，有时候上报的数据不是错误数据，而是日常数据收集，则更为频繁，所以不能实时上报---可以在window.unload事件中进行数据上报，这个事件触发时机为页面刷新、关闭和跳转
3、在关闭页面的同时，常用的ajax可能会出现发不出去的情况，因为页面已经关闭了----选择使用sendBeacon信标来进行数据的发送，sendBeacon是异步的，不会影响当前页到下一个页面的跳转速度，且不受同域限制。sendBeacon 如果成功进入浏览器的发送队列后，会返回true；如果受到队列总数、数据大小的限制后，会返回false。返回ture后，只是表示进入了发送队列，浏览器会尽力保证发送成功，但是否成功了，不会再有任何返回值。目前暂无具体的数据长度限制标准。

注意：
1、sendBeacon只能发送POST请求
2、sendBeacon有返回值，true表示浏览器成功地将需要发送的数据放入处理队列中。false则没有，意味着请求无法发送成功。
3、传递的参数可选，可以是 ArrayBufferView, Blob, DOMString, 或者 FormData 类型的数据
4、后台接收参数的时候，需要使用流的方式进行接受，否则无法接收成功 


启动方式：
打开node-server里面的myapp文件夹，打开cmd，输入npm start即可运行node后台，进行数据的接受
