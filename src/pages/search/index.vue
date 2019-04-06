// 搜索页
<template>
    <div>
        <!-- 搜索框 -->
        <div class="search">
            <div class="search-content">
                <div class="search-input">
                    <icon type="search" size="16"/>
                    <input @input="searchHandle" v-model="keyword" placeholder="请输入关键字">
                </div>
                <button v-if="keyword" class="cancel">取消</button>
            </div>
        </div>

        <!-- 数据列表获取 -->
        <div class="search-modal">
            <div :key="item.goods_id" v-for="item in searchResult" class="serach-item">
                {{item.goods_name}}
            </div>
        </div>
        
    </div>
</template>

<script>
import request from '../../utils/request.js'

export default {
  data () {
    return {
      keyword: '',
      searchResult: []
    }
  },
  methods: {
    async searchHandle () {
      let res = await request('goods/qsearch?query=' + this.keyword)
      const {message} = res.data
      this.searchResult = message
    }
  }
}
</script>

<style scoped lang='scss'>
  @import 'main.scss'
</style>
