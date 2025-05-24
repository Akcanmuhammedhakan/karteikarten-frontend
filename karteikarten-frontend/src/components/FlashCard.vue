<template>
  <div class="flashcard" @click="handleClick">
    <h3>{{ card.category }}</h3>
    <h4>{{ card.question }}</h4>
    <p>{{ card.answer }}</p>
  </div>
</template>

<script setup>
defineProps(['card'])

function handleClick() {
  alert('Diese Karte wurde geklickt!')
  // Später: hier kannst du z. B. auf eine Detailseite navigieren
}
</script>

<style scoped>
.flashcard {
  cursor: pointer;
  border-radius: 12px;
  background-color: white;
  padding: 1.5rem;
  margin: 1rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.flashcard:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
}

.flashcard h3 {
  margin: 0;
  font-size: 1.1rem;
  color: #555;
}

.flashcard h4 {
  margin-top: 0.5rem;
  font-size: 1.3rem;
  color: #222;
}

.flashcard p {
  margin-top: 0.5rem;
  font-size: 1rem;
  color: #666;
}
</style>