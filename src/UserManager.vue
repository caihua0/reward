//用户管理

<template>
<div class="info">
  <mt-header fixed title="信息" class="tophead">
<!--     <router-link to="/" slot="left">
      <mt-button icon="back">back</mt-button>
    </router-link> -->
    <mt-button slot="right" @click="logout">注销</mt-button>
  </mt-header>
  <div class="head">
  <img class="portrait" :src="$store.state.userInfo.portrait" />
  <span><strong>管理员</strong> {{this.$store.state.userInfo.username}}</span>
  </div>
  <mt-loadmore :top-method="loadTop" :bottom-method="loadBottom" ref="loadmore">
    <mt-cell v-for="item in list"  :key="item.id" :title="item.username">
    <mt-button type="primary" @click="editCount">{{ item.count }} 分</mt-button>
    </mt-cell>
  </mt-loadmore>
</div>

</template>
<script>
import Loading from './Loading.vue'
import { MessageBox } from  'mint-ui'
export default {
  name: 'UserInfo',
  data(){
  	return {
      isLogouting: false,
      list:[]
    }
  },
  components:{
    Loading
  },
  computed: {
  	levelClass(){
  		var level = this.level;
  		if (1 <= level && level <= 7) {
        	return 1;
      	} else if (8 <= level && level <= 16) {
        	return 2;
      	} else if (16 <= level && level <= 31) {
        	return 3;
      	} else if (32 <= level && level <= 63) {
        	return 4;
      	} else if (64 <= level && level <= 127) {
          	return 5;
      	} else if (128 <= level && level <= 254) {
        	return 6;
      	} else {
        	return 6;
      	}
  	}
  },
  created:function(){
    this.getUser();
  },
  methods: {
    //注销
    logout(){
      //删除cookie并跳到登录页
      this.isLogouting = true;
      this.delCookie('session');
      //演示
      setTimeout(()=>{
        this.$router.push('/login/');
        this.isLogouting = false;
      },3000)
    },
    loadTop(){
      this.$refs.loadmore.onTopLoaded();
      
    },
    loadBottom(){
      // this.allLoaded = true;
      this.$refs.loadmore.onBottomLoaded();
    },
    getUser(){
        var that =this;
        this.$ajax({
          url: '/user',
          method: 'post'
        }).then(function(response){
          // console.log(response)
          if(response.data.code == 1){
            that.list=response.data.data;
          }else{
            console.log(response.data.message)
          }
        }).catch(function(err){
          console.log(err)
        });
    },
    editCount(){
        var that =this;        
        MessageBox.prompt('请输入积分',{inputPlaceholder: '请输入数字'}).then(({value,action}) => {
            let loginParam={
              id:that.$store.state.userInfo.id,
              count:value
            };
            this.$ajax({
              url: '/count',
              method: 'post',
              data:loginParam
            }).then(function(response){
              // console.log(response)
              if(response.data.code == 1){
                that.getUser();
              }else{
                console.log(response.data.message)
              }
            }).catch(function(err){
              console.log(err)
            });
        },()=>{console.log("editCount")});
    }
  }
}
</script>

<style scoped>
.tophead{
  background: #31c27c;/*绿色*/
}
.head{
  width: 100%;
  height: 100px;
  margin-top: 40px;
  background: #31c27c;/*绿色*/
  
}

.portrait{
  width: 70px; 
  height: 70px; 
  overflow:hidden; 
  -webkit-border-radius: 100%;
  -moz-border-radius: 100%;
  -ms-border-radius: 100%;
  -o-border-radius: 100%; 
  border-radius: 100%; 
  background-color: #CCCCCC; 
  margin-left: 15px;
  border: 2px solid #2c3e50; 
  /*display: flex;*/
}

.ullist{
  list-style: none;
  display: flex;
  justify-content: space-between;
  margin: 0;
  padding: 20px;
}
.lilist{

  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}
.lilist>.name{
  font-size: 14px;
}
.lilist>.number{
  margin-left: 10px;
}
.lilist>.icon{
    display: inline-block;
    width: 30px;
    height: 30px;
    margin-right: 10px;
}

</style>