# 问题由来
提供给移动端的js事件中发现在安卓上touchend触发不了  

##源代码

```js
$(".basket-list ul li").on("touchend ",function(){
		
		var height=$(".basket-list").scrollTop()+$(".basket-list").height()-$(".basket-list ul li").height();
		var menumlength=$(".basket-lefmenu ul li").length;
		for(var i=1;i<=menumlength;i++){
			
			var comparelength = $(".basket-list ul:nth-child("+i+")").offset().top-$(".basket-list ul:first-child").offset().top;
			if(i!=menumlength){
				var j=i+1;
				var comparelength2= $(".basket-list ul:nth-child("+j+")").offset().top-$(".basket-list ul:first-child").offset().top;
				if(height>=comparelength&&height<comparelength2){
					$(".basket-lefmenu ul li").delay("slow").removeClass("selected ");
					$(".basket-lefmenu ul li:nth-child("+i+")").delay("slow").addClass("selected");		
				}	
			}else{
				if(height>=comparelength){
					$(".basket-lefmenu ul li").delay("slow").removeClass("selected ");
					$(".basket-lefmenu ul li:nth-child("+i+")").delay("slow").addClass("selected");			
				}	
			}
		}
	});
```
这串代码主要是想通过在touchend的时候按照滚动条高度来切换目录  


##查阅资料-解决问题
移动项目开发过程中，经常需要用到滑动的事件来处理一些效果。  
通常情况下，我们会通过  touchstart->touchmove->touchend  的过程来定义这个事件。  
这些事件的触发顺序是  touchstart, touchmove, touchmove ….. touchend  。  
绝大部分平板或手机也正如我们想象的那样有序执行着。  
但是以**Android 4.0.4**为首的一些可恶分子却有些不听话：他们的touchend事件没有如预期的那样触发。
监听这些事件我们会发现，当只是轻点一下屏幕时，touchend可以正常触发。  
**但是只要当 touchmove 被触发之后，touchend 就不会再被触发了，而且 touchmove 也没有持续触发。**
这是属于安卓的bug

用自己的源代码测试了下： 



 
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| touchstart    | 支持          |  支持 |
| touchmove     | 支持          |  支持 |
| touchend      | 不支持        |  支持 |
| touchend      | 不支持        |  支持 |



| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
