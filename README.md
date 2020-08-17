# task-03-02
1、请简述 Vue 首次渲染的过程。
    首次渲染之前首先进行Vue初始化，初始化Vue中的实例成员和静态成员。初始化结束之后调用Vue的构造函数，构造函数中调用了this._init()方法，这个方法中最终调用了$Mount方法。entry-runtime-with-compiler文件中的$mount方法先判断用户是否传统render,如果没有传递则将模板编译成render方法。render方法编译好之后将render方法存入options.render中。
    然后再调用runtime/index.js中的$mount方法,再调用mountComponent方法，触发beforeMount和定义updataComponent,在updataComponent中调用render()生成虚拟dom，update方法将虚拟dom转换成真实dom.然后再创建watcher对象，watcher对象中调用get()方法，接着触发mountd方法并返回vm



2、请简述 Vue 响应式原理。
    响应式先从Vue实例中的init()方法开始,init方法中先调用initState()方法初始化Vue实例的状态，这个方法中调用initData方法，将data属性注入到Vue实例中。并调用Observe()方法将data转化为响应式对象。
    observe方法先判断value是否是对象，不是对象直接返回，在判断对象是否有_ob_,若有则直接返回，若没有则创建observe对象并返回。
    observe类中给value对象定义不可枚举的_ob_属性记录当前observe对象，然后对数组和对象分别做响应化处理，数组响应化处理是更改原生的几个特殊方法，并遍历数组中每一个成员，给每一个成员调用observe方法，对象中调用walk方法遍历每一个属性，为每一个属性调用defineReactive。
    defineReactive为每一个属性创建dep对象用来收集依赖，defineReactive核心是定义getter和setter,getter中收集依赖，setter中发送通知。
    依赖收集的方式：在watcher对象的get方法中调用pushtarget记录Dep.target属性。访问data中成员的时候会触发defineReactive中的getter，并在getter中完成依赖收集的过程。
    把属性对应的watcher对象添加到dep对象的subs数组中。当数据变化时，会调用dep.notify方法调用watcher对象的updata方法，若wacher未被处理的话，将任务添加到queue对象中进行处理

3、请简述虚拟 DOM 中 Key 的作用和好处。
    

4、请简述 Vue 中模板编译的过程。