<template>
  <div class="page">
    <div class="chart">
      <div class="title">
        <span>echarts 图表</span>
      </div>
      <div class="chart-wrapper">
        <mpvue-echarts :echarts="echarts" :onInit="initSingle" canvasId="single-canvas" ref="single" />
      </div>
    </div>
    <div class="lazy">
      <div class="title">
        <span>延迟加载</span>
      </div>
      <div class="chart-wrapper">
        <span v-if="status" @click="lazyLoadChart">点击此处加载图形</span>
        <mpvue-echarts v-else :echarts="echarts" :onInit="initLazy" canvasId="lazy-canvas" ref="lazy" lazyLoad />
      </div>
    </div>
  </div>
</template>

<script>

import * as echarts from 'echarts'
import mpvueEcharts from 'mpvue-echarts'

export default {
  data () {
    return {
      echarts: echarts,
      initSingle: this.drawSingle,
      initLazy: this.drawLazy,
      status: true
    }
  },

  components: {
    mpvueEcharts
  },

  methods: {
    drawSingle (canvas, width, height) {
      const singleChart = echarts.init(canvas, null, {
        width: width,
        height: height
      })
      canvas.setChart(singleChart)

      var option = {
        title: {
          text: '测试下面legend的红色区域不应被裁剪',
          left: 'center'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: 'shadow'
            // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        color: ['#37A2DA', '#67E0E3', '#9FE6B8'],
        legend: {
          data: ['A', 'B', 'C'],
          top: 50,
          left: 'center',
          backgroundColor: 'red',
          z: 100
        },
        grid: {
          containLabel: true
        },

        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
          // show: false
        },
        yAxis: {
          x: 'center',
          type: 'value',
          splitLine: {
            lineStyle: {
              type: 'dashed'
            }
          }
          // show: false
        },
        series: [{
          name: 'A',
          type: 'line',
          smooth: true,
          data: [18, 36, 65, 30, 78, 40, 33]
        }, {
          name: 'B',
          type: 'line',
          smooth: true,
          data: [12, 50, 51, 35, 70, 30, 20]
        }, {
          name: 'C',
          type: 'line',
          smooth: true,
          data: [10, 30, 31, 50, 40, 20, 10]
        }]
      }

      singleChart.setOption(option)
      return singleChart
    },
    drawLazy (canvas, width, height) {
      const lazyChart = echarts.init(canvas, null, {
        width: width,
        height: height
      })
      canvas.setChart(lazyChart)

      var option = {
        color: ['#37a2da', '#32c5e9', '#67e0e3'],
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: 'shadow'
            // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        legend: {
          data: ['热度', '正面', '负面']
        },
        grid: {
          left: 20,
          right: 20,
          bottom: 15,
          top: 40,
          containLabel: true
        },
        xAxis: [
          {
            type: 'value',
            axisLine: {
              lineStyle: {
                color: '#999'
              }
            },
            axisLabel: {
              color: '#666'
            }
          }
        ],
        yAxis: [
          {
            type: 'category',
            axisTick: { show: false },
            data: ['汽车之家', '今日头条', '百度贴吧', '一点资讯', '微信', '微博', '知乎'],
            axisLine: {
              lineStyle: {
                color: '#999'
              }
            },
            axisLabel: {
              color: '#666'
            }
          }
        ],
        series: [
          {
            name: '热度',
            type: 'bar',
            label: {
              normal: {
                show: true,
                position: 'inside'
              }
            },
            data: [300, 270, 340, 344, 300, 320, 310],
            itemStyle: {
              // emphasis: {
              //   color: '#37a2da'
              // }
            }
          },
          {
            name: '正面',
            type: 'bar',
            stack: '总量',
            label: {
              normal: {
                show: true
              }
            },
            data: [120, 102, 141, 174, 190, 250, 220],
            itemStyle: {
              // emphasis: {
              //   color: '#32c5e9'
              // }
            }
          },
          {
            name: '负面',
            type: 'bar',
            stack: '总量',
            label: {
              normal: {
                show: true,
                position: 'left'
              }
            },
            data: [-20, -32, -21, -34, -90, -130, -110],
            itemStyle: {
              // emphasis: {
              //   color: '#67e0e3'
              // }
            }
          }
        ]
      }

      lazyChart.setOption(option)
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
  font-family: -apple-system-font, 'Helvetica Neue', sans-serif;
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
