<template>
  <nav class="bg-warmGray-900">
    <div class="container flex items-center justify-between mx-auto">
      <div class="flex items-center space-x-4">
        <emojione:admission-tickets class="w-16 h-16" />
        <p
          class="text-4xl tracking-wider text-transparent bg-gradient-to-r from-yellow-300 via-orange-300 to-red-500 font-extralight bg-clip-text"
        >
          Wrestling<span class="font-bold">Revolution</span>
        </p>
      </div>
      <div>
        <div v-if="isAuthenticated" class="flex items-center space-x-4">
          <p class="text-coolGray-200">Welcome {{ user?.displayName }}</p>
          <button
            @click="signOut"
            class="px-8 py-2 font-semibold bg-red-600 rounded-full focus:ring-red-900 focus:ring-4 focus:outline-none text-coolGray-100 hover:bg-red-800"
          >
            Logout
          </button>
        </div>
        <div v-else>
          <button
            @click="signIn"
            class="px-8 py-2 font-semibold bg-green-600 rounded-full focus:ring-green-900 focus:ring-4 focus:outline-none text-coolGray-100 hover:bg-green-800"
          >
            Login
          </button>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { authentication } from '~/helpers/useFirebase';

const { signIn, signOut, isAuthenticated, user } = authentication()

</script>
