<template>
  <div class="flashcard">
    <h3>{{ card.topic }}</h3>
    <h4>TEST123</h4>
    <p>{{ card.question }}</p>
    <p>{{ card.answer }}</p>
  </div>
</template>

<script setup>
defineProps({
  card: {
    type: Object,
    required: true,
  },
});
</script>

<style scoped>
.flashcard {
  cursor: pointer;
  border-radius: 16px;
  background-color: #ffefef; /* Rosa Hintergrund */
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
  color: red; /* TEST123 wird rot */
}

.flashcard p {
  font-size: 1rem;
  color: #666;
  margin-top: 0.5rem;
}
</style>