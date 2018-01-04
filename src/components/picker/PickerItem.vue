<template>
  <div class="wheel wheel-hook">
    <ul class="wheel-scroll wheel-scroll-hook">
        <li class="wheel-item" v-for="item in items" :key="item.value" :data-val="item.value">{{item.text}}</li>
    </ul>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    props: {
      index: {
        type: Number,
        require: true
      },
      items: {
        type: Array,
        require: true
      },
      selectedIndex: {
        type: Number,
        default: 0
      }
    },
    data () {
      return {
        wheel: null,
        selected: {
          index: 0
        },
        wheelNeedUpdate: false,
        oldItems: []
      }
    },
    created () {
      this.selected.index = this.selectedIndex
    },
    updated () {
      if (this.wheelNeedUpdate) {
        let scrollEl = this.$el.querySelectorAll('.wheel-scroll-hook')
        if (scrollEl && this.wheel) {
          let oldData = this.oldItems
          let selectedIndex = this.wheel.getSelectedIndex()
          let dist = 0
          if (oldData.length) {
            let oldValue = oldData[selectedIndex].value
            for (let i = 0; i < this.items.length; i++) {
              if (this.items[i] && this.items[i].value === oldValue) {
                dist = i
                break
              }
            }
          }
          this.selected.index = dist
          this.wheel.refresh()
          this.wheel.wheelTo(dist)
          this.wheelNeedUpdate = false
          this.oldItems = []
        }
      }
    },
    methods: {
      createWheel (index = this.selectedIndex, dist = this.selected.index) {
        this.wheel = new BScroll(this.$el, {
          wheel: true,
          selectedIndex: index
        })
        ;(() => {
          this.wheel.on('scrollEnd', () => {
            let currentIndex = this.wheel.getSelectedIndex()
            if (this.selected.index !== currentIndex) {
              this.selected.index = currentIndex
              this.$emit('change', this.index, currentIndex)
            }
          })
        })()
        this.wheel.enable()
        this.wheel.wheelTo(dist)
      },
      scrollColumn (dist) {
        if (this.wheel) {
          this.wheel.wheelTo(dist)
          let currentIndex = this.wheel.getSelectedIndex()
          if (this.selected.index !== currentIndex) {
            this.selected.index = currentIndex
            this.$emit('change', this.index, currentIndex)
          }
        }
      }
    },
    watch: {
      items: {
        handler: function (val, old) {
          this.oldItems = old
          this.wheelNeedUpdate = true
        },
        deep: true
      }
    }
  }
</script>

<style lang="scss" scoped>
  @mixin flex() {
    -ms-flex: 1 1 0.000000001px;
    -webkit-box-flex: 1;
    -webkit-flex: 1;
    flex: 1;
    -webkit-flex-basis: 0.000000001px;
    flex-basis: 0.000000001px;
    width: 1%;
  }

  .wheel {
    @include flex();
    height: 173px;
    overflow: hidden;
    font-size: 21px;

    .wheel-scroll {
      margin-top: 68px;
      line-height: 36px;

      .wheel-item {
        height: 36px;
        overflow: hidden;
        white-space: nowrap;
        color: #333;
      }
    }
  }
</style>
