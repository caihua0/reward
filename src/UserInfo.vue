//用户信息

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
  <span>1234565</span>
  </div>
  <div class="body">
    <ul class="ullist">
      <li class="lilist">
        <img class="icon" src="./assets/下载.png" @click="addday">
        <span class="name">积分</span><span class="number">
        <mt-badge size="small">30</mt-badge></span>
      </li>
      <li class="lilist">
        <img class="icon" src="./assets/下载.png" >
        <span class="name">奖金</span>
        <span class="number"><mt-badge size="small"type="error">30</mt-badge></span>
      </li>
      <li class="lilist">
        <img class="icon" src="./assets/下载.png" >
        <span class="name">假期</span>
        <span class="number"><mt-badge size="small" type="warning">30</mt-badge></span>
      </li>
    </ul>
  </div>
  <mt-loadmore :top-method="loadTop" :bottom-method="loadBottom" ref="loadmore">
    <mt-cell  v-for="item in list"  :key="item.id" title="操作类型">{{ item }}</mt-cell>
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
      list:[{id:1,data:'2016-01-01'},{id:1,data:'2016-01-02'},{id:1,data:'2016-01-03'},{id:1,data:'2016-01-04'}]
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
    addday(){
      MessageBox.confirm('确定执行此操作?').then(action => {
        console.log(2);
      },()=>{
        console.log(3);
      }
      );
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