
<template comments>
<!-- 悬停效果  kanyun  -----亮哥  -->
    <div class="fixtop2">

    <header class="header" ref="header"></header>

    <div class="nav" ref="nav" :class="{isFixed:isFixed}">
      <div class="box" v-for="(item,index) in list" :key="index">
        {{item.title}}
      </div>
    </div>

    <ul class="content">
      <li v-for="(item,index) in new Array(50)" :key="index">{{index+1}}</li>
    </ul>

  </div>
</template>

<script>
var throttle = require('lodash/throttle'); //从lodash中引入的throttle节流函数
// (记得安装lodash ：npm install lodash )
export default {
  name: 'navScroll2',
  data() {
    return {
      list: [
        { title: 'AAAA', id: 1 },
        { title: 'BBBB', id: 2 },
        { title: 'CCCC', id: 3 },
        { title: 'DDDD', id: 4 },
      ],
      isFixed: false, //是否固定的
      throttleScroll: null, //定义一个截流函数的变量
    };
  },
  methods: {
    //滚动的函数
    handleScroll() {
      let h = $(this.$refs.header).outerHeight(); //header的高度
      let wh = $(window).scrollTop(); //滚动的距离的，为什么这里使用的jq，因为不用考虑的什么的兼容问题
// （记得引进jq）
      let navH = $(this.$refs.nav).outerHeight(); //nav的高度

      if (wh > h) {
        this.isFixed = true;
      } else {
        this.isFixed = false;
      }
    },
  },

  mounted() {
    //写在掉接口的里面的
    this.$nextTick(() => {
      //这里使用监听的scroll的事件，为什么要使用的节流函数，如果不使用的，页面一直在滚动计算的，这样在
      //使用手机时候，出现非常卡的，隔一段时间计算，大大降低了性能的消耗（具体的好处自己去查资料）
      window.addEventListener('scroll', this.throttleScroll, false);
    });

    this.throttleScroll = throttle(this.handleScroll, 100);
  },
  deactivated() {
    //离开页面需要remove这个监听器，不然还是卡到爆。
    window.removeEventListener('scroll', this.throttleScroll);
  },
};
</script>
<style scoped lang="stylus" rel="stylesheet/stylus">
.fixtop2 {
  min-height: 100vh;
}

.header {
  height: 5rem;
  width: 100%;
  background-color: red;
}

.nav {
  display: flex;
  width: 100%;
  background-color: pink;
  &.isFixed {
    position: fixed;
    left: 0;
    top: 0;
    z-index: 9999;
  }
  .box {
    font-size: 0.3rem;
    height: 0.9rem;
    line-height: 0.9rem;
    color: #333333;
    flex: 1;
  }
}

.content {
  height: 20rem;
  li {
    width: 100%;
    height: 1rem;
    border-bottom: 1px solid #000;
  }
}
</style>





