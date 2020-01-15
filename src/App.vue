<template>
  <div id="app" cccc>

    <h1>年会倒计时小助手</h1>
    <!-- 初始时间控制部分 -->
    <div style="padding: 20px; border-radius: 10px; border:1px solid #e5e5e5; position: relative;"
      v-bind:class="{hide:runFlag>0, moveOut: runFlag>0}" class="shadow">

      <span>请选择要执行的倒计时时间：</span>
      <button v-for="item in initTimeList" v-bind:key="item" @click="currentSelectTime = item">
        {{item}}秒
      </button>

      <div style="margin-top: 5px;">
        <span>或者输入一个初始时间：</span>
        <input v-model="currentSelectTime" /><span>单位：秒</span>
      </div>

    </div>

    <!-- 倒计时开始区域 -->
    <div v-bind:class='{anim: runFlag>0}' style="text-align: center;margin-top: 100px;">
      <div style="display: inline-block; padding: 20px;border-radius: 20px; border:1px solid #e5e5e5; min-width: 250px;"
        class="shadow">
        <div style="text-align: center;">{{runFlag>0?'当前剩余时间为：':'当前选择的倒计时开始时间为：'}}</div>
        <div id="largeNumber"><span id="largeNumSpan">{{currentSelectTime}}</span><span
            style="font-size: 0.1em;">秒</span></div>
        <button style="font-size: 1em;" @click="start">开始</button>
        <button style="font-size: 1em; margin: 0;" @click="reset">重置</button>
      </div>
    </div>
  </div>


</template>

<script>

  export default {
    name: 'app',
    components: {
    },
    data: function () {
      return {
        initTimeList: [
          10,
          30,
          60,
          120,
          360,
        ],
        currentSelectTime: 0,
        runFlag: 0
      }
    },
    methods: {
      randomColor() {
        return `#${this.randomHex()}${this.randomHex()}${this.randomHex()}`
      },
      randomHex() {
        return Math.round(Math.random() * 125).toString(16);
      },

      start() {
        if (this.currentSelectTime <= 0) {
          return;
        }

        const largeNumberElem = document.getElementById('largeNumSpan');
        if (this.runFlag) { return; }
        this.runFlag = setInterval(() => {
          this.currentSelectTime--;
          window.requestAnimationFrame(() => {
            largeNumberElem.style.color = this.randomColor();
          })
          if (this.runFlag && this.currentSelectTime === 0) {
            this.init();
          }
        }, 1000);
      },
      reset() {
        this.init();
      },
      init() {
        this.runFlag && clearInterval(this.runFlag);
        this.runFlag = null;
        this.currentSelectTime = 0;
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    padding: 50px 60px;
    margin-top: 160px;
    max-width: 600px;
    margin: 0 auto;
  }

  button {
    display: inline-block;
    margin-right: 10px;
    border-radius: 5px;
  }

  #largeNumber~button {
    width: 60px;
    height: 30px;
  }

  #largeNumber {
    font-size: 100px;
    text-align: center;
  }

  .shadow {
    box-shadow: 10px 10px 10px #e5e5e5;
  }

  .hide {
    /* display: none; */
  }

  .anim {
    animation: moveTop 1s 1;
    -webkit-animation: moveTop 500ms 1;
    -webkit-animation-fill-mode: forwards;
    position: relative;
  }

  .moveOut {
    -webkit-animation: moveOut 500ms 1;
    -webkit-animation-fill-mode: forwards;
  }

  @-webkit-keyframes moveOut {
    from {
      left: 0;
    }

    to {
      left: 1000px;
    }
  }

  @keyframes moveTop {
    from {
      top: 0px;
    }

    to {
      top: -150px;
    }
  }

  h1 {
    margin-left: 20px;
  }
</style>