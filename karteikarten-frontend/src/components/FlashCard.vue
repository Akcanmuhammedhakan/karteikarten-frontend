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
  alert(`Thema: ${card.category}\nFrage: ${card.question}`)
}
</script>

<style scoped>
.flashcard {
  cursor: pointer;
  border-radius: 12px;
  background-color: white;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.flashcard:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.15);
}

.flashcard h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.1rem;
  color: #444;
}

.flashcard h4 {
  font-size: 1.25rem;
  margin: 0.3rem 0;
  color: #222;
}

.flashcard p {
  font-size: 1rem;
  color: #666;
  margin-top: 0.5rem;
}
</style>