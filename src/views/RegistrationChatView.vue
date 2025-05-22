<script setup lang="ts">
import { ref, onMounted, reactive } from 'vue';
import AssistantAvatar from '@/components/chat/AssistantAvatar.vue';
import BackgroundDecorations from '@/components/decorative/BackgroundDecorations.vue';

// Definindo a interface para as mensagens
interface Message {
  id: number;
  isAssistant: boolean;
  text: string;
  header?: string;
  options?: Option[];
  isError?: boolean;
  isTyping?: boolean;
  isSuccess?: boolean;
}

interface Option {
  id: string;
  text: string;
  action: string;
}

// Estado da conversação
const conversationState = ref('initial'); // initial, verified, blocked, unfinished, invalid, registered, typing
const userDocument = ref('');
const messages = reactive<Message[]>([
  {
    id: 1,
    isAssistant: true,
    text: 'Mensagem da plataforma Condy Simples',
    header: 'Plataforma Condy'
  },
  {
    id: 2,
    isAssistant: true,
    text: 'Antes de continuarmos, preciso confirmar seu CPF ou CNPJ para verificar seu status na nossa plataforma.',
    header: 'Plataforma Condy'
  }
]);

const isResponding = ref(false);
const placeholder = ref('Digite seu CPF ou CNPJ');
const inputDisabled = ref(false);

// Função para simular a verificação do documento
function verifyDocument(document: string) {
  if (document === '000.000.000-00') {
    // Documento inválido
    conversationState.value = 'invalid';
    messages.push({
      id: Date.now(),
      isAssistant: true,
      text: 'Ops, esse número parece inválido. Você pode verificar e tentar novamente?',
      header: 'Plataforma Condy',
      isError: true
    });
  } else if (document === '123.456.789-10') {
    // Simula um documento já cadastrado
    messages.push({
      id: Date.now(),
      isAssistant: true,
      text: userDocument.value,
      header: 'Usuário Condy',
      isSuccess: true
    });
    conversationState.value = 'verified';
  } else {
    // Documento não encontrado
    conversationState.value = 'unregistered';
    messages.push({
      id: Date.now(),
      isAssistant: true,
      text: '👋 Parece que ainda não temos seu cadastro por aqui. Você gostaria de continuar o cadastro diretamente por aqui ou prefere seguir pelo WhatsApp?',
      header: 'Plataforma Condy',
      options: [
        { id: 'continue', text: 'Continuar na plataforma', action: 'continue' },
        { id: 'whatsapp', text: 'Prefiro usar o WhatsApp', action: 'whatsapp' }
      ]
    });
  }
}

function simulateTyping() {
  isResponding.value = true;
  messages.push({
    id: Date.now(),
    isAssistant: true,
    text: '...',
    header: 'Condy está escrevendo...',
    isTyping: true
  });

  setTimeout(() => {
    // Remove a mensagem de typing
    messages.pop();
    isResponding.value = false;
  }, 1500);
}

function handleUserMessage(message: string) {
  // Adiciona a mensagem do usuário no chat
  messages.push({
    id: Date.now(),
    isAssistant: false,
    text: message
  });

  userDocument.value = message;

  // Simula o assistente digitando
  simulateTyping();

  // Após um pequeno delay, verifica o documento
  setTimeout(() => {
    verifyDocument(message);
  }, 2000);
}

function handleOptionSelect(action: string) {
  if (action === 'continue') {
    messages.push({
      id: Date.now(),
      isAssistant: true,
      text: '👀 Detectamos que você começou um cadastro, mas ainda não finalizou. Quer retomar de onde parou agora?',
      header: 'Plataforma Condy',
      options: [
        { id: 'yes', text: 'Sim, continuar com cadastro', action: 'continue_registration' },
        { id: 'no', text: 'Não, prefiro começar outro', action: 'restart_registration' }
      ]
    });
  } else if (action === 'whatsapp') {
    // Implementar redirecionamento para WhatsApp
  }
}

onMounted(() => {
  // Inicialização do componente
});
</script>

