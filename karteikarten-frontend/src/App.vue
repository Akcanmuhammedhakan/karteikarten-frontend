<template>
  <div id="app">
    <h1>Karteikarten Meisterwerk</h1>

    <!-- 🔹 Formular-Komponente -->
    <CardForm @card-saved="loadCards" />

    <!-- 🔹 Kartenübersicht -->
    <div class="card-grid">
      <FlashCard
          v-for="card in cards"
          :key="card.id"
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

// ✅ ÄNDERE DIE URL HIER
const loadCards = async () => {
  try {
    const response = await fetch('https://karteikarten-app.onrender.com/cards') // ← Richtig!
    if (!response.ok) throw new Error('Fehler beim Laden der Karten')
    cards.value = await response.json()
  } catch (err) {
    console.error('Fehler beim Laden:', err)
  }
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
</style>