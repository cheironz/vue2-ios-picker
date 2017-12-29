<template>
  <div id="app">
    <picker
      theme-color="#0BBE06"
      :items="pickerItems"
      :selected-index="[0, 3, 2]"
      ref="picker"
      @change="change"
      @select="select"
      @cancel="cancel"
      @value-change="valueChange"></picker>
    <button @click="test">test</button>
    <p>{{result}}</p>
  </div>
</template>

<script>
import Picker from './components/picker/index'

let data1 = [
  {
    text: '小美',
    value: 1
  }, {
    text: '猪猪',
    value: 2
  }
]

let data2 = [
  {
    text: '张三',
    value: 1
  },
  {
    text: '李四',
    value: 2
  },
  {
    text: '王五',
    value: 3
  },
  {
    text: '赵六',
    value: 4
  },
  {
    text: '吴七',
    value: 5
  },
  {
    text: '陈八',
    value: 6
  },
  {
    text: '杜九',
    value: 7
  },
  {
    text: '黄十',
    value: 8
  },
  {
    text: '呵呵',
    value: 9
  },
  {
    text: '哈哈',
    value: 10
  },
  {
    text: '嘿嘿',
    value: 11
  },
  {
    text: '啦啦',
    value: 12
  }
]

let data3 = [
  {
    text: '开心',
    value: 1
  }, {
    text: '生气',
    value: 2
  },
  {
    text: '搞笑',
    value: 3
  }, {
    text: '难过',
    value: 4
  }
]

export default {
  name: 'app',
  data () {
    return {
      pickerItems: [data1, data2, data3],
      result: ''
    }
  },
  methods: {
    test () {
      this.$refs.picker.show()
    },
    change (index, selectedIndex) {
      this.result = `change: index - ${index}, selectedIndex - ${selectedIndex}`
    },
    select (selectedVal, selectedIndex) {
      this.result = `select: val - ${selectedVal.join('-')}, selectedIndex - ${selectedIndex.join('-')}`
    },
    cancel () {
      this.result = 'canceled'
    },
    valueChange (selectedVal, selectedIndex) {
      this.result = `value-change: val - ${selectedVal.join('-')}, selectedIndex - ${selectedIndex.join('-')}`
    }
  },
  components: {
    Picker
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
