<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">

      <nav>
        <RouterLink to="/">Home Page</RouterLink>
        <RouterLink to="/about">About Page</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView></RouterView>
</template>

<style scoped></style>
