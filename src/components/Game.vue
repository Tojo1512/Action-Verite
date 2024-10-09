<script setup lang="ts">
import { ref, onMounted, defineProps, defineEmits, watch } from 'vue';
import { gsap } from 'gsap';

const props = defineProps<{
  category: string
}>();

const emit = defineEmits(['go-home']);

interface Question {
  type: 'action' | 'vérité';
  category: string;
  content: string;
  timer?: number;
}

const questions = ref<Question[]>([]);
const currentQuestion = ref<Question | null>(null);
const remainingTime = ref<number | null>(null);
let timerInterval: number | null = null;

const loadQuestions = async () => {
  const response = await fetch('/data.csv');
  const csvText = await response.text();
  const lines = csvText.split('\n').slice(1); // Skip header
  questions.value = lines.map(line => {
    const [type, category, content, timer] = line.split(',');
    return { 
      type: type as 'action' | 'vérité', 
      category, 
      content,
      timer: timer ? parseInt(timer) : undefined
    };
  }).filter(q => q.category === props.category);
};

const getRandomQuestion = (type: 'action' | 'vérité') => {
  const filteredQuestions = questions.value.filter(q => q.type === type);
  if (filteredQuestions.length > 0) {
    const randomIndex = Math.floor(Math.random() * filteredQuestions.length);
    currentQuestion.value = filteredQuestions[randomIndex];
    startTimer();
  }
};

const startTimer = () => {
  if (timerInterval) {
    clearInterval(timerInterval);
  }
  if (currentQuestion.value?.timer) {
    remainingTime.value = currentQuestion.value.timer;
    timerInterval = setInterval(() => {
      if (remainingTime.value !== null && remainingTime.value > 0) {
        remainingTime.value--;
      } else {
        clearInterval(timerInterval!);
      }
    }, 1000);
  } else {
    remainingTime.value = null;
  }
};

watch(currentQuestion, () => {
  gsap.from('.question-card', { 
    opacity: 0, 
    y: 20, 
    duration: 0.5, 
    ease: 'power2.out' 
  });
});

onMounted(() => {
  loadQuestions();
  gsap.from('.split-screen', { 
    opacity: 0, 
    scale: 0.9, 
    duration: 1, 
    ease: 'elastic.out(1, 0.5)' 
  });
});
</script>

<template>
  <div class="game-container">
    <h1>Action ou Vérité - {{ category }}</h1>
    <div class="split-screen">
      <div class="action-section" @click="getRandomQuestion('action')">
        <h2>ACTION</h2>
      </div>
      <div class="truth-section" @click="getRandomQuestion('vérité')">
        <h2>VÉRITÉ</h2>
      </div>
    </div>
    <div v-if="currentQuestion" class="question-card">
      <h3>{{ currentQuestion.type.toUpperCase() }}</h3>
      <p>{{ currentQuestion.content }}</p>
      <div v-if="remainingTime !== null" class="timer">
        Temps restant: {{ remainingTime }}s
      </div>
    </div>
    <button @click="$emit('go-home')">Retour à l'accueil</button>
  </div>
</template>

<style scoped>
.game-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
  max-width: 800px;
  margin: 0 auto;
}

.split-screen {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.action-section, .truth-section {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.action-section {
  background-color: rgba(231, 76, 60, 0.8);
}

.truth-section {
  background-color: rgba(52, 152, 219, 0.8);
}

.action-section:hover, .truth-section:hover {
  opacity: 0.9;
}

h2 {
  color: var(--color-text-light);
  font-size: 2.5em;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.question-card {
  background-color: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 8px;
  padding: 20px;
  margin: 20px 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h3 {
  color: var(--color-accent);
  font-size: 1.5em;
  margin-bottom: 10px;
}

p {
  font-size: 1.2em;
  color: var(--color-text);
}

.timer {
  font-size: 1.1em;
  font-weight: bold;
  color: var(--color-secondary);
  margin-top: 10px;
}

button {
  margin-top: 20px;
  font-size: 1.1em;
  padding: 12px 24px;
}

@media (max-width: 768px) {
  .split-screen {
    flex-direction: row;
    height: 50vh;
  }

  .action-section, .truth-section {
    width: 50%;
  }

  h2 {
    font-size: 1.8em;
  }

  .question-card {
    margin: 10px 0;
  }

  h3 {
    font-size: 1.3em;
  }

  p {
    font-size: 1em;
  }
}
</style>