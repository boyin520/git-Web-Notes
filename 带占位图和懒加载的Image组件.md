# 带占位图和懒加载的Image组件

```
<template>
  <div class="cp-image" :style="_boxStyle">
    <img
      class="image-main"
      :src="_src"
      :mode="mode"
      :style="_boxStyle"
      :lazy-load="lazy"
      @click="handleClick"
      @load="handleLoad"
      @error="handleError"
    />
    <div class="loading" v-if="loading">
      <img src="/static/images/base/common/image_loading.png" class="img-loading" mode="aspectFit" />
    </div>
    <div class="error" v-if="error">
      <img src="/static/images/base/common/image_error.png" class="img-error" mode="aspectFit" />
    </div>
  </div>
</template>
 
<script>
exportdefault {
  props: {
    src: {
      type: String,
      required:true
    },
    mode: {
      type: String,
      default:'aspectFill'
    },
    width: {
      type: Number
    },
    height: {
      type: Number
    },
    radius: {
      type: Number
    },
    lazy: {
      type: Boolean,
      default:false
    }
  },
  computed: {

// 此方法是为了服务端压缩图片，只有我的项目里需要

    _src:function() {
      let _src
      let src =this.src
      // 微信临时图片和已处理图片，不处理
      if (!src || src.indexOf('tmp/') > -1 || src.indexOf('ashx') > -1 || src.indexOf('tmp_') > -1) {
        return src
      }
      let dpr =this._sv.Base.getSystemInfo().pixelRatio
      let args ='?format=jpg&mode=high&stretch=uniform&compress=60'
      if (this.width) {
        args += `&width=${Math.round(dpr *this.width)}`
      }else {
        args += `&width=0`
      }
      if (this.height) {
        args += `&height=${Math.round(dpr *this.height)}`
      }else {
        args += `&height=0`
      }
      if (!src) {
        _src =''
      }else if (src.indexOf('?') > -1) {
        _src = src.split('?'[0]) + args
      }else {
        _src = src + args
      }
      return _src
    },

// 计算盒子和图片的样式

    _boxStyle:function() {
      let styles = {}
      if (this.width) {
        styles.width =this.width * 2 +'rpx'
      }
      if (this.height) {
        styles.height =this.height * 2 +'rpx'
      }
      if (this.radius) {
        styles.borderRadius =this.radius * 2 +'rpx'
      }
      return this._util.Base.styleObj2String(styles)
    }
  },
  data() {
    return {

// 关键，loading需要先置为true

      loading:true,
      error:false
    }
  },
  onLoad() {
    if (!this.lazy) {
      this.loading =true
    }
  },
  methods: {
    handleClick(e) {
      this.$emit('click', e)
    },
 
    handleLoad(e) {
      this.loading =false
      this.error =false
    },
 
    handleError() {
      this.loading =false
      this.error =true
    }
  }
}
</script>
 
<style lang='scss' scoped>
.cp-image {
  position: relative;
  display: inline-block;
  overflow: hidden;
}
.loading,
.error {
  @include center();
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: $color-bg-base;
  .img-loading,
  .img-error {
    height: 32%;
  }
}
</style>

// mpvue的页面内使用

import image from 'components/project/image
components: {
'cp-image': image
}
<cp-image :src="src" :width="90" :height="60" :radius="4"></cp-image>
```

