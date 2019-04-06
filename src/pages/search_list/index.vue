<template>
    <div>
      <!-- 搜索条 -->
      <!-- <search-bar></search-bar> -->
      <div class="search">
        <div class="search-input">
          <icon type="search" size="16" color="#999"/>
          {{keyword}}
        </div>
      </div>
      <!-- 选项卡 -->
      <div class="tabs">
        <div @click="tabHandle(index)" :class="{active:currentIndex === index}" :key="index" v-for="(item, index) in tabNames" class="tab-item">
            {{item}}
        </div>
      </div>

    </div> 
</template>
<script>
import SearchBar from '../../components/searchbar'

export default {
  data () {
    return {
      tabNames: ['综合', '销量', '价格'],
      keyword: '',
      currentIndex: 0
    }
  },
  methods: {
    tabHandle (index) {
      // 改变当前Tab的索引
      this.currentIndex = index
    }
  },
  components: {
    'search-bar': SearchBar
  },
  onLoad (params) {
    // 小程序的生命周期
    // 参数params表示路径传递过来的参数    传的是一个对象
    // console.log(params)
    this.keyword = params.query
  }
}
</script>

<style scoped lang='scss'>
    @import 'main.scss'
</style>


