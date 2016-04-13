# 问题由来
原本需求：在移动端滑动列表时，列表对应的目录要切换至对应位置 要达到这样一个需求因为涉及到滑动  
自然想到了touch事件，然而，在touch事件写完后应用到移动端时  
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

 
| Table         | 安卓          | ios   |
| ------------- |:-------------:| -----:|
| touchstart    | 支持          |  支持 |
| touchmove     | 支持          |  支持 |
| touchend      | 不支持        |  支持 |
| touchcancel   | 支持          |不支持 |

其中测试安卓时用了自带浏览器和UC浏览器，测ios时用了谷歌，safari,和UC，浏览器对这些问题并没有什么影响。
**同一机型不同的浏览器是一样的效果**
## 总结
1.通过touch事件写dom时，如果不需要考虑滚动事件等等  
  可以在touchstart或touchmove 里执行一下 e.preventDefault()  *（不是很建议使用）*;  
2.如果需要考虑scroll事件那就只能在touchend绑定的同时绑定touchcancel  
  如上面源代码需要考虑scroll，则我们可以  
```js
$(".basket-list ul li").on("touchend  touchcancel",function(){
		//代码略
	});
```
3.因为scroll事件在滑动中有惯性所以其实用touch事件并不准确，经过实验用scroll事件监听会效果好一些，通过判断滑动什么时候结束来达到想要的效果
	
```js	
	var topvalue=0;
		
	var	length=$(".game-name-tab").length;
	$(window).scroll(function(){
			topvalue=$(window).scrollTop();
			setTimeout("test(topvalue)",100);
		
		
	});
	var test=function(x){
		if($(window).scrollTop()==x){
			//滚动条停止时触发的事件
			
		}
	};
```
4.在解决scroll问题的时候发现ios滑动效果变得不平滑了，一开始以为是scroll操作滚动条使得页面变得卡顿不再平滑  
 其实不然，我们在用css出现滚动条的时候是**overflow:auto;**或者**overflow:scroll; ** 
 这时候再加一条**-webkit-overflow-scrolling: touch;**就好
















