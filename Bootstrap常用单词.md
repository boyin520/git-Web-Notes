# Bootstrap常用单词

布局容器

    .container                         固定宽度

    .container-fluid                   全屏

    .row                               行

    .col-lg-                           大屏幕

    .col-md-                           中屏幕

    变量

    @grid-columns: 12                  列数

    @grid-gutter-width: 30px            槽宽

    @grid-float-breakpoint: 768px       媒体查询阙值(确定合适让列浮动)

    中心内容

    .lead                              段落突出显示

    内联文本元素

    <mark></mark>                      改变包含内容背景色

    <del></del><s></s>                 删除线

    <ins></ins><u></u>                  下划线

    <small></small>                     小号文本

    <strong></strong><b></b>           加粗

    <em></em><i></i>                   斜体

    文本对齐

    .text-left                         靠左

    .text-center                       居中

    .text-right                        靠右

    .text-justify                      文本对齐

    .text-nowrap                       不自动换行

    .text-lowercase                    文本小写

    .text-uppercase                    文本大写

    .text-capitalize                   首字母大写

    引用

    <blockquote></blockquote>          引用

    blockquote.blockquote-reverse      引用右对齐

    列表

    .list-unstyled                     移除默认样式

    .list-inline                       所有列表放同一行

    .dl-horizontal                     设置了浮动和偏移，水平排列

    代码

    <code></code>                      包裹内联样式的代码片段

    <kbd></kbd>                        标记用户通过键盘输入的内容

    .pre-scrollable                    使 <pre> 元素可滚动，代码块区域最大高度为340px

    变量

    <var></var>                        变量

    表格

    .table                             表格

    .table-striped                     条纹状表格

    .table-bordered                    带边框的表格

    .table-hover                       鼠标悬停

    .table-condensed                   紧缩表格

    .table-responsive                  响应式表格

    .success                           绿色

    .active                            灰色

    .info                              青色

    .warning                           黄色

    .danger                            红色

    表单

    .form-control                      向所有的文本元素 <input>、<textarea> 和 <select>

   .form-group                        表单

   .form-inline                       内联表单(向左对齐的，标签是并排的)

    <label for=""></label>             每个输入控件设置 label 标签，屏幕阅读器将正确识别

    .form-horizontal                   水平排列的表单

                text                   文本

                password               密码

                datetime               日期时间

                datatime-local         当地日期时间

                date                   日期

                month                  月

                time                   事件

    input       week                    周

                number                 数字

                email                  邮件

                url                    链接

                search                 搜索

                tel                    电话

                color                  颜色

                rows="3"               根据需要改变 rows 属性

    .checkbox                          多选框

    .radio                             单选框

    .checkbox-inline                   内联多选框

    .radio-inline                      内联单选框

    选择框(select)

    .form-control                      下拉菜单

    multiple="multiple"                允许用户选择多个选项

    .form-control-static               需要在一个水平表单内的表单标签后放置纯文本时，请在 <p> 上使用 class .form-control-static

    <fieldset></fieldset>              可以禁用 <fieldset> 中包含的所有控件

    .has-feedback                      为输入框添加额外的图标

    .form-control-feedback                 为输入框添加额外的图标子元素

    .sr-only                           隐藏表单控件的 <label> （而不是使用其它标签选项，如 aria-label 属性）， 一旦它被添加，Bootstrap 会自动调整图标的位置。

    按钮

    .btn                               按钮

    .btn-default                       默认样式

    .btn-primary                       原始按钮样式

    .btn-success                       绿色

    .btn-info                          青色

    .btn-warning                       黄色

    .btn-danger                        红色

    .btn-link                          链接式按钮

    .btn-block                         块级按钮(拉伸至父元素100%的宽度)

    .active                            激活状态

    .disabled                          禁用按钮

    图片

    .img-responsive                    响应式图片

    .img-rounded                       图片圆角

    .img-circle                        圆形图片

    .img-thumbnail                     缩略图/有边框

    辅助类

    .pull-left                         左浮动

    .pull-right                        右浮动

    .center-block                      水平居中

    .clearfix                          清除浮动

    .show                              强制元素显示

    .hidden                            强制元素隐藏

    .sr-only                           除了屏幕阅读器外，其他设备上隐藏元素

    .sr-only-focusable                 与 .sr-only 类结合使用，在元素获取焦点时显示(如：键盘操作的用户)

    .text-hide                         将页面元素所包含的文本内容替换为背景图

    .close                             显示关闭按钮

