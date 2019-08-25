<template>
  <div id="app">
    <Header></Header>
    <div class="main">
      <router-view name="main"></router-view>
      <router-view name="slidebar"></router-view>
    </div>
  </div>
</template>

<script>
import Header from './components/Header'
import PostList from './components/PostList' 

export default {
  name: 'App',
  components:{
    Header,
  }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
  }
  .main{
    width: 98%;
    height: 100%;
    margin: 0 auto;
  }
  body{
    background-color: #e1e1e1;
  }

  .loading{
    text-align: center;
    margin-top: 25vh;
  }

  .loading img {
      width: 400px !important;
      height: 400px !important;
  }
</style>
