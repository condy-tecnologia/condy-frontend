<script setup lang="ts">
import { RouterView } from 'vue-router'
</script>

<template>
  <div class="min-h-screen flex flex-col bg-[#f0f1fa]">
    <header class="p-4 flex justify-center border-b">
      <div class="container flex justify-center">
        <img src="/logo-condy.svg" alt="Logo Condy" class="h-10" />
      </div>
    </header>

    <main class="flex-1 p-4 flex flex-col items-center justify-center">
      <div class="container max-w-4xl">
        <RouterView />
      </div>
    </main>
  </div>
</template>
