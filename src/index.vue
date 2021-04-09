<template>
	<div ref="waterfall" class="waterfall" :style="{height: containerHeight + 'px'}">
		<div ref="widgetRef" class="waterfall-item waterfall-fade" v-for="(item, index) in widgetList" :key="index" :style="{width: columnWidth + 'px'}">
			<slot v-bind:data="item" />
		</div>
	</div>
</template>
<script>
export default {
  name: 'VueWaterfallRender',
  props: {
    dataList: {
      type: Array,
      default: () => []
    },
    minColumnWidth: {
      type: Number,
      default: 260
    },
    gap: {
      type: Number,
      default: 8
    }
  },
  data() {
    return {
      containerHeight: 300,
      columnHeightMap: {},
      columnWidth: this.minColumnWidth,
      widgetList: []
    };
  },
  mounted() {
    window.addEventListener('resize', this.resizeListener);
    const { columnCount, columnWidth } = this.calcuCols();
    this.columnCount = columnCount;
    this.columnWidth = columnWidth;
    this.preloadLoad();
  },
  watch: {
    dataList() {
      this.preloadLoad();
    }
  },
  methods: {
    calcuCols() {
      const clientWidth = this.$refs.waterfall.clientWidth;
      const { gap, minColumnWidth } = this;
      const columnCount = Math.max(Math.floor(clientWidth / (minColumnWidth + gap)), 1);
      let columnWidth = minColumnWidth;
      if (columnCount === 1) {
        columnWidth = clientWidth;
      } else {
        columnWidth = (clientWidth + gap) / columnCount - gap;
      }
      return { columnCount, columnWidth };
    },
    async preloadLoad() {
      const { dataList } = this;
      const widgetList = [];
      for (const item of dataList) {
        if (item.cover) {
          if (item.coverWidth && item.coverHeight) {
            item.imgWidth = item.imgWidth || item.coverWidth;
            item.imgHeight = item.imgHeight || item.coverHeight;
            item.coverHeight = Math.round(this.columnWidth / (item.imgWidth / item.imgHeight));
          } else {
            await this.imgAsyncLoad(item.cover);
          }
        }
        widgetList.push(item);
      }
      this.widgetList = widgetList;
      this.$nextTick(() => {
        this.renderWidgets();
      });
    },
    renderWidgets() {
      const els = this.$refs.widgetRef;
      this.columnHeightMap = {};
      els && els.forEach(el => {
        const { columnHeightMap, gap, columnWidth, columnCount } = this;
        const columnsHeight = Object.values(columnHeightMap);
        let top = 0;
        let left = 0;
        let columnIndex = 0;
        if (columnsHeight.length < columnCount) {
          columnIndex = columnsHeight.length;
        } else {
          const minColheight = Math.min.apply(null, columnsHeight);
          columnIndex = columnsHeight.indexOf(minColheight);
          top = minColheight + gap;
        }
        left = columnIndex * (columnWidth + gap);
        el.style.top = top + 'px';
        el.style.left = left + 'px';
        columnHeightMap[columnIndex] = top + el.clientHeight;
        this.containerHeight = Math.max.apply(null, Object.values(columnHeightMap)) + gap;
      });
    },
    imgAsyncLoad(url) {
      return new Promise((resolve, reject) => {
        var img = new Image();
        img.src = url;
        img.onload = img.onerror = (e) => {
          const height = e.type === 'load' ? Math.round(this.columnWidth / (img.width / img.height)) : 0;
          resolve(height);
        };
      });
    },
    resizeListener() {
      const { columnCount, columnWidth } = this.calcuCols();
      this.columnCount = columnCount;
      this.columnWidth = columnWidth;
      this.preloadLoad();
    }
  }
};
</script>
<style lang="scss" scoped>
.waterfall {
  position: relative;
	.waterfall-item {
		position: absolute;
	}
  .waterfall-fade {
    transition: 0s;
    animation: waterfallFade 0.5s ease both;
  }
}
@keyframes waterfallFade {
  0%{
    opacity: 0;
    transform: translateY(-30px)
  }
  100%{
    opacity: 1;
    transform: translateY(0)
  }
}
</style>
