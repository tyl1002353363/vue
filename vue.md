# vue学习（mvvm）

### 一、mvvm

注重UI层面，其核心是vm  也就是ViewModel。优点：易上手，轻便

view对应html   model对应js

举例：

	// html结构

	<div id="demo">
	    <p>{{message}}</p>
	    <input v-model="message">
	</div>

	var data = {
	    message: 'Hello Vue.js!'
	}

	var demo = new Vue({
	    el: '#demo',
	    data: data
	})

需要注意的的是{{这里面是表达式，可以为单独的变量，也可以添加数组的方法}}

### 二、指令 Directives

自定义指令有三个函数 bind(),update(), unbind().如果只需要updata()函数，可以直接写成函数的形式

	Vue.directive('disable', {
	    // 每当绑定的数据变化时，这个函数就被调用啦
	    update: function (value) {
	         // 这里的 this 指向一个directive对象。
	         // this.el 指向当前被绑定的DOM元素
	         // value则是所绑定数据的新值
	         this.el.disabled = !!value
	    }
	})

指令定义好之后就可以在html里面使用了

	<div id="demo">
	    <button v-disable="disabled">
	        {{disabled ? 'Disabled' : 'Submit'}}
	    </button>
	</div>

不要忘记给disabled初始的值

	var demo = new Vue({
	    el: '#demo',
	    data: {
	        disabled: false
	    }
	})


### 三、过滤器

与angular相似 都是filter，话不多说，直接上代码

	// 这里用了两个过滤器，其中U盘普尔case是vue自带的过滤器，可以直接使用，reverse是我们自己定义的过滤器
	{{message | reverse | uppercase}}

filter的定义方式也很简单

	Vue.filter('reverse',function(value){
		return value.split(' ').reverse().join(' ')
	})
这里有一点需要注意，过滤器的定义要放在new Vue前面，否则hu9i不起作用


### 四、侦听事件

侦听事件是用来侦听DOM事件的，使用到的指令是v-on

举个栗子

	<div id="demo">
	  <button v-on:click="onClick">click me</button>
	</div>

我们给button标签绑定了一个点击事件onClick，那么在Vue实例中就需要对事件进行定义

	var data = {
		n:1
	}
	var vm = new Vue({
		el:'#demo',
		data:data,
		methods:{
			onClick:function(e){
				this.n++;
			}
		}
	})
	
还可以监听键盘事件，可以用keyCode也可以用Vue提供的常用的按键的别名

	// 这是用keyCode的侦听事件
	<input v-on:keyup.13="submit"/>

	// 这是用别名的侦听事件
	<input v-on:keyup.enter="submit">

	// 这是缩写
	<input @keyup.enter="submit">

还可以对事件进行修饰，再来个栗子

	<!-- 阻止单击事件冒泡 -->
	<a v-on:click.stop="doThis"></a>
	
	<!-- 提交事件不再重载页面 -->
	<form v-on:submit.prevent="onSubmit"></form>
	
	<!-- 修饰符可以串联 -->
	<a v-on:click.stop.prevent="doThat">
	
	<!-- 只有修饰符 -->
	<form v-on:submit.prevent></form>

	
### 五、列表

提到列表，学过angular的都会想到repeat，我看有的文档也是用的repeat，但是到我这就提示这个错误
![listError](http://7xoona.com1.z0.glb.clouddn.com/ZBF%7D0DH%60W5KD%5D%7DP9ORB9%60NO.png)

最开始我也是很纠结啊，为什么不好用呢，后来才知道从1.0之后，repeat就不在用了，被for代替了。

那现在就来说说v-for，数组，对象都是用这个来循环的，像这样

	<ul>
		<li v-for="item in list">{{$index}}-{{item.title}}-{{item.content}}</li>
	</ul>

vue提供了一个$index，表示数组的下标。

你还可以这样写。

	<div>
	  	<span v-repeat="n in 10">{{ n }} </span>
	</div>

如果是对象，你需要这样写，

	<ul>
		<li v-for="value in obj">
			{{$key}}:{{value}}
		</li>
	</ul>

同样，你也可以这样写，

	<ul>
		<li v-for="(key,value) in obj">
			{{key}}:{{value}}
		</li>
	</ul>

这里相当于给$key起了一个别名，其实两种方式都是一样的。

Vue还提供两种方法，一个是变异方法，一个是非变异方法，分别介绍一下

变异方法，用了这个方法，原数组会被修改，比如：push(),pop(),shift(),unshift(),splice(),sort()和reverse(),这些我就不去一个个解释了，举个栗子吧

	vm.list.push({ title: 'title1' })

非变异方法，那就是跟变异方法相不同的啦，这个就是执行完这种方法对原数组没有影响的，比如：filter(), concat() 和 slice()，上代码

	vm.list = vm.list.filter(function (item) {
	   return item.title.match(/list/)
	})

接下来是track-by和track-by $index,原谅我脑子不好使，我还真没看懂这两个什么作用，大家可以去官网的教程看一看 [vue官网](http://cn.vuejs.org/guide/list.html#track-by)

还有两个方法$set(),$remove(),举个官网上的栗子，直接用索引设置元素

	// 第一个参数是索引值，第二个参数是需要改变的
	example1.items.$set(0, { title: 'changeTitle'})

修改数据的长度，删除数组中的某一项

	// 删除数组的第一项
	vm.list.$remove(vm.list[0])






	


