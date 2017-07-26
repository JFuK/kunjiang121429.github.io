---
title: localstorage和cookies实现多个标签页间的通信
date: 2017-07-26 11:44:56
tags:web通信
---
### localstorage实现
localstorage在一个标签页里被修改、添加和删除时都会触发一个storage事件，通过在另一个标签页内监听stroage事件,就可以获取localstorage存储的内容，实现不同标签页间的通信。
标签页1：
``` bash
<input id="name">
<input type="button" id="send" value="发送">
<script type="text/javascript">
	$(function(){
		$('#send').on('click',function(){
			var name=$('#name').val();
			localstorage.setItem("name",name);
		})
	})
</script>
```
标签页2：
``` bash
<script type="text/javascript">
	$(function(){
		window.addEventListener("storage",function(event){
			console.log(event.key+'='+event.newValue);
		});
	});
</script>
```
### cookies + setInterval实现
将要传递的信息存储在cookie中，设置定时器，每隔一段时间读取cookie信息，即可随时获取要传递的信息
<!--more-->
标签页1：
``` bash
<input id="name">  
<input type="button" id="submit" value="提交">  
<script type="text/javascript">  
    $(function(){    
        $("#submit").click(function(){    
            var name=$("#name").val();    
            document.cookie="name="+name;    
        });    
    });    
</script>
```
标签页2：
``` bash
<script type="text/javascript">  
    $(function(){   
        function getCookie(key) {    
            return JSON.parse("{\"" + document.cookie.replace(/;\s+/gim,"\",\"").replace(/=/gim, "\":\"") + "\"}")[key];    
        }     
        setInterval(function(){    
            console.log("name=" + getCookie("name"));    
        }, 10000);    
    });  
</script> 
```