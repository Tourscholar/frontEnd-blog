###  Vue生命周期
Vue实例需要经过创建、初始化数据、编译模板、挂载DOM、渲染、更新、卸载等一系列过程, 这个过程就是Vue的生命周期. 在Vue的整个生命周期中提供很多钩子函数在生命周期的不同时刻调用, Vue中提供给的钩子函数有```beforeCreate```、```created```、```beforeMount```、```mounted```、```beforeUpdate```、```update```、```beforeDestroy```、```destroyed```

在实例化Vue的过程中, 会直接触发的生命周期有```beforeCreate```、```created```、```beforeMount```、```mounted```, 在数据更新的过程中触发的生命周期有```beforeUpdate```、```updated```, 在销毁组件的过程中触发的生命周期有```beforeDestroy```、```destroyed```

```javascript
<!DOCTYPE html>
<html>
  <head>
    <title>Vue生命周期</title>
  </head>
  <body>
    <div id="app">
      <div>{{msg}}</div>
      <button @click="updateMsg">updateMsg</button>
      <button @click="destroyVue">destroyVue</button>
    </div>
  </body>
  <script src="https://cdn.bootcss.com/vue/2.4.2/vue.js"></script>
  <script type="text/javascript">
    var vm = new Vue({
      el: "#app",
      data: {
        msg: "Vue Lifecycle",
      },
      beforeCreate: function () {
        console.log("beforeCreate");
        console.log(this.$el); 		//undefined
        console.log(this.$data); 	//undefined
        console.log(this.msg); 		// undefined
        console.log("--------------------");
      },
      created: function () {
        console.log("created");
        console.log(this.$el); 		//undefined
        console.log(this.$data); 	//{__ob__: Observer}
        console.log(this.msg); 		// Vue Lifecycle
        console.log("--------------------");
      },
      beforeMount: function () {
        console.log("beforeMount");
        console.log(this.$el); 		//<div id="app">...</div>
        console.log(this.$data); 	// {__ob__: Observer}
        console.log(this.msg); 		// Vue Lifecycle
        console.log("--------------------");
      },
      mounted: function () {
        console.log("mounted");
        console.log(this.$el); 		//<div id="app">...</div>
        console.log(this.$data); 	//{__ob__: Observer}
        console.log(this.msg); 		// Vue Lifecycle
        console.log("--------------------");
      },
      beforeUpdate: function () {
        console.log("beforeUpdate");
        console.log(this.$el);
        console.log(this.$data);
        console.log(this.msg);
        debugger;
        console.log("--------------------");
      },
      updated: function () {
        console.log("updated");
        console.log(this.$el);
        console.log(this.$data);
        console.log(this.msg);
        console.log("--------------------");
      },
      beforeDestroy: function () {
        console.log("beforeDestroy");
        console.log(this.$el);
        console.log(this.$data);
        console.log(this.msg);
        console.log("--------------------");
      },
      destroyed: function () {
        console.log("destroyed");
        console.log(this.$el);
        console.log(this.$data);
        console.log(this.msg);
        console.log("--------------------");
      },
      methods: {
        updateMsg: function () {
          this.msg = "Vue Update";
        },
        destroyVue: function () {
          this.$destroy();
        },
      },
    });
  </script>
</html>
```

#### beforeCreate
从Vue实例开始创建到```beforeCreate```钩子执行的过程中主要进行了一些初始化操作, 例如组件的事件与生命周期钩子的初始化. 在此生命周期钩子执行时组件并未挂载, ```data```、```methods```等也并未绑定, 此时主要可以用来加载一些与Vue数据无关的操作, 例如展示一个```loading```
```javascript
console.log("beforeCreate");
console.log(this.$el);		// undefined
console.log(this.$data);	// undefined
console.log(this.msg);		// undefined
```

#### created
从```beforeCreate```到```created```的过程中主要完成了 数据绑定的配置、计算属性与方法的挂载、```watch/event```事件回调等. 在此生命周期钩子执行时, 组件未挂载到DOM, 属性```$el```仍然为```undefined```, 但此时已经可以开始操作```data```与```methods```, 只是页面还未渲染, 在此阶段通常用来发起一个Ajax请求
```javascript
console.log("created");
console.log(this.$el);		// undefined
console.log(this.$data);	// {_ob_: Observer}
console.log(this.msg);		// Vue Lifecycle
```

#### beforeMount
从```created```到```beforeMount```的过程中主要完成了页面模板的解析, 在内存中将页面的数据与指令等进行解析, 当页面解析完成, 页面模板就此存在于内存中. 在此生命周期钩子执行时```$el```被创建, 但是页面只是在内存中, 并未作为DOM渲染
```javascript
console.log("beforeMount");
console.log(this.$el);		// <div id="app"></div>
console.log(this.$data);	// {_ob_: Observer}
console.log(this.msg)l		// Vue Lifecycle
```

#### mounted
从```beforeMount```到```mount```的过程中执行的是将页面从内存中渲染到DOM的操作. 在此生命周期钩子执行时页面已经渲染完成, 组件正式完成创建阶段的最后的最后一个钩子, 即将进入运行阶段. 此外关于渲染页面模板的优先级是: ```render```函数 > ```template```函数 > 外外部HTML
```javascript
console.log("mounted");
console.log(this.$el);		// <div id="app"></div>
console.log(this.$data);	// {_ob_: Observer}
console.log(this.msg);		// Vue Lifecycle
```

#### beforeUpdate
当数据发送更新时```beforeUpdate```钩子便会调用, 此时Vue实例中数据已经是最新的, 但是在页面中的数据还是旧的, 在此可以进一步地更改状态, 并不会触发附加的渲染过程. 
```javascript
console.log("beforeUpdate");
console.log(this.$el);		// <div id="app"></div>
console.log(this.$data);	// {_ob_: Observer}
console.log(this.msg);		// Vue Update
debugger
```

#### updated
当数据发送更新并在DOM渲染完成后```update```钩子便会调用, 在此时组件的DOM已经更新, 可以执行依赖于DOM的操作
```javascript
console.log("updated");		// <div id="app"></div>
console.log(this.$el);		// {_ob_: Observer}
console.log(this.msg);		// Vue Update
```

#### beforeDestroy
在Vue实例被销毁之前```beforeDestory```钩子也会被调用, 在此时实例仍然完全可用
```javascript
console.log("beforeDestroy");
console.log(this.$el);		// <div id="app"></div>
console.log(this.$data);	// {_ob_: Observer}
console.log(this.msg);		// Vue Update
```

#### destroyed
在Vue实例被销毁之后```destroyed```钩子便会被调用, 在此时Vue实例绑定的所有东西都会解除绑定, 所有的事件监听器会被移除, 所有的子实例也会被销毁, 组件无法使用, ```data```和```methods```也都不可使用, 即使更改了实例的属性, 页面的DOM也不会重新渲染
```javascript
console.log("destroy");
console.log(this.$el);		// <div id="app"></div>
console.log(this.$data);	// {_ob_: Observer}
console.log(this.msg);		// Vue Update
```

