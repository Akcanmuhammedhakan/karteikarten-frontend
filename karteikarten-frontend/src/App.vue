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
import FlashCard from './components/FlashCard.vue'

const cards = [
  { id: 1, question: 'Was ist JavaScript?', answer: 'JavaScript ist eine Programmiersprache für Webseiten.' },
  { id: 2, question: 'Was ist Vue.js?', answer: 'Vue.js ist ein Framework für die Erstellung von Benutzeroberflächen.' },
  { id: 3, question: 'Was ist HTML?', answer: 'HTML ist die Struktur einer Webseite.' }
]
</script>

<style scoped>
#app {
  max-width: 600px;
  margin: 2rem auto;
  padding: 1rem;
}
</style>