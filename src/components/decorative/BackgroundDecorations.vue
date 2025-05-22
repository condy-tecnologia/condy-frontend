<template>
  <div class="fixed inset-0 pointer-events-none overflow-hidden">
    <!-- Círculo azul -->
    <div class="absolute top-[10%] left-[5%] w-4 h-4 rounded-full bg-blue-500 opacity-70"></div>

    <!-- Círculo verde -->
    <div class="absolute top-[15%] right-[8%] w-3 h-3 rounded-full bg-green-500 opacity-60"></div>

    <!-- Estrela amarela -->
    <div class="absolute top-[35%] left-[10%] text-yellow-500 opacity-70">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="currentColor" viewBox="0 0 24 24">
        <path d="M12 0L14.59 8.2H23.41L16.41 13.4L19 21.6L12 16.4L5 21.6L7.59 13.4L0.59 8.2H9.41L12 0Z"/>
      </svg>
    </div>

    <!-- Triângulo roxo -->
    <div class="absolute top-[25%] right-[20%] text-purple-500 opacity-60 transform -rotate-12">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
        <path d="M12 2L22 20H2L12 2Z"/>
      </svg>
    </div>

    <!-- Estrela amarela -->
    <div class="absolute bottom-[25%] right-[8%] text-yellow-500 opacity-70">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="currentColor" viewBox="0 0 24 24">
        <path d="M12 0L14.59 8.2H23.41L16.41 13.4L19 21.6L12 16.4L5 21.6L7.59 13.4L0.59 8.2H9.41L12 0Z"/>
      </svg>
    </div>

    <!-- Cruz rosa -->
    <div class="absolute bottom-[15%] left-[15%] text-pink-500 opacity-60 transform rotate-12">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="currentColor" viewBox="0 0 24 24">
        <path d="M16 8V0H8V8H0V16H8V24H16V16H24V8H16Z"/>
      </svg>
    </div>
  </div>
</template>
