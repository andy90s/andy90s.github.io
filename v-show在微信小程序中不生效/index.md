# v-show在微信小程序中不生效

<!--more-->
## v-show在微信小程序中不生效
```css
<style lang="scss" scoped>
/* #ifdef MP-WEIXIN */
view[hidden] {
  display: none !important;
}
/* #endif */
</style>
```
