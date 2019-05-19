<template>
  <canvas
    v-if="canvasId"
    class="f2-canvas"
    :id="canvasId"
    :canvasId="canvasId"
    @touchstart="touchStart"
    @touchmove="touchMove"
    @touchend="touchEnd"
    @error="error">
  </canvas>
</template>

<script>
import Renderer from './lib/renderer';
import F2 from './lib/f2';
import './interaction/index';
// override
F2.Util.measureText = function (text, font) {
  ctx.font = font || '12px sans-serif';
  return ctx.measureText(text);
};

F2.Util.addEventListener = function (source, type, listener) {
  source.addListener(type, listener);
};

F2.Util.removeEventListener = function (source, type, listener) {
  source.removeListener(type, listener);
};

F2.Util.createEvent = function (event, chart) {
  const type = event.type;
  let x = 0;
  let y = 0;
  const touches = event.touches;
  if (touches && touches.length > 0) {
    x = touches[0].x;
    y = touches[0].y;
  }

  return {
    type,
    chart,
    x,
    y
  };
};

export default {
  data : {
    message : undefined
  },
  props: {
    type: {
      required: true,
      type: Object,
      default() {
        return null;
      },
    },
    f2: {
      required: true,
      type: Object,
      default() {
        return null;
      },
    },
    onInit: {
      required: true,
      type: Function,
      default: null,
    },
    canvasId: {
      type: String,
      default: 'f2-canvas',
    },
    lazyLoad: {
      type: Boolean,
      default: false,
    }
  },
  onReady() {
    if (!this.f2) {
      console.warn('组件需绑定 f2 变量，例：<mpvue-f2 id="mychart-dom-bar" :onInit="onInit" '
        + 'canvas-id="mychart-bar" :f2="f2"></mpvue-f2>');
      return;
    }

    // 通过update事件更新数据，重绘图表
    let self = this
    this.$parent.$on('update',function(msg){
       if(msg.type == self.type){
         self.message = msg.data
         self.init()
       }
    })

    if (!this.lazyLoad) this.init();
  },
  methods: {
    init() {
      let self = this;
      const version = wx.version.version.split('.').map(n => parseInt(n, 10));
      const isValid = version[0] > 1 || (version[0] === 1 && version[1] > 9)
        || (version[0] === 1 && version[1] === 9 && version[2] >= 91);
      if (!isValid) {
        console.error('微信基础库版本过低，需大于等于 1.9.91。'
          + '参见：https://github.com/antvis/wx-f2#%E5%BE%AE%E4%BF%A1%E7%89%88%E6%9C%AC%E8%A6%81%E6%B1%82');
        return;
      }

      if (!this.onInit) {
        console.warn('请传入 onInit 函数进行初始化');
        return;
      }

      const { canvasId } = this;
      this.ctx = wx.createCanvasContext(canvasId);

      const canvas = new Renderer(this.ctx);
      this.canvas = canvas;

      const query = wx.createSelectorQuery();
      query.select(`#${canvasId}`).boundingClientRect((res) => {
        if (!res) {
          setTimeout(() => this.init(), 50);
          return;
        }
        if (typeof callback === 'function') {
          this.chart = callback(canvas, res.width, res.height);
        } else if (this.onInit) {
          this.chart = this.onInit(canvas, res.width, res.height, self.message);
        }
      }).exec();
    },
    canvasToTempFilePath(opt) {
      const { canvasId } = this;
      this.ctx.draw(true, () => {
        wx.canvasToTempFilePath({
          canvasId,
          ...opt,
        });
      });
    },
    touchStart(e) {
       if (this.canvas) {
         this.canvas.emitEvent('touchstart', [e]);
       }
    },
    touchMove(e) {
      if (this.canvas) {
        this.canvas.emitEvent('touchmove', [e]);
      }
    },
    touchEnd(e) {
      if (this.canvas) {
        this.canvas.emitEvent('touchend', [e]);
      }
    },
    press(e) {
      if (this.canvas) {
        this.canvas.emitEvent('press', [e]);
      }
    }
  },
};
</script>

<style scoped>
.f2-canvas {
  width: 100%;
  height: 100%;
}
</style>
