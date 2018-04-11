# MarkdownPad 2导出HTML自动生成目录 #

## 功能说明 ##

实现功能：添加js和css后能使导出的HTML文档自动生成目录；

生成目录规则：以h2标签为一级标题，h3为二级标题。

## 代码注释 ##

### css ###
在MarkdownPad 2自带的一个主题上做二次修改，添加几个自定义样式；

### JS ###

为方便遍历元素同时能支持离线浏览，故在js里面直接加入jQuery v1.11.0，jq代码不做注释，下面讲解下我添加的代码：

	<script>
		$(window).ready(function(e){
			//菜单标签变量
			var menu = '';
			//遍历body里面的元素
			$('body').children().each(function(index){
				//给元素加上ID
				$(this).attr('ID','ele'+index);
				//以h2标签为目录标题
				if($(this).get(0).tagName=='H2'){
					//如果不是第一个h2则添加有序标签结尾
					if(str != ''){
						str += '</ol>';
					}
					//在菜单中添加一个标题
					menu += '<label onclick="jumpTo(\'ele'+index+'\')"><strong>'+$(this).text()+'</strong></label><ol>';
				}else if($(this).get(0).tagName=='H3'){
					//在菜单中增加一个子节点
					menu += '<li onclick="jumpTo(\'ele'+index+'\')">'+$(this).text()+'</li>';
				}
			});
			str += '</ol>';
			//保存body中现有的标签
			var eles = $('body').html();
			//清空body
			$('body').empty();
			//目录初始元素
			var initHtml = '';
			initHtml += '<div class="hs5233-fixed">';
			initHtml += '	<div class="hs5233-menu">';
			initHtml += '		<h2>目录</h2>';
			initHtml += '	</div> ';
			initHtml += '</div>';
			initHtml += '<div class="hs5233-content"></div>';
			//body中插入初始元素
			$('body').html(initHtml);
			//输出目录
			$('.hs5233-menu').append(str);
			//输出内容
			$('.hs5233-content').html(eles);
		});
		//点击菜单则跳转到对应元素的ID
		function jumpTo(eleID){
			$(window).scrollTop($('#'+eleID).offset().top);
		}
	</script>

## 使用方法 ##

### 添加css ###

点击【工具】->【选项】->【样式表】->【添加】，将css代码复制进去，保存并关闭。

### 添加JS ###

点击【工具】->【选项】->【高级】->【HTML Head编辑器】，将js代码复制进去，保存并关闭。
