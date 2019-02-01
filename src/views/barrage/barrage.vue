<!-- @format -->

<script>
export default {
  name: 'barrage', // vue component name
  prop: {
    //显示区域高度
    msg: ['msg']
  },
  data() {
    return {
      msg: null, //弹幕文字
      top: null, //距离顶部高度
      dzState: true, //控制图片替换
      screenHeight: 600, //显示区域
      barrageData: [] //存取数据
    };
  },
  mounted() {
    this.$nextTick(() => {
      setTimeout(this.timer, 180000); //每3分钟清一次数据，减少缓存
    });
  },
  methods: {
    /**
     * @setData 发送弹幕事件
     * @obj 每一条弹幕
     */
    setData() {
      if (this.msg) {
        let num = Math.random() * this.screenHeight - 64;
        this.top = Math.abs(Math.floor(num)); //随机获取高度
        let r = Math.floor(Math.random() * 256);
        let g = Math.floor(Math.random() * 256);
        let b = Math.floor(Math.random() * 256);
        this.color = '#' + r.toString(16) + g.toString(16) + b.toString(16); //随机获取字体颜色
        let obj = { msg: this.msg, top: this.top, color: this.color, dzState: this.dzState };
        this.barrageData.push(obj);
        this.msg = '';
      }
    },
    /**
     * @dzData 点击图片事件
     * @el 当前点击的图片
     */
    dzData(el) {
      el.dzState = !el.dzState;
    },
    timer() {
      this.barrageData = [];
    }
  }
};
</script>

<template>
  <div>
     <div class="main" :style="{height:screenHeight + 'px'}">
      <div
        class="barrage"
        v-for="el in barrageData"
        :key="el.id"
        :style="{top:el.top + 'px'}"
      >
        <div class="barrage-box">
          <div class="z p">
            <a :style="{color:el.color}">{{el.msg}}</a>
          </div>
          <div class="close z" :class="el.dzState ? 'bx1' : 'bx2'" @click="dzData(el)"></div>
        </div>
      </div>
    </div>
    <div class="barrage-input">
      <input
        type="text"
        class="b-input"
        placeholder="请输入弹幕内容"
        v-model.trim="msg"
        @keyup.enter="setData()"
      >
    </div>
  </div>
</template>

<style scoped>
/*组件主样式*/
.main {
  position: relative;
  width: 100%;
  margin: 0 auto;
  background: #ddd;
  overflow-x: hidden;
}
.barrage {
  position: absolute;
  top: 25px;
  right: -1000px;
  display: inline-block;
  z-index: 999;
  animation: go 12s ease-in;
  animation-iteration-count: 1;
}
.barrage:hover {
  animation-play-state: paused;
}
.barrage-box {
  background-color: rgba(0, 0, 0, 0.2);
  padding-right: 8px;
  height: 32px;
  display: inline-block;
  border-radius: 25px;
  cursor: pointer;
  overflow: hidden;
}
.barrage-box:hover {
  background-color: rgba(0, 0, 0, 0.5);
}
.barrage-box .portrait {
  margin: 0 0 0 4px;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  overflow: hidden;
  float: left;
}
.barrage-box .portrait img {
  width: 100%;
  height: 100%;
}
.barrage .z {
  float: left !important;
}
.barrage-box div.p a {
  margin-right: 7px;
  font-size: 16px;
  color: #fff;
  margin-left: 18px;
  line-height: 34px;
  text-decoration: none;
}
.barrage .close {
  display: inline-block;
  width: 25px;
  height: 25px;
  border-radius: 50%;
  margin: 3px 1px 2px 6px;
  cursor: pointer;
  overflow: hidden;
}
.bx1 {
  background: url('./finger-heart1.png') no-repeat;
}
.bx2 {
  background: url('./finger-heart2.png') no-repeat;
}
.barrage-input {
  width: 200px;
  height: 30px;
  margin: 0 auto;
  margin-top: 20px;
  text-align: center;
  position: relative;
}
.barrage-input .b-input {
  position: relative;
  width: 200px;
  height: 30px;
  line-height: 30px;
  font-size: 16px;
  border: 1px solid #d1d1d1;
  border-radius: 4px;
  padding-left: 10px;
}
@keyframes go {
  from {
    right: 0%;
  }
  to {
    right: 100%;
  }
}
</style>
