<template>
  <div id="app">
    <h1>Karteikarten Meisterwerk</h1>

    <!-- 🔹 Formular-Komponente -->
    <CardForm @card-saved="loadCards" />

    <!-- 🔹 Sortier-Button -->
    <button @click="sortCards" class="sort-button">Karten zufällig mischen</button>

    <!-- 🔹 Kartenübersicht -->
    <div class="card-grid">
      <FlashCard
          v-for="card in cards"
          :key="card.id + '-' + shuffleKey.value"
          :card="card"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import FlashCard from './components/FlashCard.vue'
import CardForm from './components/CardForm.vue'

const cards = ref([])
const shuffleKey = ref(0) // sorgt für visuelles Neurendern

// Karten vom Backend laden
const loadCards = async () => {
  try {
    const response = await fetch('https://karteikarten-app.onrender.com/cards')
    if (!response.ok) throw new Error('Fehler beim Laden der Karten')
    const fetchedCards = await response.json()
    cards.value = [...fetchedCards]
  } catch (err) {
    console.error('Fehler beim Laden:', err)
    cards.value = []
  }
}

// Shuffle-Funktion
const shuffleArray = (array) => {
  const shuffled = [...array]
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1))
    ;[shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]]
  }
  return shuffled
}

// Karten zufällig mischen
const sortCards = () => {
  cards.value = shuffleArray(cards.value)
  shuffleKey.value++ // zwingt Vue zum Neurendern
}

onMounted(loadCards)
</script>

<style scoped>
#app {
  max-width: 1000px;
  margin: 2rem auto;
  padding: 2rem;
  text-align: center;
  color: #eee;
  font-family: 'Segoe UI', sans-serif;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1.5rem;
  margin-top: 2rem;
}

.sort-button {
  margin-top: 1.5rem;
  padding: 0.6rem 1.2rem;
  font-size: 1rem;
  border-radius: 8px;
  border: none;
  background: #3b82f6;
  color: white;
  cursor: pointer;
}

.sort-button:hover {
  background: #2563eb;
}
</style>