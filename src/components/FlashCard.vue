<template>
  <div class="flashcard">
    <h2>{{ card.topic }}</h2>
    <h3>{{ card.question }}</h3>
    <p>{{ card.answer }}</p>
  </div>
</template>

<script setup>
defineProps(['card'])
</script>

<style scoped>
.flashcard {
  border: 1px solid black;
  padding: 1rem;
  margin: 1rem;
  border-radius: 10px;
  background-color: #f5f5f5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  width: 250px;
  height: 250px; /* ⭐ Fixe Höhe für alle Karten */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.flashcard h2 {
  margin: 0;
  font-size: 1.5rem;
  color: #333;
}

.flashcard h3 {
  margin-top: 0.5rem;
  font-size: 1.2rem;
  color: #666;
}

.flashcard p {
  margin-top: 0.5rem;
  font-size: 1rem;
  color: #555;
}
</style>