<template>
  <div id="app"  style="background: #e1e1e1" >
    <Header></Header>
    <div    class="main">
      <router-view name="slidebar"></router-view>
      <router-view name="main"></router-view>

    </div>

  </div>
</template>

<script>
import Header from './components/Header'
import PostList from './components/PostList'
export default {
  name: 'App',
  components:{
   Header,PostList
  }
}
</script>

<style scoped>
.main{
  width:100%;
  margin:20px auto;
}
</style>
