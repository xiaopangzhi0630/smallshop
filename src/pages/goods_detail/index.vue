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
