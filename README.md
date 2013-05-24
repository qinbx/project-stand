project-stand (项目规范)
========================

描述：为个人前端项目规范。

* 需求背景
* 指导思路
* 目录结构设置、目录命名、文件命名
* CSS编码规范
* Javascript编码规范

##需求背景##
目前个人接触的以jquery为基础的小型、小中型web项目的前端，多包含以下资源：

* js工具库。
>     jQuery,jqueryUI
>     jQuery组件。form validate，datatable。
>     其它工具库。如cookie。

* css模板和组件。
>     reset.css, normerniz.css
>     icon-font.css
>     jqueryui, bootstrap等

* 业务相关js
>     app config, app init
>     具体业务逻辑js。有的多页面共用，有的仅某个页面使用。

* 业务相关css
>     基础css。如font,table,按钮等。
>     覆盖其它组件默认风格的css。
>     具体页面布局和细节实现的css。

* 图片
>     修饰型图片。如背景图片、图标。
>     内容型图片。如logo、新闻图片、产品图片。

* 其它
>     web font字体文件
>     less,sass源文件
>     js测试文件
>     ...

 
##规范目标和指导思路##
规范目标
>     - 让代码组织结构明了、一目了然。增强可读性和可维护性。
>     - 为进一步优化打好基础。比如js和css文件的压缩、合并、更新。

指导思路
>     - 按文件的作用域划分为:多站共用，单站共用，多页面共用，单页面使用几个级别。
>     - 工具代码文件和业务代码文件分开。

##目录结构设置、目录命名、文件命名##
纠结点：单词连接用不用下划线或中划线。


目录和文件命名
>     以字母打头，可以包含字母、数字、下划线。
>     避免拼音，尽量用简单易懂的英文单词或英文单词简写。
>     避免用大写字母。？？？

变量属性命名
>     javascript类以大驼峰式命名。如MyTestClass
>     javascript其它变量以小驼峰式命名，必要时可用下划线连接单词或单词组。如myTestClass。
>     css类尽量用小写字母。
>     html属性必须用小写字母。如data-v。
>     css,html元素ID尽量用小写字母。

示例：

    |- static/
    |    |- js_lib/   jquery, jquery.formvalidate.js
    |    |- css_lib/  normerniz.css, icon-font.css, anamination.css
    |    |- js/     app_config.js, page_index.js,pgaes_prodct.js
    |    |- css/    basic_style.css
    |    |- img/     修饰型图片
    |    |- images/  内容型图片
    |    |- pugin_datatable/   datatable.js, datatable.css, img
    |    |- plugin_highchart/   highchart.js, flash..
    |    |- font/      
    |    |- less
    |- index.html
    |- product_list.htm
    |- product_detail.htm
    |- product_search.htm
    |- admin/
        |- login.html
        |- main.html
        |- product_manage.htm

##编码规范##
