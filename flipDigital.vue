<template>
  <div class="dataStatistics">
    <div class="digit_set" :class="{'set_last': index === nowNums.length -1}" v-for="(number,index) in nowNums">
      <template v-for="n in 10">
        <div class="digit" :class="{'active': (n-1) === number, 'previous': (n-1) === number -1 }">
          <div class="digit_top">
            <span class="digit_wrap">{{n-1}}</span>
          </div>
          <div class="digit_bottom">
            <span class="digit_wrap">{{n-1}}</span>
          </div>
        </div>
      </template>
    </div>
    <div id="placeholder" style="display: none"></div>
  </div>
</template>
<script>
  export default {
    name: 'digital-flip',
    data() {
      return {
        nowNums: []
      };
    },
    props: {
      begin: { // 开始数字
        type: Number,
        default: 0
      },
      end: { // 结束数字
        type: Number,
        default: 0
      },
      duration: { // 完成整个数字的变化需要的时间
        type: Number,
        default: 5
      },
      decimal: { // 保留几位小数
        type: Number,
        default: 0
      },
      digitCount: { // 总共需要显示几位数字
        type: Number,
        default: 0
      },
      formatFuc: {
        type: Function,
        default: this.formatNumber
      }
    },
//    computed: {
//      nowNums: function() {
//      }
//    },
    watch: {
      end: {
        handler: function(value) {
          console.log('---->' + 'this.begin' + this.begin + ' this.end' + this.end);
          let option = {
            formattingFn: this.formatNumber
          };
          var numAnim = new CountUp('placeholder', this.begin, this.end, this.decimal, this.duration, option);
          numAnim.start();
        },
        immediate: true
      }
    },
    created: function() {
    },
    mounted: function() {
    },
    methods: {
      formatNumber(value) {
        let length = this.digitCount === 0 ? value.toString().split('').length : this.digitCount;
        this.nowNums = this.zfill(value + '', length).toString().split('').map((x) => Number(x));
      },
      zfill(num, size) {
        var s = '000000000' + num;
        return s.substr(s.length - size);
      }
    }
  };
</script>
<style lang="scss" rel="stylesheet/scss">
  @import "../assets/css/base/fn.scss";
  $digital-fontsize: 50px;
  $digital-number-width: 32px; // 数字宽度
  $digital-lineheight: 60px; // 数字字体高度
  $digital-height: 62px; // 数字高度（加1px上下边框）
  $digital-font-padding: 2px; // 数字内边距
  $digital-radious: 5px; // 圆角

  .dataStatistics {
    display: inline-block;
    color: $blue41;
    font-family: "Helvetica Neue", Helvetica, sans-serif;
    font-size: $digital-fontsize;
    font-weight: bold;
    line-height: $digital-lineheight;
    height: $digital-height;
    text-align: center;
    vertical-align: middle;
    .digit_set {
      float: left;
      border-radius: $digital-radious;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.8);
      border: 1px solid #111;
      width: $digital-number-width;
      height: 100%;
      display: inline-block;
      position: relative;
      margin: 0 1px;
      .digit {
        position: absolute;
        height: 100%;
        .digit_top {
          width: $digital-number-width;
          background-color: $blue43;
          border-bottom: 1px solid $blue43;
          box-sizing: border-box;
          top: 0;
          z-index: 0;
          border-radius: $digital-radious $digital-radious 0 0;
          .digit_wrap {
            line-height: $digital-lineheight;
            display: block;
            overflow: hidden;
          }
          &::before {
            content: '';
            height: 100%;
            width: 100%;
            position: absolute;
            left: 0;
            top: 0;
          }
        }
        .digit_bottom {
          background-color: $blue43;
          bottom: 0;
          z-index: 0;
          border-radius: 0 0 $digital-radious $digital-radious;
          .digit_wrap {
            display: block;
            margin-top: -($digital-lineheight)/2;
          }
          &::before {
            content: '';
            border-radius: 0 0 $digital-radious $digital-radious;
            height: 100%;
            width: 100%;
            position: absolute;
            left: 0;
            top: 0;
          }
        }
        .digit_wrap {
          line-height: $digital-lineheight;
          display: block;
          overflow: hidden;
        }
        div {
          position: absolute;
          left: 0;
          overflow: hidden;
          height: 50%;
          width: $digital-number-width;
          padding: 0 $digital-font-padding;
        }
      }
      .digit.previous {
        .digit_top {
          opacity: 1;
          z-index: 2;
          transform-origin: 50% 100%;
          animation: flipTop 0.3s ease-in both;
        }
        .digit_bottom {
          z-index: 1;
          opacity: 1;
        }
      }
      .digit.active {
        .digit_top {
          z-index: 1;
        }
        .digit_bottom {
          z-index: 2;
          transform-origin: 50% 0%;
          animation: flipBottom 0.3s 0.3s ease-out both;
        }
      }
    }

    @keyframes flipTop {
      0% {
        transform: perspective(400px) rotateX(0deg);
      }
      100% {
        transform: perspective(400px) rotateX(-90deg);
      }
    }

    @keyframes flipBottom {
      0% {
        transform: perspective(400px) rotateX(90deg);
      }
      100% {
        transform: perspective(400px) rotateX(0deg);
      }
    }
  }
</style>
