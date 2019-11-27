<template>
  <div id="app" class="container h-100">
    <div id="nav">
        <Header />
    </div>
    <router-view/>
    
    <figure>
      <div class="text-align"  style="margin-top:15px; margin-left:-180px;font-size:0.8rem"><i>sponsored by :</i></div>
      <img class='img-fluid' src="@/assets/sponsor.png" alt="onewoorks and vireserve and iqbal fikri" />
    </figure>
  </div>
</template>

<script>
import Header from '@/components/Header'
export default {
  components: {
    Header
  }
}
</script>

<style>
body {
  background-color: #eeeeee;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
  font-size: 0.9rem
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
