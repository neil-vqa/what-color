<template>
  <Navbar />
  <h1 class="text-5xl">What Color</h1>
  <router-view/>
</template>

<script>
import Navbar from './Navbar.vue'
export default {
  components: {
    Navbar
  }
}
</script>

<style>


</style>