<template>
    <div>
        <!-- 轮播图 -->
        <swiper indicator-dots>
            <block v-for="(item, index) in detail.pics" :key="index">
                <swiper-item>
                <image :src="item.pics_big_url" class="slide-image" mode="aspectFill"/>
                </swiper-item>
            </block>
        </swiper>
        <!-- 商品的基本信息 -->
        <div class="goods-info">
          <div class="goods-price">
            ￥ {{detail.goods_price}}
          </div>
          <div class="goods-title">
            <h4>{{detail.goods_name}}</h4>
            <div class="goods-star">
              <span class="iconfont icon-shoucang"></span>
              <p>分享</p>
              <button open-type='share' class="share-btn">分享</button>
            </div>
          </div>
           <!-- 商品的详细信息 -->
           <div class="goods_detail">
             <div class="goods_detail_title">
               商品详情
             </div>
             <div v-html="detail.goods_introduce"></div>
           </div>
        </div>
    </div>
</template>

<script>
import request from '../../utils/request.js'

export default {
  data () {
    return {
      detail: {},
      goodsId: null
    }
  },
  methods: {
    async loadData () {
      //   根据商品id查询数据
      let res = await request('goods/detail', 'get', {
        goods_id: this.goodsId
      })
      const {message} = res.data
      this.detail = message
    }
  },
  onLoad (params) {
    // 获取路径的参数
    this.goodsId = params.goods_id
    this.loadData()
  }
}
</script>

<style scoped lang='scss'>
  @import 'main.scss'
</style>
