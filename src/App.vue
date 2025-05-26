<template>
  <div class="min-h-screen bg-gradient-to-br from-teal-900 via-purple-900 to-black text-white p-6 overflow-hidden" id="background">
    <h1 class="text-5xl font-extrabold text-center mb-10 text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 to-orange-600 animate-pulse">
      Karteikarten Meisterwerk
    </h1>

    <!-- Funktionsleiste -->
    <div class="mb-8 flex justify-center space-x-4">
      <button @click="addCard" class="px-6 py-3 bg-green-600 hover:bg-green-700 text-white font-semibold rounded-lg shadow-md transition-transform hover:scale-105">Karte hinzufügen</button>
      <button @click="sortCards" class="px-6 py-3 bg-blue-600 hover:bg-blue-700 text-white font-semibold rounded-lg shadow-md transition-transform hover:scale-105">Nach Thema sortieren</button>
      <button @click="saveCards" class="px-6 py-3 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold rounded-lg shadow-md transition-transform hover:scale-105">Speichern</button>
      <button @click="quizMode" class="px-6 py-3 bg-purple-600 hover:bg-purple-700 text-white font-semibold rounded-lg shadow-md transition-transform hover:scale-105">Abfragen</button>
    </div>

    <!-- Debugging: Zeige die Anzahl der Karten -->
    <p class="text-center text-gray-400 mb-4">{{ cards.length }} Karten gefunden</p>

    <!-- Karteikarten-Grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 max-w-7xl mx-auto">
      <FlashCard
          v-for="card in sortedCards"
          :key="card.id"
          :card="card"
          @click="toggleCard(card.id)"
          @edit="editCard"
          @delete="deleteCard"
          :is-flipped="flippedCards[card.id]"
          :is-editing="editingCardId === card.id"
          @save-edit="saveEdit"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import FlashCard from './components/FlashCard.vue';

// Daten
const cards = ref([
  { id: 1, topic: 'Datenbanktechnologien', question: 'Was beinhaltet das Fach?', answer: 'SQL-Basics, JavaScript, Vorlesungsstoff' },
  { id: 2, topic: 'WebTech', question: 'Was ist im WebTech-Projekt wichtig?', answer: 'Projektarbeit, Vorlesungsstoff, Übungsaufgaben' },
  { id: 3, topic: 'Controlling', question: 'Was sind die wichtigsten Themen?', answer: 'Controlling-Basics, Kostenstellenrechnung' },
]);

// Zustände
const flippedCards = ref({});
const editingCardId = ref(null);

// Funktionen
const addCard = () => {
  const newId = cards.value.length + 1;
  cards.value.push({ id: newId, topic: `Thema ${newId}`, question: `Frage ${newId}?`, answer: `Antwort ${newId}` });
};

const sortCards = () => {
  cards.value.sort((a, b) => a.topic.localeCompare(b.topic));
};

const saveCards = () => {
  localStorage.setItem('flashcards', JSON.stringify(cards.value));
  alert('Karten wurden gespeichert!');
};

const quizMode = () => {
  if (cards.value.length === 0) {
    alert('Keine Karten zum Abfragen!');
    return;
  }
  const card = cards.value[0];
  alert(`Frage: ${card.question}\nAntwort: ${card.answer}`);
};

const toggleCard = (id) => {
  flippedCards.value[id] = !flippedCards.value[id];
};

const editCard = (id) => {
  editingCardId.value = id;
};

const deleteCard = (id) => {
  cards.value = cards.value.filter(card => card.id !== id);
};

const saveEdit = (id, updatedCard) => {
  const index = cards.value.findIndex(card => card.id === id);
  if (index !== -1) cards.value[index] = updatedCard;
  editingCardId.value = null;
};

const sortedCards = computed(() => [...cards.value]);
</script>

<style scoped>
#background {
  background-image: url('data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320"%3E%3Cpath fill="%23009ffd" fill-opacity="0.3" d="M0,96L48,112C96,128,192,160,288,176C384,192,480,192,576,176C672,160,768,128,864,128C960,128,1056,160,1152,176C1248,192,1344,192,1392,192L1440,192L1440,320L1392,320C1344,320,1248,320,1152,320C1056,320,960,320,864,320C768,320,672,320,576,320C480,320,384,320,288,320C192,320,96,320,48,320L0,320Z"%3E%3C/path%3E%3C/svg%3E');
  background-size: 200% 100%;
  animation: wave 15s infinite linear;
}

@keyframes wave {
  0% { background-position: 0 0; }
  100% { background-position: 200% 0; }
}

h1 {
  text-shadow: 0 0 20px rgba(255, 165, 0, 0.8);
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

button {
  transition: all 0.3s ease;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}
</style>