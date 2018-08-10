# 在微信小程序中使用 F2

> 本项目是 F2 的 Mpvue 小程序版本。开发者可以通过熟悉的 F2 配置方式及 Vue 语法，快速开发图表，满足各种可视化需求。

## 安装

``` bash
npm i mpvue-f2
```

## 使用

``` vue
<template>
  <div class="echarts-wrap">
    <mpvue-f2 :f2="f2" :onInit="onInit" canvasId="demo-canvas" />
  </div>
</template>

<script>
import mpvueF2 from 'mpvue-f2'
const F2 = require('@antv/f2')

let chart = null

function initChart (canvas, width, height) {
  const data = [{ 'year': 1997, 'type': 'United States', 'value': 4.9 }, { 'year': 1997, 'type': 'Florida', 'value': 4.8 }, { 'year': 1998, 'type': 'United States', 'value': 4.5 }, { 'year': 1998, 'type': 'Florida', 'value': 4.3 }, { 'year': 1999, 'type': 'United States', 'value': 4.2 }, { 'year': 1999, 'type': 'Florida', 'value': 3.9 }, { 'year': 2000, 'type': 'United States', 'value': 4 }, { 'year': 2000, 'type': 'Florida', 'value': 3.7 }, { 'year': 2001, 'type': 'United States', 'value': 4.7 }, { 'year': 2001, 'type': 'Florida', 'value': 4.7 }, { 'year': 2002, 'type': 'United States', 'value': 5.8 }, { 'year': 2002, 'type': 'Florida', 'value': 5.6 }, { 'year': 2003, 'type': 'United States', 'value': 6 }, { 'year': 2003, 'type': 'Florida', 'value': 5.2 }, { 'year': 2004, 'type': 'United States', 'value': 5.5 }, { 'year': 2004, 'type': 'Florida', 'value': 4.6 }, { 'year': 2005, 'type': 'United States', 'value': 5.1 }, { 'year': 2005, 'type': 'Florida', 'value': 3.7 }, { 'year': 2006, 'type': 'United States', 'value': 4.6 }, { 'year': 2006, 'type': 'Florida', 'value': 3.2 }, { 'year': 2007, 'type': 'United States', 'value': 4.6 }, { 'year': 2007, 'type': 'Florida', 'value': 4 }, { 'year': 2008, 'type': 'United States', 'value': 5.8 }, { 'year': 2008, 'type': 'Florida', 'value': 6.3 }, { 'year': 2009, 'type': 'United States', 'value': 9.3 }, { 'year': 2009, 'type': 'Florida', 'value': 10.4 }, { 'year': 2010, 'type': 'United States', 'value': 9.6 }, { 'year': 2010, 'type': 'Florida', 'value': 11.1 }, { 'year': 2011, 'type': 'United States', 'value': 8.9 }, { 'year': 2011, 'type': 'Florida', 'value': 10 }, { 'year': 2012, 'type': 'United States', 'value': 8.1 }, { 'year': 2012, 'type': 'Florida', 'value': 8.5 }, { 'year': 2013, 'type': 'United States', 'value': 7.4 }, { 'year': 2013, 'type': 'Florida', 'value': 7.2 }, { 'year': 2014, 'type': 'United States', 'value': 6.2 }, { 'year': 2014, 'type': 'Florida', 'value': 6.3 }, { 'year': 2015, 'type': 'United States', 'value': 5.3 }, { 'year': 2015, 'type': 'Florida', 'value': 5.4 }, { 'year': 2016, 'type': 'United States', 'value': 4.9 }, { 'year': 2016, 'type': 'Florida', 'value': 4.9 }, { 'year': 2017, 'type': 'United States', 'value': 4.4 }, { 'year': 2017, 'type': 'Florida', 'value': 4.3 }]

  chart = new F2.Chart({
    el: canvas,
    width,
    height
  })

  chart.source(data, {
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

  chart.tooltip({
    custom: true,
    showCrosshairs: true,
    onChange (obj) {
      const legend = chart.get('legendController').legends.top[0]
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
      const legend = chart.get('legendController').legends.top[0]
      legend.setItems(chart.getLegendItems().country)
    }
  })

  chart.guide().rect({
    start: [2011, 'max'],
    end: ['max', 'min'],
    style: {
      fill: '#CCD6EC',
      opacity: 0.3
    }
  })
  chart.guide().text({
    position: [2014, 'max'],
    content: 'Scott administratio\n(2011 to present)',
    style: {
      fontSize: 10,
      textBaseline: 'top'
    }
  })

  chart.line().position('year*value').color('type', val => {
    if (val === 'United States') {
      return '#ccc'
    }
  })
  chart.render()
  return chart
}

export default {
  components: {
    mpvueF2
  },
  data () {
    return {
      f2: F2,
      onInit: initChart
    }
  }
}
</script>

<style scoped>
.echarts-wrap {
  width: 100%;
  height: 300px;
}
</style>

```

这对于所有 F2 图表都是通用的，用户只需要修改上面 `initChart` 的内容，即可改变图表。

本项目只展示了部分 demos，更全的见 [AntV F2](https://antv.alipay.com/zh-cn/f2/3.x/demo/index.html)。

完整的例子请参见 [examples](https://github.com/soonfy/mpvue-f2/tree/examples) 分支。

## 属性

| 名称          | 类型           | 默认值         | 描述           |
| -------------|--------------- | ------------- | ------------- |
| echarts      | Object         | null          | echarts 对象  |
| canvasId     | String         | ec-canvas     | canvasId      |
| onInit       | Function       | null          | 初始化函数     |
| lazyLoad     | Boolean        | false         | 懒加载         |
| disableTouch | Boolean        | false         | 禁用触摸事件   |
| throttleTouch| Boolean        | false         | 节流触摸事件   |

## 微信版本要求

支持微信版本 >= 6.6.3，对应基础库版本 >= 1.9.91。

调试的时候，需要在微信开发者工具中，将“详情”下的“调试基础库”设为 1.9.91 及以上版本。

发布前，需要在 [https://mp.weixin.qq.com](https://mp.weixin.qq.com) 的“设置”页面，将“基础库最低版本设置”设为 1.9.91。当用户微信版本过低的时候，会提示用户更新。

## 暂不支持的功能

- Tooltip

## License

[MIT](LICENSE)