<template>
  <div class="relative h-full">
    <BackgroundDecorations />

    <div class="bg-white rounded-3xl shadow-lg w-full max-w-3xl mx-auto overflow-hidden h-[calc(100vh-12rem)]">
      <div class="bg-[#f0f1fa] p-4 flex items-center border-b">
        <div class="mr-3">
          <AssistantAvatar />
        </div>
        <div>
          <h2 class="font-medium text-gray-800">Condy</h2>
          <p class="text-sm text-gray-500">Assistente virtual</p>
        </div>
      </div>

      <div class="h-[calc(100%-4rem)] flex flex-col">
        <div class="flex-1 overflow-y-auto p-4 space-y-4">
          <!-- Mensagens do chat -->
          <template v-for="message in messages" :key="message.id">
            <div class="flex mb-4" :class="{ 'justify-end': !message.isAssistant }">
              <div class="flex" :class="{ 'flex-row-reverse': !message.isAssistant }">
                <div v-if="message.isAssistant" class="mr-3">
                  <AssistantAvatar />
                </div>

                <div
                  class="max-w-md rounded-xl p-4 shadow-sm"
                  :class="{
                    'bg-white text-gray-800': message.isAssistant && !message.isError,
                    'bg-[#304FFE] text-white': !message.isAssistant,
                    'bg-red-50 text-red-800': message.isError,
                    'animate-pulse': message.isTyping
                  }"
                >
                  <div v-if="message.header" class="font-medium mb-1" :class="{ 'text-gray-400': message.isAssistant && !message.isError }">
                    {{ message.header }}
                  </div>
                  <div>
                    <p>{{ message.text }}</p>

                    <!-- Opções de resposta -->
                    <div v-if="message.options" class="mt-3 space-y-2">
                      <button
                        v-for="option in message.options"
                        :key="option.id"
                        class="flex items-center w-full bg-[#f0f1fa] hover:bg-[#e0e2f7] text-gray-800 py-2 px-4 rounded-lg text-left"
                        @click="handleOptionSelect(option.action)"
                      >
                        <svg v-if="option.id === 'continue' || option.id === 'yes'" class="w-5 h-5 mr-2 text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                        <svg v-else-if="option.id === 'whatsapp'" class="w-5 h-5 mr-2 text-green-500" fill="currentColor" viewBox="0 0 24 24">
                          <path d="M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z"></path>
                        </svg>
                        <svg v-else-if="option.id === 'no'" class="w-5 h-5 mr-2 text-red-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                        {{ option.text }} <svg class="ml-auto h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                        </svg>
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </template>
        </div>

        <div class="p-4 border-t">
          <div class="bg-white rounded-xl shadow-md p-3 flex items-center gap-2">
            <div class="flex-1">
              <input
                v-model="userDocument"
                :placeholder="placeholder"
                class="w-full p-2 focus:outline-none"
                :disabled="inputDisabled || isResponding"
                @keyup.enter="handleUserMessage(userDocument)"
              />
            </div>

            <div class="flex gap-2">
              <button
                type="button"
                class="text-gray-400 hover:text-gray-600"
                :disabled="inputDisabled || isResponding"
              >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13" />
                </svg>
              </button>

              <button
                type="button"
                class="text-gray-400 hover:text-gray-600"
                :disabled="inputDisabled || isResponding"
              >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
              </button>

              <button
                @click="handleUserMessage(userDocument)"
                class="bg-[#304FFE] text-white p-2 rounded-lg flex items-center justify-center disabled:opacity-50"
                :disabled="inputDisabled || isResponding || !userDocument.trim()"
              >
                <span>Enviar</span>
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                </svg>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="mt-3 flex justify-center">
      <div class="flex items-center gap-3">
        <span class="h-2 w-2 rounded-full bg-[#304FFE]"></span>
        <span class="h-2 w-2 rounded-full bg-gray-300"></span>
        <span class="h-2 w-2 rounded-full bg-gray-300"></span>
        <span class="h-2 w-2 rounded-full bg-gray-300"></span>
      </div>
    </div>
  </div>
</template>