.caret                             显示下拉式功能

 

    下拉菜单

    .dropdown                              下拉菜单

    .dropdown-menu                         下拉选项

    .dropup                                上拉菜单

    .dropdown-toggle                       下拉菜单切换

    .dropdown-menu-right                   右对齐

    .dropdown-menu-left                    左对齐

    .dropdown-header                       标题

    .drivider                              分割线

    .disabled                              禁用项

    按钮组

    .btn-group                             按钮组

    .btn-toolbar                           多重按钮组

    .btn-group-vertical                    水平按钮组

    .btn-group-justified                   两端对齐排列的按钮组

    输入框组

    .input-group                           输入框组

    .input-group-addon                     单独一侧加控件

    .input-group-btn                       单独一侧加按钮

    导航元素

    .nav nav-tab                           标签页

    .nav nav-pills                         胶囊式标签页

    .nav nav-pills nav-stacked                胶囊式标签页以垂直方向堆叠排列的

    .nav-justified                         两端对齐的标签页

    .disabled                              禁用的标签页

    .tab-content                           与 .tab-pane 和 data-toggle="tab" (data-toggle="pill" ) 一同使用, 设置标签页对应的内容随标签的切换而更改

    .tab-pane                              与 .tab-content 和 data-toggle="tab" (data-toggle="pill")一同使用, 设置标签页对应的内容随标签的切换而更改

    导航条

    .navbar                               

    .navbar-defalut                        默认样式

    .navbar-toggle                         响应式导航栏

    .narbar-header                         导航栏头部

    .narbar-brand                          导航栏图标

    .navbar-form                           导航栏表单

    .navbar-btn                            导航栏按钮

    .navbar-text                           导航栏文本

    .navbar-link                           非导航的链接

    .navbar-left                           左对齐

    .navbar-right                          右对齐

    .navbar-fixed-top                      固定在顶部

    .navbar-fixed-botton                   固定在底部

    .navbar-static-top                     静态的顶部

    .navbar-inverse                        反色导航栏

    分页

    .pagination                           

    .disabled                              定义不可点击的链接

    .active                                指示当前的页面

    翻页

    .pager

    .previous                              上一页左对齐

    .next                                  下一页右对齐

    .disabled                              禁用

    面包屑导航

    .breadrumb                            

    标签

    .lable lable-default                   默认灰色标签

    .lable lable-primary                   蓝色标签

    .lable lable-success                   绿色标签

    .lable lable-info                      浅蓝色标签

    .lable lable-warning                   黄色标签

    .lable lable-danger                    红色标签

    徽章

    .badge                                

    巨幕

    .jumbotron                        

    页头

    .page-header                          

    缩略图

    .thumbnail                            

    警告框

    .alert alert-sccess                    绿色警告框

    .alert alert-info                      浅蓝色警告框

    .alert alert-warning                   黄色警告框

    .alert alert-danger                    红色警告框

    .alert-dismissible                     可关闭的警告框

    进度条

    .progress                             

    .progress-bar                          显示进度百分比

    .progress-bar-striped                  条纹进度条

    多媒体对象

    .media

    .media-left                            媒体左边内容

    .media-object                          媒体对象

    .media-body                            媒体主体

    .media-heading                         媒体标题

    .media-list                            媒体对象列表

    列表组

    .list-group

    .list-group-item                       列表组子元素

    面版

    .panel

    .panel-default                         默认面版

    .panel-body                            面版主体

    .panel-heading                         面版标题

    .panel-footer                          面版脚注

    嵌入内容

    .embed-responsive                     

.embed-responsive-item                    嵌入内容子元素
