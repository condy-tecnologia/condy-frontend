<script setup lang="ts">
import { RouterLink } from 'vue-router';
import BackgroundDecorations from '@/components/decorative/BackgroundDecorations.vue';
</script>

<template>
  <div class="relative">
    <BackgroundDecorations />

    <div class="bg-white rounded-3xl p-6 md:p-8 shadow-lg max-w-2xl mx-auto">
      <div class="flex flex-col items-center text-center">
        <div class="mb-6">
          <img src="/assistant-avatar.png" alt="Assistente Condy" class="w-24 h-24 rounded-full border-4 border-white shadow-lg mb-3" />
        </div>

        <h1 class="text-2xl md:text-3xl font-bold text-gray-800 mb-3">Olá, futuro gestor de condomínio!</h1>

        <p class="text-gray-600 mb-6">
          Eu sou a Condy, sua parceira inteligente para tornar a administração do seu prédio mais simples e eficiente.
        </p>

        <p class="text-gray-600 font-medium mb-8">
          Vamos começar? É rápido, seguro e vai economizar seu tempo!
        </p>

        <div class="flex flex-col w-full gap-3">
          <RouterLink to="/cadastro" class="bg-[#304FFE] text-white py-3 px-6 rounded-xl font-medium hover:bg-blue-700 transition-colors">
            Iniciar meu cadastro
          </RouterLink>

          <RouterLink to="/" class="text-gray-500 py-3 hover:underline">
            Voltar para o início
          </RouterLink>
        </div>

        <div class="absolute -bottom-8 right-12 text-yellow-400 transform rotate-12">
          <span class="text-4xl">👆</span>
        </div>
      </div>
    </div>
  </div>
</template>
