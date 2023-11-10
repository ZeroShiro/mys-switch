<template>
  <view class="warp_swicth" :style="{ transform: `scale(${scale})` }">
    <view class="switch_box" @click="change">
      <view class="switch" :class="{ isCheck: isCheck, disabled: disabled }" :style="{ backgroundColor: bgColorVal }">
        <view class="circle_potion"></view>
        <view class="textChange" v-if="onText && offText" :style="{ color: textColorVal }">
          {{ isCheck ? onText : offText }}
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  props: {
    // v-model 值
    value: {
      type: Boolean,
      default: false,
    },
    // 是否可点击
    disabled: {
      type: Boolean,
      default: false,
    },
    // 开启文字展示
    onText: {
      type: String,
      default: '',
    },
    // 关闭文字展示
    offText: {
      type: String,
      default: '',
    },
    // 缩放大小 
    scale: {
      typeof: Number,
      default: 1,
    },
    // 默认关闭背景颜色
    offBgColor: {
      type: String,
      default: '#CCCCCC',
    },
    // 默认开启背景颜色
    onBgColor: {
      type: String,
      default: '#3875F6',
    },
    // 默认关闭文字色
    offTextColor: {
      type: String,
      default: '#FFFFFF',
    },
    // 默认开启文字色
    onTextColor: {
      type: String,
      default: '#FFFFFF',
    },
    // 自定义处理值
    isAsync: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    bgColorVal() {
      return this.isCheck ? this.onBgColor : this.offBgColor;
    },
    textColorVal() {
      return this.isCheck ? this.onTextColor : this.offTextColor;
    },
  },
  beforeMount() {
    this.isCheck = this.value;
  },
  watch: {
    value(val) {
      this.isCheck = val;
    },
  },
  methods: {
    change() {
      if (!this.disabled) {
        if (this.isAsync) {
          // changeAsync(cb){
          //  请求不改变值
          //   api(check? 1:0).then(() => {
          //  完成后变化
          //       cb(); 
          //   });
          // }
          this.$emit('changeAsync', () => {
            this.isCheck = !this.isCheck;
            this.$emit('input', this.isCheck);
          });
          return;
        }
        this.isCheck = !this.isCheck;
        this.$emit('change', this.isCheck);
        this.$emit('input', this.isCheck);
      }
    },
  },
  data() {
    return {
      isCheck: false,
    };
  },
};
</script>

<style lang="scss" scoped>
$circleRpx: 40rpx; //圆形宽高 自定义
// height: 48rpx;相差8左右
.warp_swicth {
  display: inline-block;
  .switch_box {
    display: inline-block;
    height: 48rpx;
    min-width: 100rpx;
    .switch {
      background-color: #ccc;
      border-radius: 100rpx;
      width: 100%;
      height: 100%;
      transition: all 0.2s linear;
      display: flex;
      align-items: center;
      position: relative;
      .circle_potion {
        position: absolute;
        z-index: 1;
        border-radius: 50%;
        width: $circleRpx;
        height: $circleRpx;
        background-color: #fff;
        transition: all 0.2s linear;
        left: 4rpx;
      }
      .textChange {
        margin-left: $circleRpx;
        margin-right: 0;
        padding: 0 16rpx;
        color: #fff;
      }
      &.isCheck {
        background-color: red;
        .circle_potion {
          margin-left: -4rpx;
          left: 100%;
          transform: translate(-100%);
        }
        .textChange {
          margin-right: $circleRpx;
          margin-left: 0;
        }
      }
      &.disabled {
        opacity: 0.5;
      }
    }
  }
}
</style>
