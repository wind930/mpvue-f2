<template>
  <div class="page">
    <div class="chart">
      <div class="title">
        <span>f2 图表</span>
      </div>
      <div class="chart-wrapper">
        <mpvue-f2 :f2="f2" :onInit="initSingle" canvasId="single-canvas" ref="single" />
      </div>
    </div>
    <div class="lazy">
      <div class="title">
        <span>延迟加载</span>
      </div>
      <div class="chart-wrapper">
        <span v-if="status" @click="lazyLoadChart">点击此处加载图形</span>
        <mpvue-f2 v-else :f2="f2" :onInit="initLazy" canvasId="lazy-canvas" ref="lazy" lazyLoad />
      </div>
    </div>
  </div>
</template>

<script>

import mpvueF2 from 'mpvue-f2'
const F2 = require('@antv/f2')

export default {
  data () {
    return {
      f2: F2,
      initSingle: this.drawSingle,
      initLazy: this.drawLazy,
      status: true
    }
  },

  components: {
    mpvueF2
  },

  methods: {
    drawSingle (canvas, width, height) {
      let singleChart = new F2.Chart({
        el: canvas,
        width,
        height
      })

      const data = [{ 'year': 1997, 'type': 'United States', 'value': 4.9 }, { 'year': 1997, 'type': 'Florida', 'value': 4.8 }, { 'year': 1998, 'type': 'United States', 'value': 4.5 }, { 'year': 1998, 'type': 'Florida', 'value': 4.3 }, { 'year': 1999, 'type': 'United States', 'value': 4.2 }, { 'year': 1999, 'type': 'Florida', 'value': 3.9 }, { 'year': 2000, 'type': 'United States', 'value': 4 }, { 'year': 2000, 'type': 'Florida', 'value': 3.7 }, { 'year': 2001, 'type': 'United States', 'value': 4.7 }, { 'year': 2001, 'type': 'Florida', 'value': 4.7 }, { 'year': 2002, 'type': 'United States', 'value': 5.8 }, { 'year': 2002, 'type': 'Florida', 'value': 5.6 }, { 'year': 2003, 'type': 'United States', 'value': 6 }, { 'year': 2003, 'type': 'Florida', 'value': 5.2 }, { 'year': 2004, 'type': 'United States', 'value': 5.5 }, { 'year': 2004, 'type': 'Florida', 'value': 4.6 }, { 'year': 2005, 'type': 'United States', 'value': 5.1 }, { 'year': 2005, 'type': 'Florida', 'value': 3.7 }, { 'year': 2006, 'type': 'United States', 'value': 4.6 }, { 'year': 2006, 'type': 'Florida', 'value': 3.2 }, { 'year': 2007, 'type': 'United States', 'value': 4.6 }, { 'year': 2007, 'type': 'Florida', 'value': 4 }, { 'year': 2008, 'type': 'United States', 'value': 5.8 }, { 'year': 2008, 'type': 'Florida', 'value': 6.3 }, { 'year': 2009, 'type': 'United States', 'value': 9.3 }, { 'year': 2009, 'type': 'Florida', 'value': 10.4 }, { 'year': 2010, 'type': 'United States', 'value': 9.6 }, { 'year': 2010, 'type': 'Florida', 'value': 11.1 }, { 'year': 2011, 'type': 'United States', 'value': 8.9 }, { 'year': 2011, 'type': 'Florida', 'value': 10 }, { 'year': 2012, 'type': 'United States', 'value': 8.1 }, { 'year': 2012, 'type': 'Florida', 'value': 8.5 }, { 'year': 2013, 'type': 'United States', 'value': 7.4 }, { 'year': 2013, 'type': 'Florida', 'value': 7.2 }, { 'year': 2014, 'type': 'United States', 'value': 6.2 }, { 'year': 2014, 'type': 'Florida', 'value': 6.3 }, { 'year': 2015, 'type': 'United States', 'value': 5.3 }, { 'year': 2015, 'type': 'Florida', 'value': 5.4 }, { 'year': 2016, 'type': 'United States', 'value': 4.9 }, { 'year': 2016, 'type': 'Florida', 'value': 4.9 }, { 'year': 2017, 'type': 'United States', 'value': 4.4 }, { 'year': 2017, 'type': 'Florida', 'value': 4.3 }]

      singleChart.source(data, {
        year: {
          range: [0, 1],
          ticks: [1997, 1999, 2001, 2003, 2005, 2007, 2009, 2011, 2013, 2015, 2017]
        },
        value: {
          tickCount: 10,
          formatter (val) {
            return val.toFixed(1) + '%'
          }
        }
      })

      singleChart.tooltip({
        custom: true,
        showCrosshairs: true,
        onChange (obj) {
          const legend = singleChart.get('legendController').legends.top[0]
          const tooltipItems = obj.items
          const legendItems = legend.items
          const map = {}
          legendItems.map(item => {
            map[item.name] = Object.assign({}, item)
          })
          tooltipItems.map(item => {
            const { name, value } = item
            if (map[name]) {
              map[name].value = value
            }
          })
          legend.setItems(Object.values(map))
        },
        onHide () {
          const legend = singleChart.get('legendController').legends.top[0]
          legend.setItems(singleChart.getLegendItems().country)
        }
      })

      singleChart.guide().rect({
        start: [2011, 'max'],
        end: ['max', 'min'],
        style: {
          fill: '#CCD6EC',
          opacity: 0.3
        }
      })
      singleChart.guide().text({
        position: [2014, 'max'],
        content: 'Scott administratio\n(2011 to present)',
        style: {
          fontSize: 10,
          textBaseline: 'top'
        }
      })

      singleChart.line().position('year*value').color('type', val => {
        if (val === 'United States') {
          return '#ccc'
        }
      })
      singleChart.render()
      return singleChart
    },
    drawLazy (canvas, width, height) {
      let lazyChart = new F2.Chart({
        el: canvas,
        width,
        height
      })

      var Global = F2.Global
      var data = [
        { country: '巴西', population: 18203 },
        { country: '印尼', population: 23489 },
        { country: '美国', population: 29034 },
        { country: '印度', population: 104970 },
        { country: '中国', population: 131744 }
      ]

      lazyChart.source(data, {
        population: {
          tickCount: 5
        }
      })
      lazyChart.coord({
        transposed: true
      })
      lazyChart.axis('country', {
        line: Global._defaultAxis.line,
        grid: null
      })
      lazyChart.axis('population', {
        line: null,
        grid: Global._defaultAxis.grid,
        label: function label (text, index, total) {
          var textCfg = {}
          if (index === 0) {
            textCfg.textAlign = 'left'
          } else if (index === total - 1) {
            textCfg.textAlign = 'right'
          }
          return textCfg
        }
      })
      lazyChart.interval().position('country*population')
      lazyChart.render()

      return lazyChart
    },
    lazyLoadChart () {
      this.status = false
      setTimeout(() => {
        this.$refs.lazy.init()
      }, 0)
    }
  },

  created () {}
}
</script>

<style scoped>

.page {
  padding: 10px;
  margin: 0;
  font-size: 16px;
  color: #999;
  font-family: -apple-system-font, "Helvetica Neue", sans-serif;
}

.page div {
  margin-bottom: 20px;
  padding: 10px 0;
}

.title > span {
  display: inline-block;
  padding: 0 10px;
  border-left: #448ef6 4px solid;
}

.chart-wrapper {
  width: 100%;
  height: 300px;
}

.chart-wrapper > span {
  display: inline-block;
  width: 100%;
  height: 80%;
  text-align: center;
}

</style>
