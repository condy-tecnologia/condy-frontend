<script setup lang="ts">
import { ref } from 'vue';
import ChatMessage from './ChatMessage.vue';
import ChatInput from './ChatInput.vue';
import AssistantAvatar from './AssistantAvatar.vue';

interface Message {
  id: number;
  isAssistant: boolean;
  text: string;
  header?: string;
}

const props = defineProps<{
  initialMessages?: Message[];
  placeholder?: string;
  disabled?: boolean;
}>();

const messages = ref<Message[]>(props.initialMessages || []);
const messagesEndRef = ref<HTMLDivElement | null>(null);

function scrollToBottom() {
  if (messagesEndRef.value) {
    setTimeout(() => {
      messagesEndRef.value?.scrollIntoView({ behavior: 'smooth' });
    }, 100);
  }
}

function addMessage(text: string, isAssistant: boolean = false, header?: string) {
  messages.value.push({
    id: Date.now(),
    isAssistant,
    text,
    header,
  });
  scrollToBottom();
}

function handleSendMessage(text: string) {
  addMessage(text, false);
  // Aqui você pode enviar a mensagem para o backend
  // e então adicionar a resposta quando ela chegar
}
</script>

<template>
  <div class="flex flex-col h-full">
    <div class="flex-1 overflow-y-auto p-4 space-y-4">
      <div v-if="messages.length === 0" class="text-center text-gray-400 p-6">
        Sem mensagens ainda.
      </div>

      <template v-else>
        <ChatMessage
          v-for="message in messages"
          :key="message.id"
          :is-assistant="message.isAssistant"
          :message="message.text"
          :header="message.header"
        >
          <template v-if="message.isAssistant" #avatar>
            <AssistantAvatar />
          </template>
        </ChatMessage>
      </template>

      <div ref="messagesEndRef"></div>
    </div>

    <div class="p-4 border-t">
      <ChatInput
        :placeholder="placeholder"
        :disabled="disabled"
        @send="handleSendMessage"
      />
    </div>
  </div>
</template>
