<template>
  <div >
    <div class="userInfomation">
      <div class="header">
        <ul class="breadcrumb">
          <li >
            <a href="/">主页</a>
            <span class="divider">/</span>
          </li>
        </ul>
      </div>
      <section>
        <img :src="userinfo.avatar_url" alt="">
        <span>{{userinfo.loginname}}</span>
        <p>{{userinfo.score}}积分</p>
        <span>注册时间：{{userinfo.create_at |formatDate}}</span>
      </section>
    </div>
      <div class="userInfomation">
      <div class="header">
        <span>最近创建的话题</span>
      </div>
        <ul class="replies">
          <li v-for="item in userinfo.recent_topics" class="cell">
            <img :src="item.author.avatar_url" alt="">
            <router-link :to="{
          name:'post_content',
          params:{
            id:item.id
          }
          }">
              {{item.title}}
            </router-link>

          </li>
        </ul>
      </div>

    <div class="userInfomation">
      <div class="header">
        <span>最近参与的话题</span>
      </div>
      <ul class="topics">
        <li v-for="item in userinfo.recent_reply">
          <img :src="item.author.avatar_url" alt="">
          <router-link :to="{
          name:'post_content',
          params:{
            id:item.id
          }
          }">
            {{item.title}}
          </router-link>

        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name:'UserInfo',
  data(){
    return{
      userinfo:{}
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
  beforeMount() {
    this. getUserInfoData();
  }

}
</script>
<style>
.userInfomation{
  margin-left: 5px;
  margin-right:5px;
  margin-bottom:13px;
}
.userInfomation .header{
  background-color: #f6f6f6;
  padding: 10px;
  border-radius: 3px 3px 0 0;
}
.breadcrumb{
  padding: 0;
  margin: 0;
  border:none;
}
.userInfomation .breadcrumb a{
  font-size: 14px;
  color: #80bd01;
}
.userInfomation .breadcrumb>li{
  text-shadow: none;
  display: inline-block;
  line-height: 10px;
  text-align: -webkit-match-parent;
}
.userInfomation .breadcrumb>li>.divider{
  padding:0 5px;
  color:#ccc;
}
.userInfomation section {
  padding: 12px;
  background: white;
}
.userInfomation img {
  width: 30px;
  margin-right: 10px;
}
.userInfomation li {
list-style: none;
}
.userInfomation .replies,.userInfomation .topics{
  padding: 12px;
  margin:0;
  background: white;
}
.userInfomation .replies .cell{
  border-bottom:1px solid #e5e5e5;
}

.userInfomation > div > p {
  padding: 12px 0 12px 12px;
  background-color: rgba(212, 205, 205, 0.17);
  font-size: 0.75rem;
  margin: 0;
}
.userInfomation > div >ul > li {
  padding: 4px 0 4px 12px;
  white-space: nowrap;
  font-size: 0.72rem;
  text-overflow: ellipsis;
  overflow: hidden;
  line-height: 30px;
  vertical-align: middle;
}
.userInfomation >ul > li > a {
  color: #094E99;
  text-decoration: none;
}
</style>
