# vue-flip-number

基于 Vue 2.x 的数字切换组件

## 安装

```
npm i vue-flip-number --save
```

## 使用

```vue
<template>
  <div>
    <flip :deadline="deadline" :length="length"></flip>
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
deadline:显示的数字
length：
# 参考

- [vuejs-countdown](https://github.com/getanwar/vuejs-countdown)
- [Demo for 'Flip clock & countdown, Vue'](https://codepen.io/shshaw/pen/BzObXp)
