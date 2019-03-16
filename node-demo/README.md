写一个JSONP
请求方：A.com的前端（浏览器）
响应方：B.com的后端（服务器）
1.请求方创建script,src指向响应方，同时传一个查询参数?callbackName=xxx
2.响应方根据查询参数callbackName，构造形如
-xxx.call(undefined, '你要的数据')
-xxx('你要的数据')
3.浏览器接收到响应，就会执行xxx.call(undefined, '你要的数据')，请求方得到数据

约定：
1.不用callbackName，直接用callback
2.xxx函数使用随机数
 ```
functionName = 'xxx'+parseInt(Math.random()+1000, 10)
在window上定义这个函数
window[functionName] = function (result){
code...
}
script.src="http://B.com"+functionName
 script.onload = function (e) {
        e.currentTarget.remove()  //移出标签
        delete window[functionName]    //删除函数
    }
    script.onerror = function (e) {
        e.currentTarget.remove()
        delete window[functionName]
    }
 ```
