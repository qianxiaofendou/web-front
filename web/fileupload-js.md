在前端操作中，经常会有文件上传的需求，除了样式好看外，还需要对文件的一些信息做一下校验，在之前的项目中采用的bootstrap+jquery的组合来写的，因此样式上借用bootstrap的一些组件来美化效果，下面说一下自己使用的一些注意点：


```html
<div class="form-group displayHide showHideSection" id="material">
    <label class="control-label">
        <span class="text-danger">*</span>素材
    </label>
    <div class="control-body">
        <input type="file" id="materialFile" name="mtFile1" style="display:none;"
        accept="image/*" />
        <input type="text" id="mtFile1" class="imgfile form-control " />
    </div>
    <div class="control-body">
        <button class="btn  btn-default" type="button" onclick="$('input[name=mtFile1]').click();">
            文件上传
        </button>
    </div>
</div>
```
上面是html信息，下面说一下js操作

```javascript
/**
 * 图片规则验证
 * id 文件域上传id 如：id="materialFile"
 * showid 这里指的是上传文件显示的输入框id  如上面的：id="mtFile1"
 * fn 错误信息显示函数
 * errElmId 错误信息显示id 如果有这个参数，按照这个参数来展示
 * 
 */
function validateImg(id,showid,fn,errElmId){   
  let size  = 600,
      regName = function(imgName){
         let reg = /.jpg$|.jpeg$|.gif$|.png$|.bmp$|.JPG$|.PNG$|.JPEG$|.GIF$|.PNG|.BMP|.SWF|.swf$/;
         if(typeof imgName == 'string' && imgName!=''){
           return reg.exec(imgName)==null?false:true;
         }else{
           return false;
         }

      };
  let $imgfile = $(`#${id}`);
  if($imgfile.prop("files").length>0){
     let files = $imgfile.prop("files");
     let ifImgType = true,
         strfile = '';
     for(let i = 0;i<files.length; i++){
       if(!regName(files[i].name)){
          ifImgType = false;
          if(typeof fn == 'function') fn(`${files[i].name} 文件格式不正确，请重新上传`,errElmId);
          $imgfile.val('');
          $(`#${showid}`).val(''); 
          // $imgfile.get(0).outerHTML = $imgfile.get(0).outerHTML;
       }else if(Math.ceil(parseInt(files[i].size)/1024) > size){
         ifImgType = false;
          if(typeof fn == 'function') fn(`${files[i].name} 文件大小超过${size}k，请重新上传`,errElmId);
          $imgfile.val('');
          $(`#${showid}`).val(''); 
          // $imgfile.get(0).outerHTML = $imgfile.get(0).outerHTML;
       }
       if(!ifImgType){             
          break;
       }
       
       strfile += files[i].name + ',';
     }


     if(!ifImgType){
       return;
     }else{      
       $(`#${showid}`).val(strfile);  
     }

  }
},


/**
 * 错误信息提示
 * 根据自己业务的需要，下面是我自己业务里面需要的，仅作参考
 */
function error_show(msg,id){  
   if(!id){    
     $('.alert-danger2 p').html(msg);
     $('#error-alert').show();
   }else{
     $('#'+id).find('.alert').text(msg).removeClass('displayHide');
   }
}

   $('#materialFile').change(function() {  
     // console.log($(this).prop('files'));
     validateImg('materialFile','mtFile1',error_show,'material');  
   }); 
```

        




