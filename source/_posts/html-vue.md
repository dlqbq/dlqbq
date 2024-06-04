---
title: 在html中直接使用vue.js
---
比较小众需求，适合静态网站，搭配CDN，🍺!

### 1. 在HTML文件中从CDN引入了Vue.js库, {% post_link useful-cdn %}。

``` html
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/3.4.27/vue.global.prod.min.js"></script>
```

### 2. 在HTML文件中插入如下代码

``` html
<div id="app">
    <h1>{{title()}}</h1>
    <button @click="clickme">点击我</button>
    <p v-if="showMessage">{{ message }}</p>
    <ol>
        <li v-for="item in items">{{ item }}</li>
    </ol>
</div>
```

### 3. 在JavaScript代码

``` javascript
const app = Vue.createApp({
  data() {
    return {
      showMessage: false,
      message: 'Hello',
      items: ['HTML', 'CSS', 'JavaScript']
    }
  },
  methods: {
    title: function () {
      return `${this.message}, 你成功了!`;;
    },

    clickme() {
      this.showMessage = !this.showMessage;
    }
  }
}).mount('#app');
```

更多用法请查阅文档 [VueJs](https://vuejs.org/)