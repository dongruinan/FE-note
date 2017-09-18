---
title: Vue入门
---

# 1.Vue的安装

* 使用cdn
	
 		http://cdn.jsdelivr.net/vue/1.0.26/vue.min.js

* 通过npm安装
* 
		$ npm install vue
* 通过bower安装

		$ bower install vue
# 2.初识Vue

* 2.1 hello world

		<div class="app">
		    {{message}}
		</div>
		<script src="vue.js"></script>
		<script>
		    var vm = new Vue({
		        el:'.app',
		        data:{
		            message:'hello world'
		        }
		    });
		</script> 

* 2.2 双向数据绑定v-model
	
		<div class="app">
  		  {{message}}+<input type="text" v-model="message">
		</div>

* 2.3 绑定表达式
	
	+ 2.3.1 {% raw %}{{}}{% endraw %}把模型的数据取出显示到页面上，`支持三元表达式`
		
			{{message?message:'init data'}}

	+ 2.3.2 {% raw %}{{*}}{% endraw %}首次绑定数据后，不随数据变化(绑定一次) 
		
			{{*message}}

	+ 2.3.3 {% raw %}{{}}{% endraw %}把html类型的数据正常绑定到页面上

			<div class="app">
			    {{{message}}}
			</div>
			<script>
			    var vm = new Vue({
			        el:'.app',
			        data:{
			            message:'<h1>hello world</h1>'
			        }
			    });
			</script>
* 2.4 Vue的实例

 	一个 Vue 实例其实正是一个 MVVM 模式中所描述的 ViewModel，实例上的data属性绑定的数据和原数据指定的是同一内存空间

		var message  = {
		    name:'hello'
		};
		var vm = new Vue({
		    el:'.app',
		    data:{
		        message:message
		    }
		});
		alert(vm.message == message);


	> 注意

	不能把原数据指向新的数据，否则不能刷新视图
		
		 var message  = {
		     name:'hello'
		 };
		 var vm = new Vue({
		     el:'.app',
		     data:{
		         message:message
		     }
		 });

		 message = {name:'hello1'}
	> 实例创建后不能设置以前没有的属性，无法映射到视图


		 var message  = {
		     name:'hello'
		 };
		 var vm = new Vue({
		     el:'.app',
		     data:{
		         message:message 
		     }
		 });
		 message.age = 100;

	> Vue.set/vm.$set可以给对象添加属性

* 2.5 实例上的属性和方法

	Vue通过$暴露实例上的属性和方法

	+ 2.5.1 $el
		
		获取当前绑定的元素

			vm.$el = document.querySelector('.app');
	
	> 不能更改绑定对象
	+ 2.5.2 $data

		获取当前绑定的数据

			vm.$data = {message:{name:1}}

	> 更换data对象刷新视图，尽量不去更换
	+ 2.5.3 $watch

		监控模型的变化

			vm.$watch('message', function (newVal,oldVal) {
			    console.log(newVal,oldVal);
			});
# 3生命周期

	+ 3.1 属性介绍

| 方法名| 用法 |
| --- | ---: |
| created	            | 先实例化,在实例化后(检测el) |
| vm.$mount('#app')	| 手动挂载实例 |
| beforeCompile	    | 开始编译之前 |
| compiled	        | 编译完成后 |
| ready	            | 插入文档后 |
| vm.$destroy()	    | 手动销毁实例 |
| beforeDestroy	    | 将要销毁 |
| destroyed	        | 销毁实例 |	



+ 3.2 生命周期的使用
	
		var vm = new Vue({
		    data:{
		        hello:'zfpx'
		    },
		    created: function () {alert('实例创建完成');},
		    beforeCompile: function () {alert('开始编译前')},
		    compiled: function () {alert('编译完成')},
		    ready: function () {alert('准备好了')},
		    beforeDestroy: function () {alert('准备销毁')},
		    destroyed: function () {alert("销毁")}
		});
		vm.$mount('#app');
		vm.$destroy();

+ 3.3 属性的计算
	
	用于计算性属性(默认为属性的get方法)

		var vm = new Vue({
		    data:{
		        name:'zfpx'
		    },
		    computed: {
		        hello: function () {
		            return this.name+8;
		        }
		    }
		});
		vm.$mount('#app');
	属性的获取和设置
		
		var vm = new Vue({
		    data:{
		        name:'zfpx'
		    },
		    computed: {
		        hello:{
		            get: function () {
		                return this.name+8;
		            },
		            set: function (val) {
		                this.name = val
		            }
		        }
		    },
		});
