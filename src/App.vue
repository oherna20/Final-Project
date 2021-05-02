<template>
  <NavBar />
  <Home v-if="isAuthenticated" />
  <EmptyHome v-else />
  </template>

<script setup>
  import { authentication } from '~/helpers/useFirebase'
  const { isAuthenticated } = authentication()
</script>

<style>
  #app {
  background:url(./assets/images/cinema.jpg) center;
}
</style>