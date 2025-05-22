<script setup lang="ts">
defineProps<{
  isAssistant: boolean;
  message: string;
  header?: string;
}>();
</script>

<template>
  <div class="flex mb-4" :class="{ 'justify-end': !isAssistant }">
    <div class="flex" :class="{ 'flex-row-reverse': !isAssistant }">
      <div v-if="isAssistant" class="mr-3">
        <slot name="avatar">
          <!-- Importar o componente AssistantAvatar para usar aqui -->
        </slot>
      </div>

      <div
        class="max-w-md rounded-xl p-4 shadow-sm"
        :class="{
          'bg-white text-gray-800': isAssistant,
          'bg-[#304FFE] text-white': !isAssistant
        }"
      >
        <div v-if="header" class="font-medium mb-1" :class="{ 'text-gray-400': isAssistant }">
          {{ header }}
        </div>
        <div>
          <p v-if="!$slots.default">{{ message }}</p>
          <slot v-else></slot>
        </div>
      </div>
    </div>
  </div>
</template>
