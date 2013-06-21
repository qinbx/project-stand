##（1）js,css,html代码分离（松耦合）##

**各自的本职**：

- html：内容描述
- css: 外观呈现的描述
- javascript: 交互

```
// 不好的例子: html中夹杂着javascript
<input type="text" onchange="changeName()" />
<button onclick="submitAddTagForm()">Submit</button>

// 好的写法：原生javascript绑定事件
var btn = document.getElementById('action-btn');
btn.addEventListener("click", doSomething, false);

//好的写法：jQuery写法
$('#action-btn').click(doSomething);
```

```
// 不好的写法：css中包含javascript 表达式
.box {
    width: expression(document.body.offsetWidth + "px");
}
```
```
// 不好的写法：javascript中包含css
var errorDiv = document.getElementById('errorDiv');
errorDiv.style.color = "red";
errorDiv.style.visibility = "visible";
errorDiv.style.cssText = "color:red; visibility:hidden";

// 好的写法：通过css定义样式，用js给元素添加class来实现效果
var errorDiv = document.getElementById('errorDiv');
errorDiv.className += "errorDivcls";
```
```
// 不好的写法：javascript中包含html
var errorDiv = document.getElementById('errorDialog');
errorDiv.innerHTML = "<h3>ERROR</h3><p>Invalid e-mail address</p>";

// 好的做法：
// 1. 将内容html放在隐藏层里
<div id="tpl-errorDialog"><h3>ERROR</h3><p>Invalid e-mail address</p></div> // html
$('#errorDialog').html( $('#tpl-errorDialog').html() );  // javascript

// 2. 将内容html放在特殊标签里
<script type="text/tml" id="tpl-errorDialog">
    <h3>ERROR</h3><p>Invalid e-mail address</p>
</script>
   $('#errorDialog').html( $('#tpl-errorDialog').html() );  // javascript

//3. 通过ajax从服务器端请求html
    $('#errorDialog').load("/popup-add-keword.action");

// 上面的1)和2)涉及到复杂数据时，可考虑用Javascript模板引擎（如，Handlebars）
```

基于这个思路，在jsp页面中书写页面级css和javascript也是不推荐的。推荐页面链入单独的css文件和javascript文件。


##（2）尽量少地使用全局变量##
全局变量多容易导致混乱、命名冲突等问题。

两种比较好的处理思路：模块化(如YUI,AMD,REQUIR.js)和单全局变量方式(如jQuery)
###模块化因研究不多，暂不介绍。###
###单全局变量方式###
不太好的示例：  
/pages/widget/keywordRankTrendTableWidget.jsp

好的示例：  
/pages/widget/keywordTagComparisonWidget.jsp

//立即执行函数
```
//立即执行函数。可以做到零全局变量，定义后立即执行。
(function(){
    // code here
})();

var args= (function(){
    var arg = {
        date1: 1,
        date2: 2,
        argItem: 3
    };

    // 这里可以执行一些处理代码，如格式验证、判断大小。如下代码可以有逻辑错误，仅演示用。
    arg.date1 = Math.min(date1,date2);
    arg.date1 = Math.max(date1,date2);

   return arg;    
})();

// jQuery的基本思路
(function(){
    // 构造函数
    var $ = function() {
         // code
    };
    // 类方法
    $.prototype.addClass = function(str){
         // code
    };

    // 静态方法
    $.ajax = function(arg) {

    }
    
    // 把变量暴露给外部  
    window.jQuery = window.$ = $;
})();
```

###（3）根据作用域的代码文件组织思路###
作用域，即影响域。分为：

* **多个项目间共用的代码**。sites lever。如：
 - jquery, jQuery插件，datatable。
 - 自己定义的一些工具库。isEmail()等方法集。
* **单个项目间公共的代码**。site lever, app lever。如 
 - 项目的配置信息。如当前app相对根目录的路径(/saas/)、日期格式类型(yy/dd/mm，因为每个项目不一样)。
 - 整站都可能会用到的业务逻辑公共函数、自定义页面组件。isDateStr(), showDialog()
 - 整站公共的组件、jquery插件初始化方法。
* **频道、多个页面公共的代码**。chanel lever, pages lever
* **单个页面使用的代码**。 page lever.

```
static\
- sites\
     - js\
     - css\
     - img
- site\
     - js\
     - css\
     - img  
- pages\
     - js\
     - css\
     - img
- page\
     - js\
     - css\
     - img
```

考虑到合并、压缩js,css文件的话，可以将同作用域的文件压缩，这么引用。  
```
//index.html
<link rel="stylesheet" href="static/sites/release/pkg.css" />
<link rel="stylesheet" href="static/site/release/pkg.css" />
<link rel="stylesheet" href="static/page/css/index.css" />

<script src="static/sites/release/pkg.js"></script>
<script src="static/site/release/pkg.js"></script> 
<script src="static/page/js/login.js"></script>

//shopping_cart.html
<link rel="stylesheet" href="static/sites/release/pkg.css" />
<link rel="stylesheet" href="static/site/release/pkg.css" />
<link rel="stylesheet" href="static/pages/css/cart.css" />

<script src="static/sites/release/pkg.js"></script>
<script src="static/site/release/pkg.js"></script>  
<script src="static/page/js/cart.js"></script>

```

