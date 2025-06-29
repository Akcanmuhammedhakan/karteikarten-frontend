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

.flashcard {
  cursor: pointer;
  border-radius: 16px;
  background-color: #ffefef;
  padding: 2rem;
  margin: 1rem;
  box-shadow: 0 6px 24px rgba(0, 0, 0, 0.12);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  text-align: center;
}

.flashcard:hover {
  transform: scale(1.03);
  box-shadow: 0 12px 36px rgba(0, 0, 0, 0.2);
}

.flashcard h3 {
  font-size: 1.2rem;
  color: #007bff;
  margin-bottom: 0.5rem;
}

.flashcard h4 {
  color: red; /* TEST123 sollte rot angezeigt werden */
}

.flashcard p {
  font-size: 1rem;
  color: #666;
  margin-top: 0.5rem;
}
</style>

//aktuell