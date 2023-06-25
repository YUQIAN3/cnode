<template>
  <div class="authorInfo">
    <div class="authorDetail">
      <p class="topbar">作者</p>

      <router-link :to="{
                       name:'user_info',
                        params:{
                         name:userinfo.loginname
                        }
            }">
        <img :src="userinfo.avatar_url" alt="">
        <span>{{userinfo.loginname}}</span>
      </router-link>

      <p>{{userinfo.score}}积分</p>
    </div>
    <div class="recent_replies">
      <p class="topbar">作者的其他话题</p>
     <ul>
       <li v-for="list in replyLimitBy5">
         <router-link :to="{name:'post_content',
                               params:{
                                id:list.id,
                                name:list.author.loginname
                               }}" >
           <span>{{list.title}}</span>
         </router-link>
       </li>
     </ul>


    </div>
    <div class="recent_topics">
      <p class="topbar">无人回复的话题</p>
      <ul>
        <li v-for="list in topicLimitBy5">
          <router-link :to="{name:'post_content',
                               params:{
                                id:list.id,
                                name:list.author.loginname
                               }}" >
            <span>{{list.title}}</span>
          </router-link>
        </li>
      </ul>
    </div>
  </div>

</template>
<script>
export default {
  name: 'SlideBar',
  data() {
    return {
      userinfo: {}
    }
  },
  methods: {
    getUserInfoData() {
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          if (res.data.success == true) {
            this.userinfo = res.data.data;
          }
        })
        .catch(function (err) {
          console.log(err)
        })
    }
  },
  computed:{
    replyLimitBy5(){
      if(this.userinfo.recent_replies){
       return this.userinfo.recent_replies.splice(0,5)
      }
    },
    topicLimitBy5(){
      if(this.userinfo.topics_replies){
        return this.userinfo.topics_replies.splice(0,5)
      }
    },
  },
  beforeMount() {
    this. getUserInfoData();
  }

}
</script>
<style scoped>
.authorDetail, .recent_replies, .recent_topics {
  background-color: #fff;
}
.authorInfo {
  width: 328px;
  float: right;
  margin-top: 0;
}
li{
  padding: 3px 0 ;
}
.recent_replies ul, .recent_topics ul {
  margin-top: 0px;
  margin-bottom: 0px;
  list-style: none;
  padding-left: 14px;
}

ul a {
  font-size: 12px;
  text-decoration: none;
  color: #778087;
}

.topbar {
  padding: 10px;
  background-color: #f6f6f6;
  height: 20px;
  font-size: 12px;
  margin-top: 10px;
}

img {
  height: 48px;
  width: 48px;
  border-radius: 3px;
  margin: 10px;
}
img,span{
  vertical-align: middle;
}

.loginname {
  width: 100px;
  float: right;
  margin-top: 22px;
  margin-right: 159px;
  font-size: 14px;
}

.authorDetail  a {
  text-decoration: none;
  color: #778087;
}

.authorDetail .topbar {
  margin-top: 0px;
}
</style>
