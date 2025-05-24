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
  border-radius: 16px;
  background: linear-gradient(135deg, #ffffff, #f4f4f4);
  padding: 1.5rem;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
  transition: transform 0.2s, box-shadow 0.2s;
  text-align: left;
}

.flashcard:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.25);
}

.flashcard h3 {
  margin: 0;
  font-size: 1rem;
  font-weight: 500;
  color: #0066cc;
}

.flashcard h4 {
  margin-top: 0.75rem;
  font-size: 1.25rem;
  color: #111;
}

.flashcard p {
  margin-top: 0.5rem;
  font-size: 0.95rem;
  color: #333;
}
</style>