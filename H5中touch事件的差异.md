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