<script setup lang="ts">
import { ref } from 'vue';
import Home from './components/Home.vue';
import Game from './components/Game.vue';

const currentPage = ref('home');
const selectedCategory = ref('');

const startGame = (category: string) => {
  selectedCategory.value = category;
  currentPage.value = 'game';
};

const goHome = () => {
  currentPage.value = 'home';
};
</script>

<template>
  <div class="container">
    <Home v-if="currentPage === 'home'" @start-game="startGame" />
    <Game v-else :category="selectedCategory" @go-home="goHome" />
  </div>
</template>

<style>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

button {
  background-color: #4CAF50;
  color: white;
  padding: 15px 32px;
  font-size: 16px;
  margin: 10px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
}

h1, h2 {
  color: #333;
}
</style>