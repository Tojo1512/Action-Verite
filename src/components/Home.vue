<script setup lang="ts">
import { defineEmits, onMounted } from 'vue';
import { gsap } from 'gsap';

const emit = defineEmits(['start-game']);

const startGame = (category: string) => {
  emit('start-game', category);
};

onMounted(() => {
  gsap.from('.home-container', { opacity: 0, y: 50, duration: 1, ease: 'power3.out' });
  gsap.from('.button-container button', { 
    opacity: 0, 
    y: 20, 
    duration: 0.8, 
    stagger: 0.2, 
    ease: 'back.out(1.7)',
    delay: 0.5
  });
});
</script>

<template>
  <div class="home-container">
    <h1>Action ou Vérité</h1>
    <h2>Choisissez une catégorie</h2>
    <div class="button-container">
      <button @click="startGame('standard')">Standard</button>
      <button @click="startGame('drole')">Drôle</button>
      <button @click="startGame('hot')">Hot</button>
    </div>
  </div>
</template>

<style scoped>
.home-container {
  background-color: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.button-container {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-top: 2rem;
}

button {
  font-size: 1.2rem;
  padding: 12px 24px;
}

@media (max-width: 768px) {
  .button-container {
    flex-direction: column;
    align-items: center;
  }

  button {
    width: 80%;
    margin-bottom: 1rem;
  }
}
</style>