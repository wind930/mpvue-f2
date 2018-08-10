<template>
  <div class="page">
    <a v-for="(chart,key) in charts" :key="key" class="list-item" :href="'pages/'+chart.name+'/main'">
      <image :src="'../../static/img/'+chart.name+'.jpg'" :data-page="chart.name" mode="aspectFit" />
      {{chart.value}}
    </a>
  </div>
</template>

<script>

export default {
  data () {
    return {
      charts: [
        { name: 'wx-echarts', value: 'echarts' },
        { name: 'wx-f2', value: 'f2' }
      ]
    }
  },

  components: {},

  methods: {},

  created () {}
}
</script>

<style scoped>

.page {
  padding: 10px;
  margin: 0;
  list-style: none;

  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;

  -webkit-flex-flow: row wrap;
  justify-content: space-between;
  font-family: -apple-system-font, "Helvetica Neue", sans-serif;
}

.list-item {
  padding: 5px;
  width: 100px;
  height: 150px;
  text-align: center;
  font-size: 12px;
  margin-top: 10px;
}

.list-item image {
  box-sizing: border-box;
  width: 100%;
  height: 100px;
  padding: 12px;
  background-color: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
  display: block;
  margin: 0 auto;
  margin-bottom: 10px;
}

</style>
