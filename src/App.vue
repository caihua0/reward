//整个应用的根组件
<template>
<div class="container" id="app">
  <transition name="fade">
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </transition>
</div>
</template>

<script>
// import Login from './Loading.vue'
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
  // ,
  // components: { Loading }
}
</script>

<style>

</style>
