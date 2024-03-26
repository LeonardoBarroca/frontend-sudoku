<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Navbar from '@/components/Navbar.vue';
import Footer from '@/components/Footer.vue';

</script>
<template>
  <header>
    <Navbar />
  </header>
  <RouterView></RouterView>
  <div class="test">
    <Footer></Footer>
  </div>
</template>

<style>
@import "bootstrap/dist/css/bootstrap.min.css";

.test {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
}
</style>
