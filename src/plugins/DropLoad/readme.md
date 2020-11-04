# dropload 下拉加载插件

### 插件用法

该插件以指令 v-dropload="fun" 形式使用。
fun 为当滚动到底部时，需要调用的回调函数，通常在该回调函数中请求更多的数据。

---

### 原理

当指令所在元素插入到父元素节点时，会向上遍历（包括元素自己）出第一个 overflowY 为 auto 或者 scroll 的元素节点，并为该节点的 wheel 事件绑定侦听器
回调函数的处理为：当滚动到滚动元素的底部时，则调用回调函数 func

---

### 使用该插件的注意事项：

传给指令的回调函数 fun，需要返回一个 promise，因为在实现过程中，会通过 await fun() 等待 fun 的回调完成，以防止触发多次 fun 回调。

---