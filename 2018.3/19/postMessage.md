##postMesage解决跨域,跨窗口消息传递
>平时做web开发的时候关于消息传递,除了客户端与服务器传值还有几个经常遇到的问题
>
>1. 页面和其他打开页面的数据传递
>
>2. 多窗口之间的消息传递
>
>3. 页面与嵌套的iframe消息传递
>
>4. 上面是哪个问题的跨域数据传递

####postMessage的出现
>这些问题都可以通过postMessage解决这个问题可以实现跨域文档,多窗口,跨域消息的传递
>
>两个窗口能通信的前提是，一个窗口以iframe的形式存在于另一个窗口，或者一个窗口是从另一个窗口通过window.open()或者超链接的形式打开的（同样可以用window.opener获取源窗口）
>
>postMessage(data,origin)方法接受两个参数
>
>data:要传递的数据,html5规范中提到该参数是javascript任意类型的对象,但是并不是所有浏览器都做到了这点,部分浏览器只能处理字符串所以,尽量用json.stringify处理一下
>
>origin:字符串参数,指明目标窗口的源,协议+主机+端口号+url url会被忽略所以可以不写,这个参数是为了安全考虑postMessage方法只会将message传递给指定窗口,如果愿意也可以将参数设置为'*'这样可以传递给任意窗口,如果要指定和当前窗口同源的话设置为'/';
>
>

#####下面看下不同源的iframe如果接受数据
```javascript
	  window.onload = function(){
    // 向目标源发送数据
	    document.getElementsByTagName('iframe')[0].contentWindow.postMessage({"age":10}, 'http://localhost:8080');
	  };
	
	  // 监听有没有数据发送过来
	  window.addEventListener('message', function(e) {
	      console.log(e);
	  });
	  
	  
	  
	  
  // 监听有没有数据发送过来
  window.addEventListener('message', function(e){
      // 判断数据发送方是否是可靠的地址
      if(e.origin !== 'http://localhost:81')
        return;
    // 打印数据格式
    console.log(e);
    // 回发数据
    e.source.postMessage('hello world', e.origin);
  }, false);
```

#####下面再来看看iframe数据交换,再来说说两个窗口之间的数据交换,我们知道用window.open()可以代开一个新的窗口没如果连个窗口同源,则两个窗口的通信将会非常简单,我们可以通过window.opener.functionName在新窗口中调用原来的窗口的方法和变量.如果这两个窗口不同源,这样的操作就会变的非常困难.所幸我们还有postMessage.使得window,opener.postMessage()不会报错 代码也很简单
```javascript
	
  // 打开一个新的窗口
  var popup = window.open('http://localhost:8080/index.html');

  /// When the popup has fully loaded, if not blocked by a popup blocker:
  setTimeout(function() {
      // 当前窗口向目标源传数据
    popup.postMessage({"age":10}, 'http://localhost:8080');
  }, 1000);
  
  
```