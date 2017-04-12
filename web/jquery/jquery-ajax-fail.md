>关于使用jquery的ajax链式操作，错误返回fail回调的一些参数详解
* 先看下代码
```
$.ajax({
  url: `你的Url`,
  type: 'post',
}).done(function(data,textStatus,jqXHR){
  //成功的时候的一些操作
}).fail(function(jqXHR,textStatus,errorThrown){
  //失败的时候的一些操作
}).always(function(data|jqXHR,textStatus,jqXHR|errorThrown){
  //当请求成功时，该函数的参数与.done()的参数一致；当请求失败时，该函数的参数与.fail()的参数一致。
})
```

### 第一个参数 jqXHR（jqXHR对象）
这里的jqXHR是一个jqXHR对象，在jQuery1.4和1.4版本之前返回的是XMLHttpRequest对象，1.5版本以后则开始使用jqXHR对象，该对象是一个超集，就是该对象不仅包括XMLHttpRequest对象，还包含其他更多的详细属性和信息。
* readyState :当前状态,0-未初始化，1-正在载入，2-已经载入，3-数据进行交互，4-完成。
* status  ：返回的HTTP状态码，比如常见的404,500等错误代码。
* statusText ：对应状态码的错误信息，比如404错误信息是not found,500是Internal Server Error。
* responseText ：服务器响应返回的文本信息

### 第二个参数 textStatus（String 字符串）
返回的是字符串类型，表示返回的状态，根据服务器不同的错误可能返回下面这些信息：
* “timeout”（超时）
*  “error”（错误）
* “abort”(中止)
* “parsererror”（解析错误）
* 或者返回空值
 
### 第三个参数 errorThrown（String 字符串）
也是字符串类型，表示服务器抛出返回的错误信息，如果产生的是HTTP错误，那么返回的信息就是HTTP状态码对应的错误信息，比如404的Not Found,500错误的Internal Server Error。