### **常见class关键词：**

- 布局类：header, footer, container, main, content, aside, page, section
- 包裹类：wrap, inner
- 区块类：region, block, box
- 结构类：hd, bd, ft, top, bottom, left, right, middle, col, row, grid, span
- 列表类：list, item, field
- 主次类：primary, secondary, sub, minor
- 大小类：s, m, l, xl, large, small
- 状态类：active, current, checked, hover, fail, success, warn, error, on, off
- 导航类：nav, prev, next, breadcrumb, forward, back, indicator, paging, first, last(

1. 导航：nav
2. 主导航：mainbav
3. 子导航：subnav
4. 顶导航：topnav
5. 边导航：sidebar
6. 左导航：leftsidebar
7.  右导航：rightsidebar
8. 菜单：menu
9. 子菜单：submenu
10. 标题: title
11.  摘要: summary)

- 交互类：tips, alert, modal, pop, panel, tabs, accordion, slide, scroll, overlay,
- 星级类：rate, star
- 分割类：group, seperate, divider
- 等分类：full, half, third, quarter
- 表格类：table, tr, td, cell, row
- 图片类：img, thumbnail, original, album, gallery
- 语言类：cn, en
- 论坛类：forum, bbs, topic, post
- 方向类：up, down, left, right
- 功能

1. \- 标志：logo
2. \- 广告：banner
3. \- 登陆：login
4. \- 登录条：loginbar
5. \- 注册：regsiter
6. \- 搜索：search
7. \- 功能区：shop
8. \- 标题：title
9. \- 加入：joinus
10. \- 状态：status
11. \- 按钮：btn
12. \- 滚动：scroll
13. \- 标签页：tab
14. \- 文章列表：list
15. \- 提示信息：msg
16. \- 当前的: current
17. \- 小技巧：tips
18. \- 图标: icon
19. \- 注释：note
20. \- 指南：guild
21. \- 服务：service
22. \- 热点：hot
23. \- 新闻：news
24. \- 下载：download
25. \- 投票：vote
26. \- 合作伙伴：partner
27. \- 友情链接：link
28. \- 版权：copyright

- 其他语义类：btn, close, ok, cancel, switch; link, title, info, intro, more, icon; form, label, search, contact, phone, date, email, user; view, loading...
- *例如**：**text改**为**t**xt**、delete改**为**del，**这里**针**对**单个**单词**组合**命名，**对**词组**单词的**组合**建议**不使用**缩写**或简写*。

| 单词       | 缩写 | 说明           |
| ---------- | ---- | -------------- |
| bottom     | btm  | 底部           |
| button     | btn  | 按钮           |
| background | bg   | 背景           |
| content    | cont | 内容           |
| check      | chk  | 选择框         |
| current    | curr | 当前的         |
| delete     | del  | 删除           |
| text       | txt  | 文本           |
| disabled   | dis  | 禁用           |
| foot       | ft   | 底部           |
| head       | hd   | 头部           |
| hidden     | hide | 隐藏           |
| input      | inp  | input框        |
| image      | img  | 图片           |
| index      | idx  | 索引           |
| message    | msg  | 消息           |
| password   | pwd  | 密码           |
| previous   | prev | 前面的、上一面 |
| radio      | rad  | 单选           |
| register   | reg  | 注册           |
| select     | sel  | 选择           |
| tbody      | tbd  | 表格主体       |
| thead      | thd  | 表格头部       |
| tfoot      | tft  | 表格底部       |
| wrap       | wp   | 包装，外层     |

 

**块****（Block）的命名规范**

1. 块的名称是唯一的
2. 块的名称和块的功能一致
3. *如**：菜单应该命名为**menu*
4.  块只能用Class选择器，不能用ID选择器，因为同一个块可能出现在页面的多个地方
5. 块的内部是可以在包含多个子块的
6. 块名称用小写命名

 

| 类型       | 块名           | 类型     | 块名        |
| ---------- | -------------- | -------- | ----------- |
| 顶部       | topbar         | 登录     | login       |
| 快速导航   | quickmenu      | 菜单     | menu        |
| 导航       | nav            | 搜索框   | searchbox   |
| 关键字     | keywords       | 左边栏   | leftside    |
| 右边栏     | rightside      | 内容     | content     |
| 左、右菜单 | left/rightmenu | 服务链接 | servicelink |
| 服务       | service        | 底栏     | footerbar   |
| 版权       | copyright      | 注册     | register    |
| 新闻       | new            | 新闻列表 | news        |
| 列表项     | item           | 列表集合 | lists       |

 

**元素****（Element）的命名规范**

1. 元素的命名使用块名+元素名的组合方式，之间以中划线(-)隔开。
2. *如**：块名**-元素**名*
3. 元素的命名只能用于Class选择器
4. 如：菜单项的元素命名为menu-item
5. 使用小写命名

 

| 类型     | 元素名  | 类型     | 元素名 |
| -------- | ------- | -------- | ------ |
| 元素项   | -item   | 元素头部 | -hd    |
| 元素标题 | -title  | 元素内容 | -cont  |
| 元素底部 | -btm    | 元素顶部 | -top   |
| 元素中部 | -middle | 元素右则 | -right |
| 元素左则 | -left   |          |        |

