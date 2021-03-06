### Vue实现组件信息的缓存

当我们在开发vue的项目过程中，避免不了在路由切换到其他component再返回后该组件数据会重新加载， 
处理这种情况我们就需要用到keep-alive来缓存vue的组件信息，使其不再重新加载。

在app.vue里
```
<keep-alive>
    <router-view></router-view>
</keep-alive>
```
1
2
3
但是这种情况会对所有的组件进行缓存，不能达到单个组件缓存的效果。

那么我们给部分组件加上，实现方法如下：

在app.vue里
```
<!-- 这里是需要keepalive的 -->
<keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>

<!-- 这里不会被keepalive -->
<router-view v-if="!$route.meta.keepAlive"></router-view>
```
然后在设置路由信息的时候这样
```
{
  path: '',
  name: '',
  component: ,
  meta: {keepAlive: true} // 这个是需要keepalive的
},
{
  path: '',
  name: '',
  component: ,
  meta: {keepAlive: false} // 这是不会被keepalive的
}
```
这就实现部分组件缓存的功能 
如果缓存的组件想要清空数据或者执行初始化方法，在加载组件的时候调用activated钩子函数，如下：
```
activated: function () {
    this.data = '';
}
```