title:  Jcrop图像裁剪
date: 2015-11-06 19:28:00
categories: 前端插件
tags: [图像裁剪,Jcrop]
toc: true
---
>Jquey Jcrop是一款功能强大的图像裁剪插件，结合服务端的处理可实现图片裁剪，改变等功能。
<!--more-->
一、Jcrop简介
---------

	Jquey Jcrop是一款功能强大的图像裁剪插件，结合服务端的处理可实现图片裁剪，改变等功能。
	具有如下特点:
	显示图像或块对象
	支持图片的最大，最小值的设置
	有交互性的API，包含动画
	提供CSS样式的定义
	支持IOS,android等平台

二、使用方法
------

    1.载入 CSS 文件
```
<link type="text/css" rel="stylesheet" href="../css/jquery.Jcrop.css" />
```
2.载入 JavaScript 文件
```
<script  type="text/javascript" src="jquery.js"></script> 
<script type="text/javascript" src="../js/jquery.Jcrop.js"></script>	
```
3.给 IMG 标签加上 ID
```
<div class="Preview-pic" id="previewIcon">
   <img id="imgCrop" class="imgCrop" name="imgCrop" 
	width="300" height="300" border="0" src=""/>	
  </div>
```
4.调用 Jcrop
```
jcrop_api=$.Jcrop('#imgCrop',{
		aspectRatio: 1,
		onChange: showCoords, // 选框改变时的事件
		onSelect: showCoords // 选框选定时的事件
	});
```

三、使用实例
------

   1.下载Jcop的依赖文件，下载地址如下:
 http://deepliquid.com/content/Jcrop.html,将css和js文件引入项目中	。
	
 2.页面定义
