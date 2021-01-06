# vue-waterfall-render

基于vue.js的响应式瀑布流组件。

## 示例

点击[DEMO](https://imkch.github.io/vue-waterfall-render/dist/index.html)查看

## 安装使用

### 通过NPM安装，import导入

``` bash
npm install --save vue-waterfall-render
```
**全局安装**
``` javascript
import VueWaterfallRender from 'vue-waterfall-render';
// 引入样式文件
import 'vue-waterfall-render/lib/index.css';

Vue.component('VueWaterfallRender', VueWaterfallRender);
```

**局部安装**
``` javascript
import VueWaterfallRender from 'vue-waterfall-render';
// 引入样式文件
import 'vue-waterfall-render/lib/index.css';

export default {
  components: {
    VueWaterfallRender
  }
}
```

``` html
<vue-waterfall-render :dataList="dataList">
  <template v-slot="scope">
    <a href="//imkch.com">
      <img v-if="scope.data.cover" :src="scope.data.cover"/>
      <div class="info">
        <h3>{{scope.data.name}}</h3>
        <p>{{scope.data.describe}}</p>
      </div>
    </a>
  </template>
</vue-waterfall-render>
```

### 通过Script标签引入

``` html
<link href="https://unpkg.com/vue-waterfall-render/lib/index.css">
<script src="https://unpkg.com/vue-waterfall-render/lib/index.umd.js"></script>
```

``` javascript
new Vue({
  el: '#app',
  components: {
    VueWaterfallRender
  }
})
```

## API

### 属性（props）

| 属性名         | 类型   | 默认 | 描述               |
| -------------- | ------ | ---- | ------------------ |
| dataList       | Array  | []   | 需要渲染的数据集合 |
| minColumnWidth | Number | 260  | 最小列宽           |
| gap            | Number | 8    | 行列间距           |
