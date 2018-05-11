# week 9 note
## intro to ```jQuery```
### jQuery 
- jQuery是一个轻量级js库
	- 提供包装好的通用js事务处理方法
		- 访问DOM/HTML；AJAX
- jQuery可以在大部分主流浏览器运行->跨浏览器
- 大公司的支持

### using jQuery
- 以单个js文件的形式发布库
	- 方便下载安装，由cdn支持
- cdn的优势: 减小你的服务器带宽载荷，减小加载时间

### jQuery语法
- basic syntax is: ```$(selector).action()```
	- $表示使用jQuery
	- (selector)找到HTML元素
	- jQuery action() 元素被执行的操作
	- example: ```$("test").hide()```隐藏class="test"的元素
- jQuery selector
	- 类似css的选择器
	- 四个基本选择器
		- $("*"): 通用选择器，匹配所有元素(slow)
		- $("tag"): 元素选择器，匹配所有给定的元素名
		- $(".class"): 类选择器，匹配所有给定的css class元素
		- $("#id"): id选择器，匹配所有给定的HTML id属性的元素
	- 其他定义在css里的选择器也可以使用
	- example:
		- to select the single <div> element with id="grab": var singleElement = $("#grab");
		- to get a set of all <a> elements: var allAs = $("a");
		- to select all odd <tr> elements: $("tr:odd");
		- these selectors replace the use of getElementById()
		- 所包含的属性用":"隔开, 如：$("ul a:link")取<ul><li><a href=""></a></li></ul>中的href
		- 所包含的子元素用空格隔开，如：$("#main time").
	- more selector
		- 伪类选择器：select all links that have been visited: ```var visitedLinks = $("a: visited")；···
		- 超css选择器
			- content filters: 基于标准选择元素。如：元素有一个特定子元素或者子元素包含特定的一段文本
			- form selector: 选择表单元素的速记法
			
		- content filter: ```$("body *:contains('warning')");```
		- form selectors: 
			![form filters](./form filter.png)

### registering event handler
- 标准事件处理语法：
	```
	$("p").click(function(){
		//actions here
	});
	```
- 通用DOM事件: ```click, dbclick, mouseenter, mouseleave```
- the ```document ready``` event
	- 把jQuery放到document ready 事件中是一个好习惯
	- ready event由jQuery定义，在DOM加载完成后触发
	- example: 
		```
		$(document).ready(function(){
			$("input[type=file]").change(function(){
				console.log("the file to upload is: " + this.value);
			});
		});
		```
		
### normal DOM manipulation(操作)
- 创建元素：
	```
	//pure js way
	var jsLink = document.createElement("a");
	jsLink.href = "http://www.funwebdev.com";
	jsLink.innerHTML = "visit us";
	jsLink.title = "JS";
	
	//jQuery way
	var jQueryLink = $("<a href='http://www.funwebdev.com' title='jQuery'>Visit us</a>");

	//jQuery long-form way
	var jQueryVerboseLink = $("<a></a>");
	jQueryVerboseLink.attr("href", 'http://www.funwebdev.com');
	jQueryVerboseLink.attr("title", "jQuery verbose");
	jQueryVerboseLink.html("visite us");
	```
	
- appending DOM elements 拼接DOM元素
	- HTML before 
	```
	<div class="linkOut">
		funwebdev.com
	</div>
	```  
	- jQuery append
	```
	$(".linkOut").append(jsLink);
	```  
	- HTML after
	```
	<div class="linkOut">
		funwebdev.com
		<a href="http://www.funwebdev.com">Visit us</a>
	</div>
	```
	
- prepending DOM element 
	- HTML before 
	```
	<div class="linkOut">
		funwebdev.com
	</div>
	```  
	- jQuery append
	```
	$(".linkOut").append(jsLink);
	```  
	- HTML after
	```
	<div class="linkOut">
		<a href="http://www.funwebdev.com">Visit us</a>
		funwebdev.com
	</div>
	```

- useful methods
	- 用```attr()``` 来设置或者获取属性：```var link = $("a").attr("href");``` & ```$("img").attr("class", "fancy");```
	- css 属性通过```css()```来设置或获取：```$("#colourBox").css("background-color", "#FF0000");```
	- 一个HTML元素的内容可以通过```html()```来获取或更新
	- 元素的值通过```val()```来获取
	
### AJAX
- 同步与异步请求
	- 同步：浏览器等待响应，收到响应再渲染页面
	- 异步：允许用户在服务器处理请求的同时与其他部分交互。
		- 一般通过客户端脚本实现。创建```XMLHttpRequest```对象来管理请求，使用implicit/explicit回掉函数来处理响应
	- ajax example:
		```
		<html>
			<head>
				<meta http-equiv="Content-type" content="text/html; charset=UTF-8">
				<style type="text/css">
					.box{border: 1px solid black;
					padding: 1000px;
				}</style>
				<title>Switch content Asynchronous</title>
				<script src="https://code.jquery.com/jquery-3.3.1.min.js"
						integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
						crossorigin="anonymous"></script>
				<script type="text/javascript">
					$(document).ready(function(){
						$("img").mouseenter(function(){
							var url = "https://zhuanlan.zhihu.com/p/" + $(this).attr("id");
							$("#contentArea").load(url);
						});
						$("img").click(function(){
							var url = "https://www.google.com";
							$("#contentArea").load(url);
						});
						$("img").mouseleave(function(){
							$("#contentArea").html("");
						});
					});
					
				</script>
			</head>
			<body>
				<h1>Mouse over a book for more information</h1>
				<img src="./haha.jpg" id="36725275">
				<div class="box" id="contentArea"></div>
			</body>
		</html>
		```
	- jQuery ajax support： jQuery 提供多种异步请求的方法
		- 往所选元素里加载一个url的响应使用```load(url, data, callback)```. data是可选的，能和请求一起被发送。回掉函数可选，在load()结束后执行。   
			```$(selector).load(URL, data, callback);``` 无data用get，有data用post
		- 用http get请求数据：```get(url, data, callback);``` callback在响应到达后执行。  
			```$.get(url, data, callback);```
		- 用http post请求数据：```post(utl, data, callback);``` callback在响应到达后执行。  
			```$.post(url, data, callback);```
			
### integrate jQuery with Expressjs application
- change to the title form view
	- 给返回结果一个存放位置
	- 给脚本加上一个引用
	- 改变上传按钮的操作
	- example：
		- titleFormAjax.pug
		```
		doctype html
		
		html(lang="en")
		head
			title ajax search eample
			script(src="")
			script(src="")
		body
			h2 Simple Form
			input#title(type="search", placeholder="bbc")
			button#button(type="button") search
			div#results
		```
		- client side script
		```
		$(document).ready(function(){
			$('#button').on('click', function(e){
				var parameters = {title: $("#title").val()};
				$.get('revisionajax/getLatest', parameters, function(result){
					$('#results').html(result);
				});
			});
		});
		
		//same functon
		
		$(document).ready(function(){
			$('#button').on('click', function(){
				var data = $('#title').val();
				$('#results').load('revisionajax/getLatest?title=' + data);
			});
		});
		```
	- 注意事项：
		- 改路径
		- url到controller的映射
		- controller创建新的view
	
- jqXHR 对象 
	- 所有的jQuery Ajax请求都会返回一个jqXHR对象来封装server发回的响应。jqXHR是MXMLHttpRequest对象的超集。
	- 这个对象可以用来处理不同的服务器响应：success， failure
	- jqXHR.done()[是成功]，jqXHR.fail()[是错误]，jqXHR.always(), 这三个构成一个try-catch-finally块。
	- example: 
		```
		$(document).ready(function(){
			$('#button').click(function(e){
				var parameters = {title: $('#title').val()};
				var jqxhr = $.get('revisionajax/getLatest', parameters);
				
				jqxhr.done(function(result){
					$('#results').html(result);
				});
				jqxhr.fail(function(result){
					$('#results').html("response status: " + jqxhr.status);
					//console.log("response status: " + jqxhr.status);
				});
			});
		});
		```
		
- 跨域问题
	- 跨域问题是线代浏览器
