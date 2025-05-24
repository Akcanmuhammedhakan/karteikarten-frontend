<template>
  <div id="app">
    <h1>Karteikarten Übersicht</h1>
    <div class="card-grid">
      <FlashCard
          v-for="card in cards"
          :key="card.id"
          :card="card"
      />
    </div>
  </div>
</template>

<style scoped>
#app {
  max-width: 1000px;
  margin: 2rem auto;
  padding: 2rem;
  text-align: center;
  color: #eee;
}

.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1.5rem;
  margin-top: 2rem;
}
</style>