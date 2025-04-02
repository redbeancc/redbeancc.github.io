---
title: element-ui 分页插件
tags:
  - element-ui
  - vue
categories:
  - 问题
description: "\U0001F33C 对分页插件的记录，方便复制使用"
abbrlink: 23c1a3eb
date: 2022-04-22 13:24:21
cover:
---
# element-ui 分页插件
```html
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

```js
handleSizeChange(e) {
    this.currentPage = 1
    this.pageSize = e
    this.load()
},
handleCurrentChange(pageNum) {
    this.currentPage = pageNum
    this.load()
}
```