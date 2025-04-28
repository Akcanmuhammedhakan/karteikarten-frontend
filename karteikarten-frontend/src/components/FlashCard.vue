<template>
  <div class="flashcard">
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
  margin: 0.5rem;
  border-radius: 10px;
  background-color: #f5f5f5;
}
</style>