<template>
  <div class="vue-ios-picker">
    <div class="picker-mask" :class="{'show': display}" @click="cancelHandler"></div>
    <div class="picker-panel" :class="{'show': display}">
      <div class="picker-choose">
        <span class="cancel" @click="cancelHandler">取消</span>
        <span class="confirm" @click="confirmHandler" :style="{'color': themeColor}">确定</span>
        <h1 class="picker-title">{{title}}</h1>
      </div>
      <div class="picker-content">
        <div class="mask-top border-1px"></div>
        <div class="mask-bottom border-1px"></div>
        <div class="wheel-wrapper wheel-wrapper-hook">
          <picker-item
            v-for="(column, index) in items"
            :key="index"
            :index="index"
            :selected-index="selected.index[index]"
            :items="column"
            @change="columnChange"
            ref="pickerItem"
          ></picker-item>
        </div>
      </div>
      <div class="picker-footer footer-hook"></div>
    </div>
  </div>
</template>

<script>
  import PickerItem from './PickerItem'

  export default {
    props: {
      themeColor: {
        type: String,
        default: '#fa8919'
      },
      title: {
        type: String
      },
      items: {
        type: Array,
        require: true
      },
      selectedIndex: {
        type: Array,
        default: function () {
          return this.items.map(() => 0)
        }
      }
    },
    data () {
      return {
        display: false,
        selected: {
          index: [],
          value: []
        },
        wheels: null
      }
    },
    created () {
      this.selected.index = this.selectedIndex
    },
    methods: {
      columnChange (index, currentIndex) {
        this.$emit('change', index, currentIndex)
      },
      show (next) {
        this.$el.style.display = 'block'
        window.setTimeout(() => {
          this.display = true
          this.$refs.pickerItem.forEach(item => {
            item.createWheel()
          })
          next && next()
        }, 0)
      },
      hide () {
        this.display = false
        window.setTimeout(() => {
          this.$el.style.display = 'none'
          this.$refs.pickerItem.forEach(item => {
            item.wheel.disable()
          })
        }, 500)
      },
      scrollColumn (index, dist) {
        if (index >= 0 && index < this.items.length && dist) {
          if (dist < 0) {
            dist = 0
          } else if (dist >= this.items[index].length) {
            dist = this.items[index].length - 1
          }
        }
        if (this.$refs.pickerItem[index]) {
          this.$refs.pickerItem[index].scrollColumn(dist)
        }
      },
      confirmHandler () {
        this.hide()
        let changed = false
        this.$refs.pickerItem.forEach((item, i) => {
          let index = item.wheel.getSelectedIndex()
          this.selected.index[i] = index
          let value = null
          if (this.items[i].length) {
            value = this.items[i][index].value
          }
          if (this.selected.value[i] !== value) {
            changed = true
          }
          this.selected.value[i] = value
        })
        this.$emit('select', this.selected.value, this.selected.index)
        if (changed) {
          this.$emit('value-change', this.selected.value, this.selected.index)
        }
      },
      cancelHandler () {
        this.hide()
        this.$emit('cancel')
      }
    },
    components: {
      PickerItem
    }
  }
</script>