```
<header class="head-revision-top clearfix">
 <h4 class="fl" id="sysField">
	<a href="">系统头像</a>
  </h4>
  <p class="fr head-revision-file">
  <input class="head-revision-input" type="file" 
		accept="image/*" name="uploadFile" id="uploadFile"      onchange="checkImgType(this);" />
 </p>
</header>
<div class="head-revision mt20 clearfix" id="uploadIcon">
  <div class="Preview-pic" id="previewIcon">
	<img id="imgCrop" class="imgCrop" name="imgCrop" 
		width="300" height="300" border="0" src=""/>	
  
  </div>
```
3.js实现
```
 var firstFlag = false,count=0;
var jcrop_api, boundx, boundy;
$(function(){
	$('#userIconBtn').click(function() {
		var x = $("#x").val();
		var y = $("#y").val();
		var w = $("#w").val();
		var h = $("#h").val();
		if (w == 0 || h == 0) {
			alert("您还没有选择图片的剪切区域,不能进行剪切图片!");
			return;
		}else{
			$.ajax({
				url: '',
				data: $('#uploadIconForm').serialize(),
				type: "POST",
				beforeSend: function()
				{  
				},
				success: function()
				{
					// 提交成功，刷新页面
		        	//location.reload(true);
					window.location.href="";
				}
			});

		}
	});
});

/**检查图片上传类型*/          
 function checkImgType(obj){
 //更换头像销毁原有的jcrop	
 if(count>=1){
 	firstFlag=false;
 	jcrop_api.destroy();
 }
  var uploadFile = '';    
  //获取图片的全路径   
  var imgFilePath = getImgFullPath(obj);        
  var endIndex = imgFilePath.lastIndexOf("\\");   
  var lastIndex = imgFilePath.length-endIndex-1;   
  if (endIndex != -1)   
     uploadFile= imgFilePath.substr(endIndex+1,lastIndex);   
  else  
     uploadFile = imgFilePath;        
       
  var tag = true;               
  endIndex = imgFilePath.lastIndexOf(".");              
  if (endIndex == -1)    
    tag = false;   
       
  var ImgName = imgFilePath.substr(endIndex+1,lastIndex);   
  ImgName = ImgName.toUpperCase();           
    
  if (ImgName !="GIF" && ImgName !="JPG" && ImgName !="PNG" && ImgName !="BMP"){    
      tag=false;   
  }
  if (!tag) {   
      alert("上传图片的文件类型必须为: *.gif,*.jpg,*.png,*.bmp,请重新选择!")   
      Upload.clear(obj);                        
      return false;   
  }else{
  		count++;
		$.ajaxFileUpload({  
                url: "user!uploadFile",            //需要链接到服务器地址  
                secureuri:false,  
                fileElementId:"uploadFile",                  //文件选择框的id属性  
                dataType: "json",                            //服务器返回的格式，可以是json  
                success: function(data){
					if(data.resultCode=1){
						$("#imgCrop").attr("src",data.resultInfo);
						$("#imgCrop").attr("width",data.width);
						$("#imgCrop").attr("height",data.height);
						$("#width").val(data.width);
						$("#height").val(data.height);
						$("#imgFileExt").val(data.imgFileExt);
						$("#oldImgPath").val(data.oldImgPath);
						jcrop_api=$.Jcrop('#imgCrop',{
							aspectRatio: 1,
							onChange: showCoords, // 选框改变时的事件
							onSelect: showCoords // 选框选定时的事件
						});
					}else{
						alert("系统连接有误,请重新上传或连接管理员");
					}
					jcrop_api.enable();
					jcrop_api.setImage(data.resultInfo);
					jcrop_api.animateTo([0,0,200,200]);
					jcrop_api.setOptions({
								 allowResize:true,
								 allowSelect: false,
								 allowMove:true
								 });
					//获取图片的实际尺寸			 
					var bounds=jcrop_api.getBounds();
					boundx=bounds[0];
					boundy=bounds[1];
				} 
            });  
  		}
}     
function getImgFullPath(obj) {   
    if (obj) {     
       //ie     
       if (window.navigator.userAgent.indexOf("MSIE") >= 1) {     
           obj.select();     
           return document.selection.createRange().text;     
       }     
       //firefox     
       else if (window.navigator.userAgent.indexOf("Firefox") >= 1) {     
           if (obj.files) {     
               return obj.files.item(0).getAsDataURL();     
           }     
           return obj.value;     
       }            
       return obj.value;     
   }     
} 
function showCoords(c) {
	$('#x').val(c.x);
	$('#y').val(c.y);
	$('#x2').val(c.x2);
	$('#y2').val(c.y2);
	$('#w').val(c.w);
	$('#h').val(c.h);
	if(!firstFlag){
		exchange();
	}
	showPreview(c);

}		
function exchange(){
	firstFlag = true;
	$('#preview').attr("src", $('#imgCrop').attr("src"));
	
}
function showPreview(coords) {
	if (parseInt(coords.w) > 0) {
		var rx = 200 / coords.w;
		var ry = 200 / coords.h;
		$('#preview').css({
			width:Math.round(200/ coords.w * boundx) + 'px',  //200 为预览div的宽和高
            height:Math.round(200/ coords.h * boundy)+ 'px', 
			marginLeft : '-' + Math.round(rx * coords.x) + 'px',
			marginTop : '-' + Math.round(ry * coords.y) + 'px'
		});
	}
}
```
4.服务器处理（java）
```
public String uploadFile() {
	int result = Constant.RET_OK;
	String resultUrl = "";
	String imgUploadPath = "";
	// 图片初始化高度与宽度
	String width = null;
	String height = null;
	int imgWidth = 0;
	int imgHeight = 0;
	SimpleDateFormat df = new SimpleDateFormat("yyyyMMddHHmmss");
	String userWebAppPath = getWebAppPath();
	/** 检查是否有图片上传文件夹 */
	FileUtil.checkImageDir(userWebAppPath);
	try {
		UserPOJO userSession = (UserPOJO) getSession(Constant.USERLOGINSESSION);
		if (null == userSession) {
			result = Constant.ISSESSIONERROR;
		}
		// 指定图片宽度和高度
		width = getRequest().getParameter("width");
		if (null == width || "".equals(width)) {
			width = "300";
		}
		height = getRequest().getParameter("height");
		if (null == height || "".equals(height)) {
			height = "300";
		}
		imgWidth = Integer.parseInt(width);
		imgHeight = Integer.parseInt(height);
		List<File> files = getUploadFile();
		if (files != null && files.size() > 0) {
			for (int i = 0; i < files.size(); i++) {
				File userFile = files.get(i);
				try {
					String filePath = FileUtil.saveLocalImg(userFile, userWebAppPath + "\\"
							+ getUploadFileFileName().get(0));
					BufferedImage src = ImageIO.read(new File(filePath)); // 读入文件
					int imgSrcWidth = src.getWidth(); // 得到源图宽
					int imgSrcHeight = src.getHeight(); // 得到源图长
					// 重新指定大小
					if (imgSrcWidth >= imgSrcHeight) {
						imgWidth = imgSrcWidth > 300 ? 300 : imgSrcWidth;
						imgHeight = (int) (imgWidth * (float) imgSrcHeight / (float) imgSrcWidth);
						imgHeight = imgHeight < 200 ? 200 : imgHeight;
					} else {
						imgHeight = imgSrcHeight > 300 ? 300 : imgSrcHeight;
						imgWidth = (int) (imgHeight * (float) imgSrcWidth / (float) imgSrcHeight);
						imgWidth = imgWidth < 200 ? 200 : imgWidth;
					}
					// 按照图片的设置大小生成
					ImageCut.scale(src, userFile, imgWidth, imgHeight);
					log.info("创建" + imgWidth + "*" + imgHeight + "图片成功");
					// 上传图片到图片服务器并获得访问地址
					FileInputStream fis = new FileInputStream(userFile);
					byte[] fileBytes = new byte[fis.available()];
					fis.read(fileBytes);
					fis.close();
					resultUrl = FileUtil.getImageUrl(FileUtil.getExtName(this.getUploadFileFileName().get(0)),
							fileBytes);
					// 将远程服务器得到的文件保存到该服务器
					String newFileName = df.format(new Date()) + "_" + new Random().nextInt(1000) + "."
							+ FileUtil.getExtName(this.getUploadFileFileName().get(0));
					imgUploadPath = IMGROOT + newFileName;
					FileUtil.saveImageToLocal(resultUrl, userWebAppPath + newFileName);
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	} catch (Exception e) {
		e.printStackTrace();
		result = Constant.SYSTEM_ERROR;
	}
	Map<Object, Object> infoMap = new HashMap<Object, Object>();
	infoMap.put(Constant.RESULT_CODE_KEY, result);
	infoMap.put(Constant.RESULT_INFO, resultUrl);
	infoMap.put("width", imgWidth);
	infoMap.put("height", imgHeight);
	infoMap.put("oldImgPath", imgUploadPath);
	infoMap.put("imgFileExt", FileUtil.getExtName(this.getUploadFileFileName().get(0)));
	JSONObject json = JSONObject.fromObject(infoMap);
	writeJSON(json.toString());
	return null;
}
```

四、API接口及参数说明
------------

	http://code.ciaoca.com/jquery/jcrop/	

五、实现效果
------

 ![这里写图片描述](http://img.blog.csdn.net/20151105171544306)