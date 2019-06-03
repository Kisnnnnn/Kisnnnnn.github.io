---
layout: post
title:  "2019-05-29-VUE中的$ref、$on、$emit"
date:   2019-05-29 21:00:00 +0800
categories: 前端
tag: 笔记
---

* content
{:toc}



## $emit(eventNanme,[...args])

- 参数：
    - {string} eventName 触发事件名字
    - [...args] 附加的参数
- 用法：
    - 触发当前实例上的事件。附加参数都会传给监听器回调。
- 使用场景
    - 子组件调用父组件的方法并且传递数据


> 注意：子组件标签中的时间也不区分大小写要用“-”隔开

子组件：

```
<template>
  <button @click="emitEvent">点击我</button>
</template>
<script>
  export default {
    data() {
      return {
        msg: "我是子组件中的数据"
      }
    },
    methods: {
      emitEvent(){
        this.$emit('my-event', this.msg)
        //通过按钮的点击事件触发方法，然后用$emit触发一个my-event的自定义方法，传递this.msg数据。
      }
    }
  }
</script>
```

父组件：

```
<template>
  <div id="app">
    <child-a @my-event="getMyEvent"></child-a>
    <!--父组件中通过监测my-event事件执行一个方法，然后取到子组件中传递过来的值-->
  </div>
</template>
<script>
  import ChildA from './components/child.vue'
  export default {
    components: {
      ChildA
    },
    methods: {
      getMyEvent(msg){
          console.log('接收的数据--------->'+msg)//接收的数据--------->我是子组件中的数据
      }
    }
  }
</script>
```

- 交互过程：
    - child.vue为子组件，当在child.vue，点击button。
    - 触发子组件的「emitEvent」方法
    - 在emitEvent方法中执行「this.$emit)('my-event',this.msg)」,调用父组件的「my-event」的方法
    - 执行「getMyEvent」方法

实际上就是在子组件上操作，但是作用在父组件上。


## ref
> 被用来给元素或者子组件注册引用信息。引用信息将会被注册在父组件的$refs对象上。如果在普通的DOM元素上使用，引用指向的就是DOM元素；如果用在子组件上，引用就是指向组件实例：

```
<!-- `vm.$refs.p` will be the DOM node -->
<!-- `vm.$refs.p` 指向这个DOM -->
<p ref="p">hello</p>
<!-- `vm.$refs.child` will be the child component instance -->
<!-- `vm.$refs.child` 指向这个子组件 -->
<child-com ref="chil"</child-com>
```

ref 本身是作为渲染结果被创建的，在初始化渲染的时候你不能访问他们，因为他们还不存在，$refs也不是响应式的，因此你不应该试图用它在模板中做数据绑定。

## $ref

> 但是我们可以使用$ref在直接访问这个子组件。

- #### 适用环境：
    - 父组件调用子组件的方法，可以传递数据。

父组件：

```
<template>
  <div id="app">
    <child-a ref="child"></child-a>
    <!--用ref给子组件起个名字-->
    <button @click="getMyEvent">点击父组件</button>
  </div>
</template>
<script>
  import ChildA from './components/child.vue'
  export default {
    components: {
      ChildA
    },
    data() {
      return {
        msg: "我是父组件中的数据"
      }
    },
    methods: {
      getMyEvent(){
          this.$refs.child.emitEvent(this.msg);
          //调用子组件的方法，child是上边ref起的名字，emitEvent是子组件的方法。
      }
    }
  }
</script>
```

子组件：

```
<template>
  <button>点击我</button>
</template>
<script>
  export default {
    methods: {
      emitEvent(msg){
        console.log('接收的数据--------->'+msg)//接收的数据--------->我是父组件中的数据
      }
    }
  }
</script>
```
- #### 交互过程
    - 绑定「child-a」组件，定义为「child」
    - 触发父组件的「getMyEvent」
    - 调用子组件的方法，child是上边ref起的名字，emitEvent是子组件的方法。
    - 子组件中「emitEvent」方法触发。

## $on(event,callback)

- 参数：
    - {string | Array<string>} event 
    - {Funtion} callback
- 用法：
    - 监听当前示例上的自定义事件。事件可以由vm.$emit触发。回调函数会接收所传入事件触发函数的额外参数
- 适用环境：
    - 兄弟组件进行通信

### 示例

- 首先创建一个vue的空白实例（兄弟间的桥梁）

- 父组件

```
<template>
     <div>
      <childa></childa>	
      <br />
      <childb></childb>  	
     </div>
</template>
<script>
   import childa from './childa.vue';
   import childb from './childb.vue';
   export default {
   	components:{
   		childa,
   		childb
   	},
   	data(){
   		return {
   			msg:""
   		}
   	},
   	methods:{
   		
   	}
   }
</script>
```
- 子组件A  childa

发送方使用 ==$emit== 自定义时间把数据带过去

```
<template>
    <div>
        <span>A组件->{{msg}}</span>
        <input type="button" value="把a组件数据传给b" @click ="send">
    </div>
</template>
<script>

// 空的组件(兄弟的桥梁)
import vmson from "../../../util/emptyVue"

export default {
    data(){
        return {
            msg:{
            	a:'111',
            	b:'222'
            }
        }
    },
    methods:{
        send:function(){
            vmson.$emit("aevent",this.msg)
        }
    }
}
</script>
```

- 子组件 childb

而接收方通过 ==$on==监听自定义时间的callback接收数据

```
<template>
 <div>
    <span>b组件,a传的的数据为->{{msg}}</span>
 </div>
</template>
<script>
	  import vmson from "../../../util/emptyVue"
	  export default {
		 data(){
		        return {
		            msg:""
		        }
		    },
		 mounted(){
		        vmson.$on("aevent",(val)=>{
		            //监听事件aevent，回调函数要使用箭头函数;
		            console.log(val);//打印结果：我是a组件的数据
		            this.msg = val;
		        })
		  }
	}
</script>
```

#### 交互过程：
- 定义一个父组件，作为A/B子组件的通信桥梁
- 在A组件来中设定一个自定义事件「aevent」
- 在B组件中，通过父组件「vmson」进行监听「aevent」事件的触发，进行回调


当然还有其他API

![image](https://ws3.sinaimg.cn/large/006tNc79gy1g3j3ej34uaj30jm0jlaah.jpg)