<style lang="scss" scoped>
  // mixins
  @mixin gradient-from-bottom($from, $to) {
    background: -moz-linear-gradient(bottom, $from, $to);
    background: -webkit-gradient(linear, left bottom, left top, from($from), to($to));
    background: -o-linear-gradient(bottom, $from, $to);
  }

  @mixin gradient-from-top($from, $to) {
    background: -moz-linear-gradient(top, $from, $to);
    background: -webkit-gradient(linear, left top, left bottom, from($from), to($to));
    background: -o-linear-gradient(top, $from, $to);
  }

  @mixin unselectable() {
    -moz-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  @mixin flex-box() {
    display: -ms-flexbox;
    display: -webkit-box;
    display: -webkit-flex;
    display: flex;
  }

  @mixin flex() {
    -ms-flex: 1 1 0.000000001px;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    -webkit-flex-basis: 0.000000001px;
    flex-basis: 0.000000001px;
    width: 1%;
  }

  @mixin border-top1px($color) {
    &:before, &:after {
      display: block;
      position: absolute;
      border-top: 1px solid $color;
      left: 0;
      width: 100%;
      content: ' ';
    }
    &:before {
      display: block;
      top: 0;
    }
    &:after {
      display: none;
      bottom: 0;
    }
  }

  @mixin border-bottom1px($color) {
    &:before, &:after {
      display: block;
      position: absolute;
      border-top: 1px solid $color;
      left: 0;
      width: 100%;
      content: ' ';
    }
    &:before {
      display: none;
      top: 0;
    }
    &:after {
      display: block;
      bottom: 0;
    }
  }

  // style
  .vue-ios-picker {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    width: 100%;
    height: 100%;
    overflow: hidden;
    text-align: center;
    font-family: 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif, 'Droid Sans Fallback';
    font-size: 14px;
    @include unselectable();
    .picker-mask {
      position: absolute;
      z-index: 500;
      width: 100%;
      height: 100%;
      transition: all 0.5s;
      -webkit-transition: all 0.5s;
      background: rgba(0, 0, 0, 0);
      background: rgba(0, 0, 0, 0.4);
      opacity: 0;

      &.show {
        background: rgba(0, 0, 0, 0.6);
        opacity: 1;
      }
    }
    .picker-panel {
      position: absolute;
      z-index: 600;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 243px;
      background: #fff;
      transform: translateY(243px);
      -webkit-transform: translateY(243px);
      transition: all 0.5s;
      -webkit-transition: all 0.5s;
      &.show {
        transform: translateY(0);
        -webkit-transform: translateY(0);
      }
      .picker-choose {
        background-color: #F9F9F9;
        border-bottom: 1px solid #ebebeb;
        position: relative;
        box-sizing: border-box;
        height: 50px;
        color: #878787;
        font-size: 14px;

        .picker-title {
          line-height: 50px;
          font-size: 19px;
          text-align: center;
          color: #333;
        }

        .confirm, .cancel {
          font-size: 16px;
          position: absolute;
          padding: 10px 12px;
          top: 4px;
        }

        .confirm {
          right: 0;
          color: #fa8919;
        }

        .cancel {
          left: 0;
        }
      }

      .picker-content {
        position: relative;

        .mask-top, .mask-bottom {
          background-color: rgba(255, 255, 255, 0.5);
          position: absolute;
          z-index: 10;
          width: 100%;
          height: 68px;
          pointer-events: none;
          transform: translateZ(0);
          -webkit-transform: translateZ(0);
        }

        .mask-top {
          @include border-bottom1px(#ccc);
          top: 0;
          @include gradient-from-bottom(rgba(255, 255, 255, 0.4), rgba(255, 255, 255, 0.8));
        }

        .mask-bottom {
          @include border-top1px(#ccc);
          bottom: 0;
          @include gradient-from-top(rgba(255, 255, 255, 0.4), rgba(255, 255, 255, 0.8));
        }
      }

      .wheel-wrapper {
        @include flex-box();
        padding: 0 10px;
      }
    }

    .picker-footer {
      height: 20px;
    }
  }

  @media (-webkit-min-device-pixel-ratio: 1.5), (min-device-pixel-ratio: 1.5) {
    .border-1px {
      &::after, &::before {
        transform: scaleY(.7);
        transform-origin: 0 0;
        transform: scaleY(.7);
      }

      &::after {
        transform-origin: left bottom
      }
    }
  }

  @media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {
    .border-1px {
      &::after, &::before {
        -webkit-transform: scaleY(.5);
        transform: scaleY(.5);
      }
    }
  }

  // iPhoneX Compatible
  @media only screen and (device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) {
    .vue-ios-picker {
      height: calc(100% - 34px);
    }
  }
</style>
