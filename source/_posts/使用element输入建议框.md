---
title: 使用element输入建议框
tags:
  - element-ui
categories:
  - 问题
description: "\U0001F512 使用element输入建议框"
abbrlink: a0389d4b
date: 2022-12-06 15:22:27
cover:
---
# 使用element输入建议框
## 要注意value一致
## html
```html
    <el-autocomplete
      value-key="value"
      prefix-icon="el-icon-search"
      class="inline-input pd-10 mr-10"
      v-model="author"
      :debounce=300
      :fetch-suggestions="querySearch"
      placeholder="请输入作者"
      :trigger-on-focus="false"
      @select="handleSelect"
      style="width: 200px;"
      clearable
    ></el-autocomplete>
```
## js
```js
    querySearch(queryString, cb) {
        let results = []
        this.request.get("/author/querySearch", {
          params: {
            authorName: this.author
          }
        }).then(res => {
          if (res.code === '200') {
            const r = res.data
            results = r
            setTimeout(() => {
              cb(results)
            }, 1000 * Math.random())
            
          }
          setTimeout(() => {
              cb(results)
            }, 1000 * Math.random())
        })
        setTimeout(() => {
          cb(results)
        }, 1000 * Math.random())
      },
      handleSelect(item) {
        this.authorId = item.name
        // console.log(item.name);
      },
```
## json 关键词 刘
```json
{
  "code": "200",
  "msg": "操作成功",
  "data": [
    {
      "name": 278,
      "value": "[美] 刘易斯·M.霍普费"
    },
    {
      "name": 420,
      "value": "[英] C. S. 刘易斯"
    },
    {
      "name": 432,
      "value": "[英] 刘易斯·卡罗尔"
    },
    {
      "name": 584,
      "value": "刘大铭"
    },
    {
      "name": 585,
      "value": "刘妍"
    },
    {
      "name": 586,
      "value": "刘子超"
    },
    {
      "name": 587,
      "value": "刘慈欣"
    },
    {
      "name": 588,
      "value": "刘擎"
    },
    {
      "name": 589,
      "value": "刘新宇"
    },
    {
      "name": 590,
      "value": "刘汝佳"
    },
    {
      "name": 591,
      "value": "刘津"
    },
    {
      "name": 592,
      "value": "刘润"
    },
    {
      "name": 593,
      "value": "刘知远"
    },
    {
      "name": 594,
      "value": "刘雪峰"
    },
    {
      "name": 595,
      "value": "刘震云"
    },
    {
      "name": 596,
      "value": "刘鹏"
    }
  ]
}
```