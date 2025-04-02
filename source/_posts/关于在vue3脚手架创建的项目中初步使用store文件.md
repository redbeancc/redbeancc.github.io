---
title: 关于在vue3脚手架创建的项目中初步使用store文件
tags:
  - vue
  - vuex
categories:
  - 教程
description: 🍳vue3 简单的使用 store
abbrlink: 2c99a659
date: 2021-09-27 12:04:32
cover:
---
# 关于在vue3脚手架创建的项目中初步使用store文件

> 在用vue3脚手架创建完成后项目工程里出现store文件夹，里面有个index.js文件，根据查找网上的信息来看，里面都是存储一些全局变量、异步变量、状态管理之类的。在这里，我初步使用了一下这个文件，仅作个人感悟，很暴力的使用。

先看一下脚手架生成的store文件

```vue
import { createStore } from 'vuex'

export default createStore({
  state: {
  // 放置变量所用
  },
  mutations: {
  // 获取set方法
  },
  actions: {
  },
  modules: {
  },
})
```

这里我先暂时用了两个方法，state和mutations，分别是放置全局变量和set方法

在vuex官网上还有一个概念：[Getter](https://vuex.vuejs.org/zh/guide/getters.html) ，在这个方法里可以写get方法获取变量

于是，修改过后的store文件如下

```vue
import { createStore } from 'vuex'

export default createStore({
  state: {
    url: "/api" + JSON.parse(window.sessionStorage.getItem("admin")).adminpic
  },
  getters: {
    geturl: state => state.url
  },
  mutations: {
    seturl(state,newurl){
      state.url = newurl
  	}
  },
  actions: {
  },
  modules: {
  },
})
```

这里存储了一个全局变量url、get方法和set方法

然后在组件1中引用

1.vue

```
<script>
export default {
	data () {
		return {
			url: “”
		}
	},
	created () {
		this.url = this.$store.getters.geturl		//这里用get方法获取到变量
	}
}
</script>
```

在组件2中修改全局变量

2.vue

```
<script>
export default {
	data () {
		return {
			url: "111"
		}
	},
	methods: {
		change () {			// 这里是你自己随意定义的一个方法
			this.$store.commit("seturl",this.url)	// 这里使用set方法对store中的变量进行更改
		}
	}
}
</script>
```

经过change方法更改过后，store中的变量url就更改为111了

组件2让store变量修改后因为组件1已经DOM渲染完了，所以如果想让组件1也随之改变，就需要对组件1的变量url进行监听

监听代码如下：

1.vue

```
<script>
export default {
	data () {
		return {
			url: “”
		}
	}，
    computed: {		// computed是计算属性，state里面的url改变之后，就会进行一次计算，并返回计算值到对应的参数中
        url () {
          return this.$store.state.url
        }
    },
    watch:{		// 监听属性，在computed计算属性更改之后会触发参数值的改变，所以能够监听到
      url () {
      // 这里会在commit之后触发，然后做后续的操作
        this.url = this.$store.getters.geturl
      }
    },
	created () {
		this.url = this.$store.getters.geturl		//这里用get方法获取到变量
	}
}
</script>
```

这样的话，在组件2中修改store全局变量后，组件1会检测到然后里面的变量也会随之修改

当然，这是最粗暴的使用奥，若有不规范的，轻喷

{% note warning flat%}
现在好像在 vue3 中使用 pinia 更轻便
{% endnote %}