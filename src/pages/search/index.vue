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
      isLoading: '',
      // 定时器
      timer: null,
      searchResult: []
    }
  },
  methods: {
    async searchHandle () {
      // 输入关键字,调用后台数据
      // 控制请求的频率(节流)：输入多个字符，但是只发送一次请求
      // 控制是否发送请求
      if (this.isLoading) {
        // 终止后续代码的执行，终止请求
        return
      }
      this.isLoading = true
      this.timer = setTimeout(async () => {
        // 关闭之前的定时任务
        clearTimeout(this.timer)
        let res = await request('goods/qsearch?query=' + this.keyword)
        const {message} = res.data
        this.searchResult = message
        // 重新打开发送请求的开关
        this.isLoading = false
      }, 1000)
    }
  }
}
</script>

<style scoped lang='scss'>
  @import 'main.scss'
</style>
