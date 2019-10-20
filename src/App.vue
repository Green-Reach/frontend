<template>
  <div id="app">
    <Nav />      
    <router-view/>
  </div>
</template>
<script>
import Nav from "@/components/Nav";

export default {
  components: {
    Nav
  }
}

</script>
<style>
@import url('https://fonts.googleapis.com/css?family=Montserrat|Open+Sans:300,400,600&display=swap');

body {
  font-family:"Helvetica", "sans-serif";
}

h1, h2, h3 {
  font-family: "Montserrat", "sans-serif";
}

</style>
