# 异步函数调用this找不到方法

<!--more-->
## 异步函数调用this找不到方法
that 一下;
```js
const that = this;

promise {
  then(res => {
    that.xxx();
  })
}
```
