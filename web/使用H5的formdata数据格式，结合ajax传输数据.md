

当使用jquery的ajax来操作多文件上传保存到后台时候，可以结合HTML5的FormData来操作，具体如下：

> 1、定义一个formdata
```javascript

 var formData = new FormData();
```
> 2、通过formData的append方法往里面追加参数，比如：

```javascript
formData.append('username','用户名');
formData.append("userpassword",'用户密码');
```

> 3、如果是文件上传，并且是多文件上传，这里会有一个问题

```javascript
if($('#materialFile').prop('files').length) { 
    console.log('img file',$('#materialFile')[0].files);
    //获取当前文件域里面的文件，如果是单文件上传直接formData.append('imageFiles', $('#materialFile')[0].files[0]) 即可
    let filelist = $('#materialFile')[0].files;    
    for(let i = 0;i<filelist.length;i++){
       //这里不会覆盖上一次的值，如果不写循环直接把文件域给append进去，后端取出来会有问题，很奇怪
      formData.append('imageFiles', filelist[i]); 
    }
}
```
> 3、最后使用ajax发送数据的时候可以直接把formData传过去

- processData、contentType、cache这三个参数必须要传，并且设置值为==false==
```javascript
$.ajax({
	url: '你的处理数据的url或者action',
	type: 'post',
	data: formData,
	cache: false,
	async:false,
	processData: false,
	contentType: false
}).done(function(data){
	console.log(data)
})
```
> 4、ajax请求formdata截图，仅供参考

  ![image](https://qianxiaofendou.github.io/web-front/img/web-ajax-form.png)
