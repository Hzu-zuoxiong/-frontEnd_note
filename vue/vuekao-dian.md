# Vue考点

* 组件之间的传值？

```
* 父组件与子组件传值
  父组件通过标签上定义传值
  子组件通过props方法接受数据

* 子组件想父组件传递数据
  子组件通过$emit方法传递参数
```

* `Vue.cli`中怎样使用自定义的组件？有遇到过哪些问题？

```
1.在components目录新建你的组件文件(index.vue)，script一定要export default {}
2.在需要用到组件的页面中导入：import index from '/component/index.vue'
3.注册到vue的components属性中，components: {index}
4.在template视图中使用<index></index>
```

* `v-show`和`v-if`指令的不同点？

```
v-show是通过修改元素的display的CSS属性让其显示或者隐藏
v-if是直接销毁和重建DOM达到让元素显示和隐藏
```

* 如何让`CSS`只在当前组件中起作用

```
将当前组件的<style>修改为<style scoped>
```

* `<keep-alive></keep-alive>`的作用是什么？

```
<keep-alive></keep-alive>包裹动态组件时，会缓存不活动的组件实例，主要用于保留组件状态或避免重新渲染
```

* `Vue`的生命周期

```
总共分为8格阶段：创建前/后、挂载前/后、更新前/后、销毁前/后
* 创建前/后：在beforeCreated阶段，vue实例的挂载元素el和数据对象data都为undefined，还未初始化。在created阶段，vue实例的
            数据对象data有了，el还没有。
* 载入前/后：在beforMount阶段，vue实例的$el和data都初始化，但还是挂载之前为虚拟DOM节点，data.message还未替换。在mounted阶
            段，vue实例挂载完成，data.message成功渲染。
* 更新前/后：当data变化时，会触发beforeUpdate和updated方法
* 销毁前/后：在执行destroy方法后，对data的改变不会再触发周期函数，说明此时vue实例已经解除了事件监听以及和DOM的绑定，但DOM结
            构依然存在
            
页面第一次加载时会触发beforeCreate、created、beforeMount、mounted这几个钩子
DOM渲染在mounted中就已经完成了
```

* 为什么避免`v-if`和`v-for`用在一起？

```
当Vue处理指令时，v-for比v-if具有更高的优先级，通过v-if移动到容器元素，不会再遍历列表中的每个值。也就是说不会在v-if为否的时候
运算v-for。
```



