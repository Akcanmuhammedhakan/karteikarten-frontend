<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-900 via-purple-900 to-black text-white p-6">
    <h1 class="text-5xl font-extrabold text-center mb-10 text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 to-orange-600 animate-pulse">
      Karteikarten Meisterwerk
    </h1>

    <!-- Funktionsleiste -->
    <div class="mb-8 text-center">
      <button @click="addCard" class="btn btn-add px-6 py-3 mr-4">Karte hinzufügen</button>
      <button @click="sortCards" class="btn btn-sort px-6 py-3 mr-4">Nach Thema sortieren</button>
      <button @click="saveCards" class="btn btn-save px-6 py-3 mr-4">Speichern</button>
      <button @click="quizMode" class="btn btn-quiz px-6 py-3">Abfragen</button>
    </div>

    <!-- Debugging -->
    <p class="text-center text-gray-400 mb-4">{{ cards.length }} Karten gefunden</p>

    <!-- Karteikarten-Grid -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 max-w-6xl mx-auto">
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
h1 {
  text-shadow: 0 0 15px rgba(255, 215, 0, 0.7);
}

.btn {
  font-size: 1.2rem;
  padding: 1rem 2rem;
  border-radius: 8px;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  min-width: 220px;
}

.btn:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 15px rgba(0, 0, 0, 0.4);
}

.btn-add { background-color: #28a745; color: white; }
.btn-add:hover { background-color: #218838; }
.btn-sort { background-color: #007bff; color: white; }
.btn-sort:hover { background-color: #0056b3; }
.btn-save { background-color: #6610f2; color: white; }
.btn-save:hover { background-color: #520dc2; }
.btn-quiz { background-color: #e83e8c; color: white; }
.btn-quiz:hover { background-color: #c82373; }

.grid-cols-1.sm\:grid-cols-2.lg\:grid-cols-3 > * {
  transition: transform 0.3s ease, opacity 0.3s ease;
}

.grid-cols-1.sm\:grid-cols-2.lg\:grid-cols-3 > *:hover {
  transform: translateY(-5px);
  opacity: 0.95;
}
</style>