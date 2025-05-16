<template>
  <div id="app">
    <h1>Karteikarten Übersicht</h1>
    <FlashCard
        v-for="card in cards"
        :key="card.id"
        :card="card"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import FlashCard from './components/FlashCard.vue'

// Reaktive Variable für die Karten
const cards = ref([])

// Beim Laden der Komponente: Daten vom Backend holen
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
  max-width: 600px;
  margin: 2rem auto;
  padding: 1rem;
}
</style>