<template>
  <div id="app">
    <vue-waterfall-render class="data-con" :dataList="dataList">
      <template v-slot="scope">
        <a href="//imkch.com">
          <img v-if="scope.data.cover" :src="scope.data.cover" :height="scope.data.coverHeight"/>
          <div class="info">
            <h3>{{scope.data.name}}</h3>
            <p>{{scope.data.describe}}</p>
          </div>
        </a>
      </template>
    </vue-waterfall-render>
  </div>
</template>

<script>
import VueWaterfallRender from '../src/index.vue';
import MockJs from 'mockjs';
export default {
  name: 'App',
  components: {
    VueWaterfallRender
  },
  data() {
    return {
      dataList: [],
      imgSizeMap: {
        '0.jpg': [500, 333],
        '1.jpg': [500, 333],
        '2.jpg': [500, 313],
        '3.jpg': [391, 220],
        '4.jpg': [500, 358],
        '5.jpg': [500, 300],
        '6.jpg': [526, 300],
        '7.jpg': [500, 313],
        '8.jpg': [500, 334],
        '9.jpg': [533, 300]
      }
    }
  },
  mounted() {
    this.getDataList();
  },
  methods: {
    async getDataList() {
      const Random = MockJs.Random;
      const mockData = [];
      for (let i = 0; i < 100; i++) {
        const imgName = `${Math.floor(Math.random() * 10)}.jpg`;
        mockData.push({
          name: i + 1 + '、' + Random.ctitle(Math.ceil(Math.random() * 10)),
          describe: Random.cword(Math.ceil(Math.random() * 100)),
          cover: Math.floor(Math.random() * 5) && `./img/${imgName}`,
          coverWidth: this.imgSizeMap[imgName][0],
          coverHeight: this.imgSizeMap[imgName][1]
        });
      }
      this.dataList = mockData;
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 30px;
  .data-con {
    a {
      display: block;
      box-shadow: 0 1px 3px rgba(18, 18, 18, .1);
      color: #666;
      text-decoration: none;
      text-align: left;
      img {
        width: 100%;
      }
      .info {
        padding: 8px 16px;
      }
      h3 {
        margin: 0;
        margin-bottom: 4px;
      }
      p {
        margin: 0;
      }
    }
  }
}
</style>
