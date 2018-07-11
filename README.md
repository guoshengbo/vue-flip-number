# vue2-flip-countdown

基于 Vue 2.x 的数字切换组件

## 安装

```
npm i vue-flip-number --save
```

## 使用

```vue
<template>
  <div>
    <flip :deadline="99999" :length="length"></flip>
  </div>
</template>

<script>
  import Flip from 'vue-flip-number'

  export default {
      data () {
        deadline: 112321322,
        length: 10
      }
      components: { Flip }
  }
</script>
```
# 参考

- [vuejs-countdown](https://github.com/getanwar/vuejs-countdown)
- [Demo for 'Flip clock & countdown, Vue'](https://codepen.io/shshaw/pen/BzObXp)
