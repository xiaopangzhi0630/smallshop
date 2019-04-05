<template>
    <div>
        <!-- 搜索 -->
        <search-bar></search-bar>
        <!-- 菜单和内容 -->
        <div class="content">
            <!-- 左侧 -->
            <div class="left">
                <div @click="changeBrand(index)" :class="{active:currentIndex === index}" :key="item.cat_id" v-for="(item, index) in cate" class="menu-item">
                    {{item.cat_name}}
                </div>
            </div>
            <!-- 右侧 -->
            <div class="right">
                <div :key='item1.cat_id' v-for='item1 in rightData' class="brand-item">
                    <div class="brand-title">
                       {{item1.cat_name}}
                    </div>
                    <div class="brand-list">
                        <div :key='i' v-for='(img, i) in item1.children' class="brand">
                            <img :src="img.cat_icon" mode="aspectFill">
                            <p>{{img.cat_name}}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import SearchBar from '../../components/searchbar'
import request from '../../utils/request'

export default {
  data () {
    return {
      cate: [],
      currentIndex: 0,
      rightData: []
    }
  },
  components: {
    'search-bar': SearchBar
  },
  methods: {
    async cateData () {
      // 调用接口获取数据
      let ret = await request('categories')
      this.cate = ret.data.message
      // console.log(ret)
    },
    changeBrand (index) {
      //   点击切换索引
      this.currentIndex = index
      //   同时刷新右侧数据
      this.rightData = this.cate[this.currentIndex].children
    }
  },
  async mounted () {
    await this.cateData()
    // 从当前分类数据中跟新出当前索引对应的右侧数据
    this.rightData = this.cate[this.currentIndex].children
    // console.log(this.rightData)
  }
}
</script>

<style scoped lang='scss'>
  @import 'main.scss'
</style>
