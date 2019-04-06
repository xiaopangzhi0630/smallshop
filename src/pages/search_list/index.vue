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
      <!-- 商品的列表 -->
      <div class="goods_list">
        <navigator :url="'/pages/goods_detail/main?goods_id=' + item.goods_id" class="goods-item" :key="index" v-for="(item, index) in list">
            <img :src="item.goods_small_logo" mode="aspectFill"/>
            <div class="goods-rigth">
              <h4>
                {{item.goods_name}}
              </h4>
              <div class="price">
                <span>￥</span>{{item.goods_price}}
              </div>
            </div>
        </navigator>
      </div>
      <!-- 提示没有更多数据了 -->
      <div class="more" v-if='hasMore'>
        没有更多数据了
      </div>
    </div> 
</template>
<script>
import SearchBar from '../../components/searchbar'
import request from '../../utils/request.js'

export default {
  data () {
    return {
      tabNames: ['综合', '销量', '价格'],
      keyword: '',
      currentIndex: 0,
      list: [],
      pagenum: 1,
      total: 0,
      // 保证接口调用完成之后才可以再次调用接口，
      // 如果接口正在获取数据，那么在这个过程中是不允许再次触发接口调用
      isLoading: false,
      hasMore: false
    }
  },
  methods: {
    tabHandle (index) {
      // 改变当前Tab的索引
      this.currentIndex = index
    },
    // 封装加载功能
    async loadData () {
      // 如果没有更多数据，就应该禁止发送请求调用接口
      // 本次接口调用是否已经加载完成
      if (this.isLoading || this.hasMore) {
        return
      }
      // 禁止再次触发匹配的商品列表
      this.isLoading = true
      // / 根据关键字加载匹配的商品列表数据
      let res = await request('goods/search', 'get', {
        query: this.keyword,
        pagenum: this.pagenum
      })
      // let res = await request('goods/search?query=' + this.keyword)
      // let res = await request(`goods/search?query=${this.keyword}&pagenum=${this.pagenum}`)
      // console.log(res)
      const {message} = res.data
      // 需要把新加载的一页数据添加到list中
      let goods = [...this.list, ...message.goods]
      this.list = goods
      this.pagenum = parseInt(message.pagenum)
      this.total = message.total

      // 判断是否还有更多的数据
      if (this.list.length >= this.total) {
        // 没有更多的数据
        this.hasMore = true
      }

      // 数据加载完之后让页码加1
      this.pagenum = this.pagenum + 1
      // 接口数据返回之后, 才允许再次发送请求
      this.isLoading = false
    }
  },
  components: {
    'search-bar': SearchBar
  },
  async onLoad (params) {
    // 小程序的生命周期
    // 参数params表示路径传递过来的参数    传的是一个对象
    // console.log(params)
    this.keyword = params.query
    //  页面展示调用loadData加载数据
    this.loadData()
  },
  // 页面上拉触底事件的处理函数
  onReachBottom () {
    // console.log(111)
    this.loadData()
  }
}
</script>

<style scoped lang='scss'>
    @import 'main.scss'
</style>


