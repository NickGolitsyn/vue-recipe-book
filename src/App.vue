<template>
  <div id="nav" v-if="$store.state.user">
    <router-link to="/">Главная</router-link> |
    <button class="btn red" @click="$store.dispatch('logout')">Logout</button>
  </div>
  <router-view/>
</template>

<script>
import { onBeforeMount } from 'vue'
import { useStore } from 'vuex'

export default {
  setup() {
    const store = useStore()

    onBeforeMount(() => {
      store.dispatch('fetchUser')
    })
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

a {
  font-weight: bold;
  color: #2c3e50;
}

a:active {
  color: #42b983;
}

.btn {
  padding: 0.5rem;
  color: #42b983;
  font-weight: bold;
  border: 2px solid #42b983;
  background-color: transparent;
  cursor: pointer;
  margin: 0.3rem;
  transition: all 0.5s;
  border-radius: 5px;
}

.btn:hover {
  color: #fff;
  background-color: #42b983;
}

.red {
  color: #b94242;
  border: 2px solid #b94242;
}

.red:hover {
  background-color: #b94242;
}
</style>
