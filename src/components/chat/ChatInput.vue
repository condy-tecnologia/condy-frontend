<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps<{
  placeholder?: string;
  disabled?: boolean;
}>();

const emit = defineEmits<{
  (e: 'send', message: string): void;
}>();

const message = ref('');

function sendMessage() {
  if (message.value.trim() && !props.disabled) {
    emit('send', message.value);
    message.value = '';
  }
}
</script>

<template>
  <div class="bg-white rounded-xl shadow-md p-3 flex items-center gap-2">
    <div class="flex-1">
      <input
        v-model="message"
        :placeholder="placeholder || 'Digite sua resposta para a Condy'"
        class="w-full p-2 focus:outline-none"
        :disabled="disabled"
        @keyup.enter="sendMessage"
      />
    </div>

    <div class="flex gap-2">
      <button
        type="button"
        class="text-gray-400 hover:text-gray-600"
        :disabled="disabled"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13" />
        </svg>
      </button>

      <button
        type="button"
        class="text-gray-400 hover:text-gray-600"
        :disabled="disabled"
      >
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
      </button>

      <button
        @click="sendMessage"
        class="bg-[#304FFE] text-white p-2 rounded-lg flex items-center justify-center disabled:opacity-50"
        :disabled="disabled || !message.trim()"
      >
        <span>Enviar</span>
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
        </svg>
      </button>
    </div>
  </div>
</template>
