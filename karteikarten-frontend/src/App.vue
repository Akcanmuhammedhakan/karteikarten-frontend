<template>
  <div id="app">
    <h1>Karteikarten Übersicht</h1>
    <div class="cards-grid">
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

const cards = ref([])

onMounted(async () => {
  try {
    const response = await fetch('https://karteikarten-app.onrender.com/cards')
    if (!response.ok) throw new Error('Fehler beim Abrufen der Karten')
    cards.value = await response.json()
  } catch (error) {
    console.error('Fetch-Fehler:', error)
  }
})
</script>

<style scoped>
#app {
  max-width: 1200px;
  margin: 3rem auto;
  padding: 2rem;
  font-family: 'Segoe UI', sans-serif;
  color: #fff;
  text-align: center;
}

.cards-grid {
  margin-top: 2rem;
  display: grid;
  gap: 2rem;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  justify-content: center;
}
</style>