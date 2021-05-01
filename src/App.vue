<template>
  <NavBar />
  <Home v-if="isAuthenticated" />
  <EmptyHome v-else />
  </template>

<script setup>
  import { authentication } from '~/helpers/useFirebase'
  const { isAuthenticated } = authentication()
</script>