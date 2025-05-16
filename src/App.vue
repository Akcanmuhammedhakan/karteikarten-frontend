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
  { id: 1, topic: 'Datenbanktechnologien', question: 'Was beinhaltet das Fach?', answer: 'SQL-Basics, JavaScript, Vorlesungsstoff' },
  { id: 2, topic: 'WebTech', question: 'Was ist im WebTech-Projekt wichtig?', answer: 'Projektarbeit, Vorlesungsstoff, Übungsaufgaben' },
  { id: 3, topic: 'Controlling', question: 'Was sind die wichtigsten Themen?', answer: 'Controlling-Basics, Kostenstellenrechnung, Test-Inhalte' }
]
</script>

<style scoped>
#app {
  max-width: 800px;
  margin: 2rem auto;
  padding: 2rem;
  text-align: center;
}

h1 {
  font-size: 2rem;
  color: #333;
  margin-bottom: 2rem;
}
</style>