### 制定简单规则：

- 以中划线连接，如`.item-img`
- 使用两个中划线表示特殊化，如`.item-img.item-img--small`表示在`.item-img`的基础上特殊化
- 状态类直接使用单词，参考上面的关键词，如`.active, .checked`
- 图标以`icon-`为前缀（字体图标采用`.icon-font.i-name`方式命名）。
- 模块采用关键词命名，如`.slide, .modal, .tips, .tabs`，特殊化采用上面两个中划线表示，如`.imgslide--full, .modal--pay, .tips--up, .tabs--simple`
- js操作的类统一加上`js-`前缀
- 不要超过四个class组合使用，如`.a.b.c.d                                                  `

**修饰关键词**

- 以header为例，我们可以添加前缀表示不同的header，如区块头部`.block-hd`(hd为header简写)，modal头部`.modal-hd`，文章头部`.article-hd`。

- 同样标题也可以分为，页面标题`.page-tt`(title的简写)，区块标题`.block-tt`等

- 样式修饰符：块或元素命名加上样式修饰符，之间用中划线(-)隔开

  *例：**块或元素**-样式**修饰符*

-  样式修饰符的命名只能用于Class选择器

-  使用小写命名  

  如：某个按钮的宽度加宽，则该按钮的样式修饰符名为long，全名就为：ui-btn-long

   

  | 类型           | 修饰符名       | 类型           | 修饰符名         |
  | -------------- | -------------- | -------------- | ---------------- |
  | 无上边框       | nobt           | 无右边框       | nobr             |
  | 无下边框       | nobb           | 无左边框       | nobl             |
  | 无上（内）边框 | nopt           | 无右（内）边框 | nopr             |
  | 无下（内）边框 | nopb           | 无左（内）边框 | nopl             |
  | 无上（内）外框 | nomt           | 无右（内）外框 | nomr             |
  | 无下（内）外框 | nomb           | 无左（内）外框 | noml             |
  | （内）上边框   | pt-10（像素）  | （内）右边框   | pr-10（像素）    |
  | （内）下边框   | pb-10（像素）  | （内）左边框   | pb-10（像素）    |
  | （外）上边框   | mt-10（像素）  | （外）右边框   | mr-10（像素）    |
  | （外）下边框   | mb-10（像素）  | （外）左边框   | mb-10（像素）    |
  | 字体颜色       | fc-red（颜色） | 字体类型       | fm-arial（类型） |
  | 字体大小       | fs-12（大小）  | 背景颜色       | bg-red（颜色）   |
  | 字体加粗       | fw-bold        | 正常字体       | fw-normal        |
  | 文字下划线     | txt-underline  | 文字中划线     | txt-through      |
  | 文字居左       | txt-l          | 文字居右       | txt-r            |
  | 文字垂直居上   | txt-t          | 文字垂直居下   | txt-b            |
  | 文字居中       | txt-c          | 文字垂直居中   | txt-m            |
  | 绝对定位       | pos-abs        | 相对定位       | pos-rel          |
  | 宽度           | w-10（像素）   | 高度           | h-10（像素）     |
  | 行高           | lh-12（像素）  | 文本缩进       | txt-in           |
  | 边框宽度       | bw-2（像素）   | 上边框宽度     | btw-2（像素）    |
  | 下边框宽度     | bbw-2（像素）  | 左边框宽度     | blw-2（像素）    |
  | 右边框宽度     | brw-2（像素）  |                |                  |
  | 减短           | -short         | 加长           | -long            |
  | 变大           | -big           | 缩小           | -small           |
  | 向上           | -up            | 向下           | -down            |
  | 向左           | -left          | 向右           | -right           |
  | 向前，上一个   | -prev          | 向后，下一个   | -next            |
  | 低级           | level-low      | 中级           | level-middle     |
  | 高级           | level-high     |                |                  |

   

 

### 行为修饰符

- 块名或元素名加上行为修饰符，之间用中划线(-)隔开。
- *块**或元素名**-行**为修饰符*
- 行为修饰符的命名只能用Class选择器
- 使用小写命名

如：修饰按钮在鼠标经过的事件，鼠标经过行为修饰符用-hover，所以全名为：ui-btn-hover

| 类型               | 修饰符名  | 类型           | 修饰符名   |
| ------------------ | --------- | -------------- | ---------- |
| 鼠标经过           | -hover    | 获取焦点       | -focus     |
| 失去焦点           | -blur     | 鼠标按下       | -mousedown |
| 键盘按下           | -keydown  | 鼠标拖动       | -drag      |
| 不可用、禁用、只读 | -disabled | 可用、启用     | -enabled   |
| 选中（下拉框）     | -selected | 选中（选择框） | -checked   |
| 成功               | -success  | 失败           | -fail      |
| 错误               | -err      | 警告           | -warning   |
| 当前状态           | -current  | 显示           | -show      |
| 隐藏               | -hide     | 添加           | -add       |
| 删除               | -del      | 编辑           | -edit      |
| 阅读、视图         | -view     | 返回           | -back      |
| 通过               | -pass     |                |            |