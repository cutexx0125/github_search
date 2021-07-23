<template>
  <div>
    <section class="jumbotron">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <input type="text" placeholder="enter the name you search" v-model="keyword"/>&nbsp;
        <button @click="searchUsers">Search</button>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Search',
  data() {
    return {
      keyword: ''
    }
  },
  methods: {
    searchUsers() {
      // 请求前更新List的数据
      this.$bus.$emit('updateListData', {
        isFirst: false,
        isLoading: true,
        errMsg: '',
        users: []
      })
      axios.get(`https://api.github.com/search/users?q=${this.keyword}`).then(
        response => {
          console.log('请求成功')
          // 请求成功后更新List的数据
          this.$bus.$emit('updateListData', {
            isLoading: false,
            errMsg: '',
            users: response.data.items
          })
        },
        error => {
          console.log('请求失败',error.message)
          // 请求失败后更新List的数据
          this.$bus.$emit('updateListData', {
            errMsg: error,
            users: []
          })
        }
      )
    }
  }
}
</script>

<style scoped>
  
</style>