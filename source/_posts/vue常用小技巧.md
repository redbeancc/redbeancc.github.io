---
title: vue常用小技巧
tags:
  - vue
  - element-ui
categories:
  - 教程
description: ☎ 关于 vue 的一些常用小技巧
abbrlink: 797606c
date: 2023-04-28 15:37:41
cover:
---
# vue常用小技巧

#### 在线预览

```
preview(url) {
      window.open('https://file.keking.cn/onlinePreview?url=' + encodeURIComponent(window.btoa((url))))
    },
```

#### element图片预览

```
<template>
    <div>
        <el-button @click="onPreview">预览</el-button>
        <el-image-viewer 
                     v-if="showViewer" 
                     :on-close="closeViewer" 
                     :url-list="[url]" />
    </div>
</template>
<script>
    // 导入组件
    import ElImageViewer from 'element-ui/packages/image/src/image-viewer'
    
    export default {
      name: 'Index',
      components: { ElImageViewer },
      data() {
        return {
          showViewer: false, // 显示查看器
          url:'https://cube.elemecdn.com/6/94/4d3ea53c084bad6931a56d5158a48jpeg.jpeg'
        }
      },
      methods: {
        onPreview() {
          this.showViewer = true
        },
        // 关闭查看器
        closeViewer() {
          this.showViewer = false
        }
    }
</script>

```

```
<!--    图片预览-->
<el-image-viewer v-if="showViewer" :on-close="closerViewer" :url-list="[viewerUrl]"></el-image-viewer>
    
二 
import ElImageViewer from 'element-ui/packages/image/src/image-viewer'

三
name: "SysFeilu",
components: { ElImageViewer },

四
showViewer: false, // 图片预览
viewerUrl: ''     // 预览地址

五
preview(url) {
    this.viewerUrl = url
    this.showViewer = true
},
closerViewer() {
    this.viewerUrl = ''
    this.showViewer = false
}
```

#### 下载

```
download(url) {
	window.open(url)
}
```

#### 分页

```
    <div class="pd-10">
      <el-pagination
        @size-change="(e) => handleSizeChange(e)"
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-sizes="[100, 200, 300, 400, 500]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
        background>
      </el-pagination>
    </div>
```

```
pageSize: 100,
currentPage: 1,
total: 0,
```

```
handleSizeChange(e) {
    this.currentPage = 1
    this.pageSize = e
    this.load()
},
handleCurrentChange(pageNum) {
    this.currentPage = pageNum
    this.load()
},
```

#### 解决element-ui中下拉菜单子选项click事件不触发的问题
将@click改为@click.native='logout';即可监听选项的点击事件。(退出响应不迅速是因为把@click给了span标签，正确的是写在el-dropdown-item上)
```javascript
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>个人信息</el-dropdown-item>
        <el-dropdown-item @click.native="logout">
          <span style="text-decoration: none">退出</span>
        </el-dropdown-item>
      </el-dropdown-menu>
```

### element按钮实现倒计时
1. view部分
```
<el-button @click="sendMsg" 
            type="primary" 
            :disabled="canClick">
            {{ content }}
</el-button>
```
2. data
```
data() {
    return {
   	  content: '发送短信',
      totalTime: 10,
      canClick: false
    }
  }
```
3. js
```
sendMsg() {
  if (this.canClick) return
  this.canClick = true
  this.content = this.totalTime + 's后重新发送'
  let clock = window.setInterval(() => {
    this.totalTime--
    this.content = this.totalTime + 's后重新发送'
    if (this.totalTime < 0) {
      window.clearInterval(clock)
      this.content = '重新发送短信'
      this.totalTime = 10
      this.canClick = false
    }
  }, 1000)
}
```
两个块级元素并排显示：float:left,注意要小于父元素的宽高

#### js操纵css属性变化
```
var info = document.getElementById('info')
info.setAttribute('class','on')
var pwd = document.getElementById('pwd')
pwd.removeAttribute('class','on')
```

#### 当input层数太深不响应时，强制刷新,尤其是在from表单中
```
 <el-input v-model="user.username" 
 @input="inputChange" 
 placeholder="用户名称" 
 style="float: left;width: 200px"></el-input>


inputChange(e) {
        //强制刷新
        this.$forceUpdate()
      }
```
#### vuex刷新页面数据丢失，使用session或者local存放
```js
// 这是vue中的state
imgUrl: localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')).avatar : '',
username: localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')).username : '',
token: localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')).token : '',
```
#### 下拉框el-dropdown格式不对的时候看看页面样式，是否是因为宽度太宽，宽度太宽，在外层套一个div，样式写在外部div上
#### el-upload上传携带token
```
<el-upload
    class="avatar-uploader"
    action="http://localhost:9090/file/upload"
    :show-file-list="false"
    :on-success="handleAvatarSuccess"
    :headers="header">
    
  var head = localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')).token : ''
  export default {
    name: "Person",
    data() {
      return {
        title: '我的信息',
        imageUrl: "",
        user: {},       // 更新信息
        flag: '我的信息',
        userpassword: {}, // 修改密码
        passwordsend: {},
        header: {token: head}
      }
    },
```
### 轮播图上下图层实现背景毛玻璃
```
    <el-carousel height="300px">
        <el-carousel-item v-for="item in carouseList" :key="item">
          <div style="position:relative">
            <div style="width: 980px;position:absolute;z-index: 30;left: 50%;margin-left: -490px">
              <img :src="item" style="width: 100%">
            </div>
            <div style="position:absolute;width: 100vw;z-index: 1">
              <img :src="item" style="width: 100%;filter: blur(10px);">
            </div>
          </div>
        </el-carousel-item>
    </el-carousel>
    /**
    *div实现上下层使用z-index属性越大，图层越靠前
    *注意：父部元素需要position:relative,子类元素position:absolute
    *absolute绝对位置实现居中
    *    position: absolute;
    *    left: 50%;
    *    width: 980px;
    *    margin-left: -490px;     //除去自身的宽度，就是居中位置
    */
```
块级元素居中：text-align: center;line-height: 80px
#### 多余字成省略号
```
.box{
    background: red;
    width: 100px;
    white-space: nowrap;             /*使文本内容不换行，写在一行*/
    overflow: hidden;                /*隐藏多余内容*/
    text-overflow: ellipsis;         /*将多余内容以省略号的方式展示*/
}
```
#### 多余字成省略号(两行后省略)
```
.name {
    font-size: 10px;
    font-family: sans-serif;
    width: 240px;
    text-overflow: ellipsis;
    word-break: break-all;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
    overflow: hidden;
}
```
### 入参时间，出参时间格式化
```java
// 设置入参
@DateTimeFormat(pattern="yyyy-MM-dd HH:mm:ss")
private Date date;

// 设置出参
@JsonFormat(pattern = "yyyy-MM-dd")
private Date endDate;
```

### 标签栏固定住不滚动，main 部分滚动
```html
<Headers :title="this.highScore.title"
         :subtitle="this.highScore.subtitle" 
         style="position:sticky;top:0px;background-color:white;"/>
粘性布局
position:sticky;
top: 0px;
当页面滚动到一定程度的时候，让某元素固定在指定位置
```