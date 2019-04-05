<template>
    <div>
        <!-- 搜索 -->
        <search-bar></search-bar>
    </div>
</template>

<script>
import SearchBar from '../../components/searchbar'
import request from '../../utils/request'
export default {
  data () {
    return {
      cate: []
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
      console.log(ret)
    }
  },
  mounted () {
    this.cateData()
  }
}
</script>

<style>

</style>
