##### vue+vue-loader v-for当数据为空数组时显示为 undefined



<template lang="pug">
    div
        span 测试 //-- 这里不能少
        div(v-for="idx in []") {{idx}} 
</template>

当组件渲染后 显示的结果为按理说应该时 “测试”
但是实际显示结果为 “测试undefined”

解决方法
在v-for 外面套一个div 如：

<template lang="pug">
    div
        span 测试 //-- 这里不能少
        div
            div(v-for="idx in []") {{idx}} 
</template>

原因分析
1、他生成的render函数如下：

var render = function() {
  var _vm = this
  var _h = _vm.$createElement
  var _c = _vm._self._c || _h
  return _c(
    "div",
    [
      _c("span", [_vm._v("测试")]),
   ① _vm._l([], function(idx) { 
        return _c("div")
      })
    ],
    2
  )
}

① 这个位置 会渲染为一个undefined textNode
_c 这个函数的第二个参数这里表示的是div的子元素
div 的子元素为：span和一个空素组。vue会把这个空素组渲染成一个undefined

如果我加一个的div 在v-for外面 生成的代码是

var render = function() {
  var _vm = this
  var _h = _vm.$createElement
  var _c = _vm._self._c || _h
  return _c("div", [
    _c("span", [_vm._v("测试")]),
    _c(
      "div",
      _vm._l([], function(idx) {
        return _c("div")
      })
    )
  ])
}

c 这个函数的第二个参数这里表示的是div的子元素
第一个div的子元素是：span 和 div
地二个子元素是：其实是一个空数组所以代表没有子元素那么当然不会渲染任何东西

这点代会返回一个空数组
vm._l 是vue的renderList函数

_vm._l([], function(idx) {
    return _c("div")

