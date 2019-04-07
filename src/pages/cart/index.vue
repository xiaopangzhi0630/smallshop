<template>
    <div class="container">

        <!-- 新增收货人信息 -->
        <div class="add_addresss">
            <text>新增收货人</text>
            <span></span>
        </div>
        <!-- 商品列表展示信息 -->
        <div class="list-title">朵朵生活馆</div>
        <div class="ware-list">
            <div :key="item.goods_id" v-for="item in products" class="ware-item">
              <div>{{item.goods_name}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    getCartData () {
      // 获取购物车的数据
      let cdata = mpvue.getStorageSync('mycart') || {}
      //  把对象转换成为数组
      let product = []
      for (let key in cdata) {
        product.push(cdata[key])
      }
      //   let products = Object.values(cdata)
      //   console.log(product)
      this.products = product
    }
  },
  onLoad () {
    // 从本地存储在中获取购物车信息
    this.getCartData()
  }
}
</script>

<style scoped lang='scss'>
  @import "main.scss"
</style>
