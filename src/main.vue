<template>
    <div class="container flip-clock">
        <template v-for="data in timeData">
            <span v-bind:key="data.label" class="flip-clock__piece" :id="data.elementId">
                <span class="flip-clock__card flip-card">
                <b class="flip-card__top">{{data.current}}</b>
                <b class="flip-card__bottom" v-bind:data-value="data.current"></b>
                <b class="flip-card__back" v-bind:data-value="data.previous"></b>
                <b class="flip-card__back-bottom" v-bind:data-value="data.previous "></b>
                </span>
            </span>
        </template>
    </div>
</template>

<script>
export default {
  name: 'flipCountdown',
  props: {
    deadline: {
      type: Number
    },
    length: {
      type: Number
    }
  },
  data () {
    return {
      timeData: [
      ]
    }
  },
  created () {
    if (!this.deadline) {
      throw new Error("Missing props 'deadline'")
    }
    this.changeNum()
  },
  watch: {
    deadline (value) {
      // this.changeNum()
      let dataList = this.digitize(this.deadline)
      if (this.length < dataList.length) {
        throw new Error("Props 'deadline' length must be greater than 'length'")
      }
      if (dataList.length < this.length) {
        let length = dataList.length
        for (let i = 0; i < this.length - length; i++) {
          dataList.unshift(0)
        }
      }
      this.timeData.forEach((data, index) => {
        data.elementId = 'flip-card-seconds' + index
      })
      dataList.forEach((data, index) => {
        this.updateTime(index, data)
      })
    }
  },
  methods: {
    digitize (n) {  // 接受一个number类参数，拆分成一个数组并返回
      var str = n + ''   // 加上空字符中，把接收的参数转换为字符串
      var arr = []        // 声明结果空数组，稍后返回
      str.split('').forEach(function (item) {   // 调用split，以空字符串为分隔符切割字符串并返回数组，在数组上调用forEach方法
        arr.push(parseInt(item))    // 对传入的每个字符用pasreInt转换为数字并压入arr数组
      })
      return arr  // 返回结果数组
    },
    addLength (list) {
      if (list.length < this.length) {
        let length = list.length
        for (let i = 0; i < this.length - length; i++) {
          let a = {
            current: 0,
            previous: 0,
            elementId: 'flip-card-seconds'
          }
          list.unshift(a)
        }
      }
      return list
    },
    changeNum () {
      let List = this.digitize(this.deadline)
      this.timeData = []
      List.forEach((data, index) => {
        let a = {
          current: data,
          previous: data,
          elementId: 'flip-card-seconds' + index
        }
        this.timeData.push(a)
      })
      this.timeData = this.addLength(this.timeData)
      this.timeData.forEach((data, index) => {
        data.elementId = 'flip-card-seconds' + index
      })
    },
    updateTime (idx, newValue) {
      if (idx >= this.timeData.length || newValue === undefined) {
        return
      }

      if (window['requestAnimationFrame']) {
        this.frame = requestAnimationFrame(this.updateTime.bind(this))
      }

      const d = this.timeData[idx]
      const val = (newValue < 0 ? 0 : newValue)

      if (val !== d.current) {
        d.previous = d.current
        d.current = val

        const el = document.querySelector(`#${d.elementId}`)
        if (el) {
          el.classList.remove('flip')
          void el.offsetWidth
          el.classList.add('flip')
        }
      }
    }
  },
  beforeDestroy () {
    if (window['cancelAnimationFrame']) {
      cancelAnimationFrame(this.frame)
    }
  }
}
</script>

<style scoped lang="less">
.flip-clock {
  text-align: center;
  perspective: 600px;
  margin: 0 auto;

  *,
  *:before,
  *:after { box-sizing: border-box; }
}


.flip-clock__piece {
  display: inline-block;
  margin: 0 0.2vw;
  
  @media (min-width: 1000px) {
    margin: 0 5px;
  }
}

.flip-clock__slot {
  font-size: 1rem;
  line-height: 1.5;
  display: block;
}

@halfHeight: 0.72em;
@borderRadius: 0.15em;

.flip-card {
  display: block;
  position: relative;
  padding-bottom: @halfHeight;
  font-size: 2.25rem;
  line-height: 0.95;
}

@media (min-width: 1000px) {
  .flip-clock__slot { font-size: 1.2rem; }
  .flip-card { font-size: 3rem; }
}

.flip-card__top,
.flip-card__bottom,
.flip-card__back-bottom,
.flip-card__back::before,
.flip-card__back::after {
  display: block;
  height: @halfHeight;
  color: #31bacc;
  background: #222;
  padding: 0.23em 0.15em 0.4em;
  border-radius: @borderRadius @borderRadius 0 0;
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  transform-style: preserve-3d;
  width: 1em;
  height: @halfHeight;
}

.flip-card__bottom,
.flip-card__back-bottom {
  color: #1b9eee;
  position: absolute;
  top: 50%;
  left: 0;
  border-top: solid 1px #000;
  background: #393939;
  border-radius: 0 0 @borderRadius @borderRadius;
  pointer-events: none;
  overflow: hidden;
  z-index: 2;
}

.flip-card__back-bottom {
  z-index: 1;
}

.flip-card__bottom::after,
.flip-card__back-bottom::after {
  display: block;
  margin-top: -@halfHeight;
}

.flip-card__back::before,
.flip-card__bottom::after,
.flip-card__back-bottom::after {
  content: attr(data-value);
}

.flip-card__back {
  position: absolute;
  top: 0;
  height: 100%;
  left: 0%;
  pointer-events: none;
}

.flip-card__back::before {
  position: relative;
  overflow: hidden;
  z-index: -1;
}

.flip .flip-card__back::before {
  z-index: 1;
  animation: flipTop 0.3s cubic-bezier(.37,.01,.94,.35);
  animation-fill-mode: both;
  transform-origin: center bottom;
}

.flip .flip-card__bottom {
  transform-origin: center top;
  animation-fill-mode: both;
  animation: flipBottom 0.6s cubic-bezier(.15,.45,.28,1);
}

@keyframes flipTop {
  0% {
    transform: rotateX(0deg);
    z-index: 2;
  }
  0%, 99% {
    opacity: 1;
  }
  100% {
    transform: rotateX(-90deg);
    opacity: 0;
  }
}

@keyframes flipBottom {
  0%, 50% {
    z-index: -1;
    transform: rotateX(90deg);
    opacity: 0;
  }
  51% {
    opacity: 1;
  }
  100% {
    opacity: 1;
    transform: rotateX(0deg);
    z-index: 5;
  }
}
</style>
