<template>
  <div>
  <div class="PostList">
    <div  class="loading" v-if="isLoading">
  <img src="../assets/loading.gif">
    </div>

    <div class="posts" v-else>
      <ul >
        <li>
          <div class="topBar">
          <span>全部</span>
          <span>精华</span>
          <span>分享</span>
          <span>问答</span>
          <span>招聘</span>
          </div>
        </li>

        <li v-for="post in posts">
          <img :src="post.author.avatar_url" alt="">
            <span >
          {{post.reply_count}}/{{post.visit_count}}</span>
          <span :class="[{put_good:(post.good===true),put_top:(post.top===true),
          'topiclist-tab':(post.good!==true&&post.top!==true)}]">
            <span>
              {{post|tabFormatter}}
            </span>
          </span>
          <router-link :to="{name:'post_content',
                               params:{
                                id:post.id,
                                name:post.author.loginname
                               }}" >
            <span>{{post.title}}111</span>
          </router-link>
          <span class="last_reply">{{post.last_reply_at |formatDate}}</span>
        </li>
        <li>
          <pagination @handleList="renderList"></pagination>
        </li>
      </ul>
      </div>
    </div>
  </div>
</template>
<script>
import Pagination from './Pagination'
export default {
  name: 'PostList',
  components:{
    Pagination
  },
  data() {
    return {
      isLoading: false,
      posts:[],
      postPage:1
    }
  },
  methods:{
    getData(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{
          page:this.postPage,
          limit:20
        },

      })
        .then(res=>{
          this.isLoading =false;
          this.posts=res.data.data
        })
        .catch(function (err) {
          console.log(err)
        })
    },
    renderList(value){
      this.postPage=value
      this.getData()
    }

  },
  beforeMount(){
    this.isLoading=true
    this.getData();
  },

}
</script>
<style scoped>
.PostList{
  background-color: #e1e1e1;
}
.posts {
  margin-top: 10px;
}

.PostList img {
  height: 30px;
  width: 30px;
  vertical-align: middle;
}
ul {
  list-style: none;
  width: 100%;
  max-width: 1344px;
  margin: 0 auto;
}

ul li:not(:first-child) {
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
  font-weight: 400;
  background-color: white;
  color: #333;
  border-top: 1px solid #f0f0f0;
}
li:not(:first-child):hover {
  background: #f5f5f5;;
}

li:last-child:hover {
  background: white;
}

li span {
  line-height: 30px;
}
.put_good, .put_top {
  background: #80bd01;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  color: #fff;
  font-size: 12px;
  margin-right: 10px;
}

.topiclist-tab {
  background-color: #e5e5e5;
  color: #999;
  padding: 2px 4px;
  border-radius: 3px;
  -webkit-border-radius: 3px;
  -moz-border-radius: 3px;
  -o-border-radius: 3px;
  font-size: 12px;
  margin-right: 10px;
}
.last_reply {
  text-align: right;
  min-width: 50px;
  display: inline-block;
  white-space: nowrap;
  float: right;
  color: #778087;
  font-size: 12px;
}
.topBar {
  height: 40px;
  background-color: #f5f5f5;
}

.topBar span {
  font-size: 14px;
  color: #80bd01;
  line-height: 40px;
  margin: 0 10px;
  cursor: pointer;
}

.topBar span:hover {
  color: #9e78c0;
}
a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
}
</style>
