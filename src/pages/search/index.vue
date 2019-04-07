// 搜索页
<template>
    <div>
        <!-- 搜索框 -->
        <div class="search">
            <div class="search-content">
                <div class="search-input">
                    <icon type="search" size="16"/>
                    <input @confirm='confirmHandle' @input="searchHandle" v-model="keyword" placeholder="请输入关键字">
                </div>
                <button @click="cancleHandle" v-if="keyword" class="cancel">取消</button>
            </div>
        </div>

        <!-- 数据列表获取 -->
        <div v-if="keyword" class="search-modal">
            <div :key="item.goods_id" v-for="item in searchResult" class="serach-item">
                {{item.goods_name}}
            </div>
        </div>
        
        <!-- 搜索历史 -->
        <div class="history">
            <h5>历史搜索</h5>
            <icon type='clear' size='16' @click='clearHistory'/>
        </div>
        <div class="history-list">
          <div :key='index' v-for='(item, index) in keywordHistory' class="history-item">
            <navigator :url="'/pages/search_list/main?query=' + item">
              {{item}}
            </navigator>
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
      searchResult: [],
      keywordHistory: mpvue.getStorageSync('keyword') || []
    }
  },
  methods: {
    // 删除历史记录
    clearHistory () {
      // 清空搜索关键字的历史信息
      // 清空的是本地存储的数据（清空本地存储的数据并不会影响data中的数据）
      mpvue.clearStorageSync()
      // 清空的是data中的数据
      this.keywordHistory = []
    },
    // 回车触发的事件
    confirmHandle () {
      // 当回车的时候，记录关键字到本地存储
      this.keywordHistory.unshift(this.keyword)
      // 把最新的数据覆盖到本地存储中
      mpvue.setStorageSync('keyword', this.keywordHistory)
      // 如何进行数组去重
      let kwh = [...new Set(this.keywordHistory)]
      // 把最新的数据覆盖到本地存储中
      mpvue.setStorageSync('keyword', kwh)
      // 重新更新页面数据
      this.keywordHistory = kwh
      // 跳转到商品列表
      mpvue.navigateTo({
        url: '/pages/search_list/main?query=' + this.keyword
      })
      // 清空输入框
      this.keyword = ''
    },
    // 取消,清空输入框
    cancleHandle () {
      this.keyword = ''
    },
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
