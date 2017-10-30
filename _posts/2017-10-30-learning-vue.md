---
layout:     post

title:      "第一次阅读 Vue.js 文档"

date:       2017-10-30 23:22:00

author:     "Shinya"

tags:
    - 前端
    - Vue
    - 学习
---

>一兮老师说了，学习一个新的东西不一定要干很多事，读完文档你就懂了，我已经很多年不看书，当然前提是要认真读

所以这里是一篇读[文档](https://vuejs.org/)中的Introduction后的小结。

- 声明式渲染：使用模板中的“胡子标签”， 通过v-bind来实现属性绑定
- 条件渲染：使用v-if命令实现条件渲染
- 循环： 使用v-for指令实现循环
- 处理用户输入： 使用v-on:click监听点击事件
- 双向绑定：使用v-model实现的双向绑定（类似React中的controlled components？）
- 组件系统： Vue.component(id, config)
- props：给组件传递参数Vue.component(id, {template,props})

可能大致开发套路是：
1. 创建组件：Vue.component()...
2. 创建模板：（使用组件）
3. 使用模板：（传入数据参数）

不一定对，等几天来验证
