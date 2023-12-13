<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App',
};
</script>
<style >
#app{
width: 100vw; /* 100% of the viewport width */
height: 100vh; /* 100% of the viewport height */
margin: 0; /* Remove default margin */
overflow: hidden;
  background-color: white;

}

</style>

