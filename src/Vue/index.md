### 1、v-for 和 v-if 可以混合使用吗？为什么？

**不可以**，`v-for`计算优先级比`v-if`高，首先会把虚拟节点渲染出来，然后再进行`v-if`判断。降低渲染性能

### 2、v-for 中为什么加 key？

如果不使用 `key`，`Vue` 会使用一种最大限度减少动态元素并且尽可能的尝试`就地修改/复用`相同类型元素的算法。**key** 是为 `Vue` 中 `vnode` 的唯一标记，通过这个 `key`，`diff` 算法可以**更准确、更快速**<br>

- **更准确**：因为带 `key` 就不是就地复用了，在 `sameNode` 函数 a.key === b.key 对比中可以避免就地复用的情况。所以会更加准确。
- **更快速**：利用 `key` 的唯一性生成 `map` 对象来获取对应节点，比遍历方式更快

### 3、事件默认有个 event 参数，它是什么？怎么使用？事件被绑定到哪里？

当事件没有参数，则默认有个 `event` 参数；如果有自定义参数，则需要使用`$event` 传过去。

- `event` 的构造函数是 `MouseEvent`，即是原生的 `event` 对象
- `event` 被挂载到当前元素下，即 `event.target`

### 4、vue 父子组件如何通讯？

- **props、$emit**：<br>
  父组件使用动态数据传递，子组件使用`props`接收，可以使用数组/对象数据结构，对象结构可以定义类型和默认值。<br>
  子组件使用`$emit` 事件回传<br>
- **自定义事件进行组件通讯**：<br>
  自定义事件就是使用一个额外的 `js` 文件，其中声明一个 `Vue` 实例即可<br>
  `$on`，`$emit`，`$off`，参数分别是：注册函数名，真实函数<br>
  **$on**：绑定自定义事件<br>
  **$emit**：调用自定义事件<br>
  **$off**：在组件销毁时，要及时销毁自定义事件，否则可能会造成内存泄漏。在 `beforeDestroy` 中调用`$off`<br>
- **$refs**：<br>
  获取当前组件实例
- **$parent&$children**：<br>
  获取当前组件的父组件和子组件
- **vuex**：<br>
  Vue 中全局状态管理系统，用于多个组件中数据共享。
- **provide&inject**：<br>
  上层组件提供，下层组件注入使用。（适用于组件库编写）

### 5、父子组件声明周期调用顺序？

- **加载渲染过程**：<br>
  父`beforeCreate` -> 父`created` -> 父`beforeMount` -> 子`beforeCreate` -> 子`created` -> 子`beforeMount` -> 子`mounted` -> 父`mounted`
- **更新过程**：<br>
  父`beforeUpdate` -> 子`beforeUpdate` -> 子`updated` -> 父`updated`
- **销毁过程**：<br>
  父`beforeDestroy` -> 子`beforeDestroy` -> 子`destroyed` -> 父`destroyed`
- **全流程**：<br>
  `beforeCreate` -> `created` -> `beforeMount` -> `mounted` -> `beforeUpdate` -> `updated` -> `beforeDestroy` -> `destroyed`