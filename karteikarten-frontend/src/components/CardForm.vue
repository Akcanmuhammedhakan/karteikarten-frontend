<template>
  <form @submit.prevent="submitCard" class="card-form">
    <h2>Neue Karte erstellen</h2>

    <label>
      Frage:
      <input v-model="card.question" required />
    </label>

    <label>
      Antwort:
      <input v-model="card.answer" required />
    </label>

    <label>
      Kategorie:
      <input v-model="card.category" required />
    </label>

    <button type="submit">Speichern</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['card-saved'])

const card = ref({
  question: '',
  answer: '',
  category: ''
})

const submitCard = async () => {
  try {
    const response = await fetch('http://localhost:8080/cards', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ ...card.value, correctCount: 0 })
    })
    if (!response.ok) throw new Error('Fehler beim Speichern')
    card.value = { question: '', answer: '', category: '' }
    emit('card-saved') // 💡 wichtig fürs Nachladen in App.vue
  } catch (err) {
    console.error(err)
    alert('Fehler beim Speichern der Karte')
  }
}
</script>

<style scoped>
.card-form {
  max-width: 400px;
  margin: 2rem auto;
  background: #fff;
  padding: 1.5rem;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  color: #222;
}

label {
  display: block;
  margin-bottom: 1rem;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.3rem;
  border-radius: 6px;
  border: 1px solid #ccc;
}

button {
  padding: 0.5rem 1rem;
  background: #333;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background: #555;
}
</style>