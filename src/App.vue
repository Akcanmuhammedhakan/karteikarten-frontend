<template>
  <div class="app-container">
    <!-- Animated Background -->
    <div class="background-animation">
      <div class="floating-circle circle-1"></div>
      <div class="floating-circle circle-2"></div>
      <div class="floating-circle circle-3"></div>
    </div>

    <!-- Header -->
    <header class="app-header">
      <div class="header-content">
        <h1 class="main-title">
          <span class="title-gradient">Karteikarten</span>
          <br>
          <span class="title-white">Meisterwerk</span>
        </h1>
        <p class="subtitle">Lerne smarter, nicht härter ✨</p>

        <!-- Stats -->
        <div class="stats-container">
          <div class="stat-item">
            <span class="stat-number">{{ cards.length }}</span>
            <span class="stat-label">Karten</span>
          </div>
          <div class="stat-item">
            <span class="stat-number">{{ uniqueTopics }}</span>
            <span class="stat-label">Themen</span>
          </div>
        </div>
      </div>
    </header>

    <!-- Control Panel -->
    <div class="control-panel">
      <div class="control-grid">
        <button @click="addCard" class="control-btn btn-add">
          <span class="btn-icon">➕</span>
          <span class="btn-text">Hinzufügen</span>
        </button>
        <button @click="sortCards" class="control-btn btn-sort">
          <span class="btn-icon">🔄</span>
          <span class="btn-text">Sortieren</span>
        </button>
        <button @click="saveCards" class="control-btn btn-save">
          <span class="btn-icon">💾</span>
          <span class="btn-text">Speichern</span>
        </button>
        <button @click="startQuiz" class="control-btn btn-quiz" :disabled="cards.length === 0">
          <span class="btn-icon">🧠</span>
          <span class="btn-text">Quiz</span>
        </button>
      </div>
    </div>

    <!-- Quiz Modal -->
    <div v-if="quizActive" class="quiz-modal">
      <div class="quiz-content">
        <div class="quiz-header">
          <h3 class="quiz-title">🎯 Quiz Modus</h3>
          <p class="quiz-progress">Frage {{ currentQuizIndex + 1 }} von {{ cards.length }}</p>
        </div>

        <div class="quiz-card">
          <h4 class="quiz-topic">📚 {{ currentQuizCard.topic }}</h4>
          <p class="quiz-text">{{ showQuizAnswer ? currentQuizCard.answer : currentQuizCard.question }}</p>
        </div>

        <div class="quiz-actions">
          <button v-if="!showQuizAnswer" @click="showQuizAnswer = true" class="quiz-btn quiz-primary">
            💡 Antwort zeigen
          </button>
          <template v-else>
            <button @click="nextQuizCard" class="quiz-btn quiz-primary">
              {{ currentQuizIndex < cards.length - 1 ? '➡️ Weiter' : '🎉 Fertig' }}
            </button>
          </template>
          <button @click="endQuiz" class="quiz-btn quiz-secondary">
            ❌ Beenden
          </button>
        </div>
      </div>
    </div>

    <!-- Cards Grid -->
    <div class="cards-section">
      <div v-if="cards.length === 0" class="empty-state">
        <div class="empty-icon">📚</div>
        <h3 class="empty-title">Noch keine Karteikarten</h3>
        <p class="empty-text">Erstelle deine erste Karteikarte, um zu beginnen!</p>
        <button @click="addCard" class="empty-btn">
          ✨ Erste Karte erstellen
        </button>
      </div>

      <div v-else class="cards-grid">
        <FlashCard
            v-for="card in sortedCards"
            :key="card.id"
            :card="card"
            @click="toggleCard(card.id)"
            @edit="editCard"
            @delete="deleteCard"
            :is-flipped="flippedCards[card.id]"
            :is-editing="editingCardId === card.id"
            @save-edit="saveEdit"
            @cancel-edit="cancelEdit"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import FlashCard from './components/FlashCard.vue'

// Daten
const cards = ref([
  { id: 1, topic: 'Datenbanktechnologien', question: 'Was beinhaltet das Fach?', answer: 'SQL-Basics, JavaScript, Vorlesungsstoff' },
  { id: 2, topic: 'WebTech', question: 'Was ist im WebTech-Projekt wichtig?', answer: 'Projektarbeit, Vorlesungsstoff, Übungsaufgaben' },
  { id: 3, topic: 'Controlling', question: 'Was sind die wichtigsten Themen?', answer: 'Controlling-Basics, Kostenstellenrechnung' },
])

// Zustände
const flippedCards = ref({})
const editingCardId = ref(null)
const quizActive = ref(false)
const currentQuizIndex = ref(0)
const showQuizAnswer = ref(false)

// Computed
const sortedCards = computed(() => [...cards.value])
const uniqueTopics = computed(() => new Set(cards.value.map(card => card.topic)).size)
const currentQuizCard = computed(() => cards.value[currentQuizIndex.value])

// Funktionen
const addCard = () => {
  const newId = Math.max(...cards.value.map(c => c.id), 0) + 1
  cards.value.push({
    id: newId,
    topic: 'Neues Thema',
    question: 'Deine Frage hier...',
    answer: 'Deine Antwort hier...'
  })
  editingCardId.value = newId
}

const sortCards = () => {
  cards.value.sort((a, b) => a.topic.localeCompare(b.topic))
}

const saveCards = () => {
  localStorage.setItem('flashcards', JSON.stringify(cards.value))
  showNotification('✅ Karten erfolgreich gespeichert!')
}

const showNotification = (message) => {
  const notification = document.createElement('div')
  notification.style.cssText = `
    position: fixed;
    top: 30px;
    right: 30px;
    background: linear-gradient(135deg, #10b981, #059669);
    color: white;
    padding: 20px 30px;
    border-radius: 15px;
    font-size: 18px;
    font-weight: 600;
    box-shadow: 0 10px 25px rgba(16, 185, 129, 0.3);
    z-index: 1000;
    border: 1px solid rgba(16, 185, 129, 0.5);
  `
  notification.textContent = message
  document.body.appendChild(notification)
  setTimeout(() => notification.remove(), 3000)
}

const startQuiz = () => {
  if (cards.value.length === 0) return
  quizActive.value = true
  currentQuizIndex.value = 0
  showQuizAnswer.value = false
}

const nextQuizCard = () => {
  if (currentQuizIndex.value < cards.value.length - 1) {
    currentQuizIndex.value++
    showQuizAnswer.value = false
  } else {
    endQuiz()
    showNotification('🎉 Quiz abgeschlossen! Gut gemacht!')
  }
}

const endQuiz = () => {
  quizActive.value = false
  currentQuizIndex.value = 0
  showQuizAnswer.value = false
}

const toggleCard = (id) => {
  flippedCards.value[id] = !flippedCards.value[id]
}

const editCard = (id) => {
  editingCardId.value = id
}

const cancelEdit = () => {
  editingCardId.value = null
}

const deleteCard = (id) => {
  if (confirm('🗑️ Möchtest du diese Karte wirklich löschen?')) {
    cards.value = cards.value.filter(card => card.id !== id)
    delete flippedCards.value[id]
    showNotification('🗑️ Karte gelöscht!')
  }
}

const saveEdit = (id, updatedCard) => {
  const index = cards.value.findIndex(card => card.id === id)
  if (index !== -1) cards.value[index] = updatedCard
  editingCardId.value = null
  showNotification('✏️ Karte aktualisiert!')
}

// Beim Laden gespeicherte Karten laden
onMounted(() => {
  const saved = localStorage.getItem('flashcards')
  if (saved) {
    cards.value = JSON.parse(saved)
  }
})
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

* {
  box-sizing: border-box;
}

.app-container {
  min-height: 100vh;
  height: 100vh;
  background: linear-gradient(135deg, #1e1b4b, #7c3aed, #ec4899);
  color: white;
  position: fixed;
  top: 0;
  left: 0;
  overflow-x: hidden;
  overflow-y: auto;
  width: 100%;
  margin: 0;
  padding: 0;
}

.background-animation {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
}

.floating-circle {
  position: absolute;
  border-radius: 50%;
  filter: blur(40px);
  opacity: 0.3;
  animation: float 6s ease-in-out infinite;
}

.circle-1 {
  width: 600px;
  height: 600px;
  background: #8b5cf6;
  top: -300px;
  right: -300px;
  animation-delay: 0s;
}

.circle-2 {
  width: 500px;
  height: 500px;
  background: #06b6d4;
  bottom: -250px;
  left: -250px;
  animation-delay: 2s;
}

.circle-3 {
  width: 550px;
  height: 550px;
  background: #f59e0b;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: 4s;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

.app-header {
  position: relative;
  z-index: 1;
  text-align: center;
  padding: 28px 5vw; /* 30% kleiner */
  width: 100%;
}

.header-content {
  width: 100%;
  margin: 0;
}

.main-title {
  font-size: clamp(1.4rem, 2.5vw, 2.5rem); /* 30% kleiner */
  font-weight: 900;
  line-height: 1.1;
  margin-bottom: 14px; /* 30% kleiner */
}

.title-gradient {
  background: linear-gradient(135deg, #06b6d4, #8b5cf6, #ec4899);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  display: inline-block;
}

.title-white {
  font-size: clamp(1.26rem, 2.1vw, 2.1rem); /* 30% kleiner */
}

.subtitle {
  font-size: clamp(0.63rem, 0.9vw, 0.84rem); /* 30% kleiner */
  color: rgba(255, 255, 255, 0.8);
  margin-bottom: 21px; /* 30% kleiner */
  font-weight: 300;
}

.stats-container {
  display: flex;
  justify-content: center;
  gap: clamp(14px, 2.1vw, 32px); /* 30% kleiner */
  margin-top: 14px; /* 30% kleiner */
  flex-wrap: wrap;
}

.stat-item {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 11px; /* 30% kleiner */
  padding: clamp(7px, 1.05vw, 11px) clamp(10px, 1.75vw, 14px); /* 30% kleiner */
  text-align: center;
  min-width: clamp(70px, 8.4vw, 112px); /* 30% kleiner */
}

.stat-number {
  display: block;
  font-size: clamp(0.77rem, 1.4vw, 1.26rem); /* 30% kleiner */
  font-weight: 700;
  color: #06b6d4;
}

.stat-label {
  font-size: clamp(0.49rem, 0.7vw, 0.63rem); /* 30% kleiner */
  color: rgba(255, 255, 255, 0.8);
}

.control-panel {
  position: relative;
  z-index: 1;
  width: 100%;
  margin: 0 auto 70px; /* 30% kleiner */
  padding: 0 5vw;
}

.control-grid {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 18px; /* 30% kleiner */
  padding: clamp(14px, 1.75vw, 20px); /* 30% kleiner */
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(clamp(112px, 11.2vw, 154px), 1fr)); /* 30% kleiner */
  gap: clamp(8px, 1.4vw, 18px); /* 30% kleiner */
  width: 100%;
  box-sizing: border-box;
}

.control-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: clamp(11px, 1.4vw, 17px) clamp(7px, 0.84vw, 11px); /* 30% kleiner */
  border: none;
  border-radius: 11px; /* 30% kleiner */
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  min-height: clamp(56px, 7vh, 77px); /* 30% kleiner */
  position: relative;
  overflow: hidden;
}

.control-btn:hover {
  transform: translateY(-10px) scale(1.08);
}

.control-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.control-btn:disabled:hover {
  transform: none;
}

.btn-add {
  background: linear-gradient(135deg, #06b6d4, #0891b2);
  color: white;
  box-shadow: 0 20px 50px rgba(6, 182, 212, 0.4);
}

.btn-sort {
  background: linear-gradient(135deg, #8b5cf6, #7c3aed);
  color: white;
  box-shadow: 0 20px 50px rgba(139, 92, 246, 0.4);
}

.btn-save {
  background: linear-gradient(135deg, #ec4899, #db2777);
  color: white;
  box-shadow: 0 20px 50px rgba(236, 72, 153, 0.4);
}

.btn-quiz {
  background: linear-gradient(135deg, #f59e0b, #d97706);
  color: white;
  box-shadow: 0 20px 50px rgba(245, 158, 11, 0.4);
}

.btn-icon {
  font-size: clamp(1.05rem, 1.4vw, 1.54rem); /* 30% kleiner */
  margin-bottom: 7px; /* 30% kleiner */
}

.btn-text {
  font-size: clamp(0.56rem, 0.84vw, 0.77rem); /* 30% kleiner */
}

.quiz-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  backdrop-filter: blur(10px);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 40px;
}

.quiz-content {
  background: linear-gradient(135deg, #1f2937, #111827);
  border-radius: 40px;
  padding: clamp(30px, 4vw, 60px);
  max-width: 90vw;
  width: 100%;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.5);
}

.quiz-header {
  text-align: center;
  margin-bottom: 60px;
}

.quiz-title {
  font-size: clamp(1.8rem, 3vw, 3rem);
  font-weight: 700;
  margin-bottom: 25px;
}

.quiz-progress {
  font-size: clamp(1rem, 1.5vw, 1.5rem);
  color: rgba(255, 255, 255, 0.7);
}

.quiz-card {
  background: linear-gradient(135deg, rgba(139, 92, 246, 0.2), rgba(236, 72, 153, 0.2));
  border-radius: 30px;
  padding: clamp(25px, 3vw, 40px);
  margin-bottom: 60px;
  border: 1px solid rgba(139, 92, 246, 0.3);
}

.quiz-topic {
  font-size: clamp(1.3rem, 2vw, 2.2rem);
  font-weight: 700;
  color: #06b6d4;
  margin-bottom: 30px;
}

.quiz-text {
  font-size: clamp(1.1rem, 1.8vw, 1.8rem);
  line-height: 1.6;
}

.quiz-actions {
  display: flex;
  gap: 40px;
  justify-content: center;
  flex-wrap: wrap;
}

.quiz-btn {
  padding: clamp(12px, 2vw, 20px) clamp(25px, 3vw, 40px);
  border: none;
  border-radius: 25px;
  font-size: clamp(0.9rem, 1.5vw, 1.4rem);
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
}

.quiz-btn:hover {
  transform: scale(1.08);
}

.quiz-primary {
  background: linear-gradient(135deg, #10b981, #059669);
  color: white;
  box-shadow: 0 20px 40px rgba(16, 185, 129, 0.4);
}

.quiz-secondary {
  background: linear-gradient(135deg, #6b7280, #4b5563);
  color: white;
  box-shadow: 0 20px 40px rgba(107, 114, 128, 0.4);
}

.cards-section {
  position: relative;
  z-index: 1;
  width: 100%;
  margin: 0;
  padding: 0 5vw 42px 5vw; /* 30% kleiner */
}

.empty-state {
  text-align: center;
  padding: clamp(42px, 7vh, 84px) 20px; /* 30% kleiner */
}

.empty-icon {
  font-size: clamp(2.8rem, 5.6vw, 5.6rem); /* 30% kleiner */
  margin-bottom: 35px; /* 30% kleiner */
}

.empty-title {
  font-size: clamp(1.26rem, 2.1vw, 2.1rem); /* 30% kleiner */
  font-weight: 700;
  margin-bottom: 21px; /* 30% kleiner */
  color: rgba(255, 255, 255, 0.9);
}

.empty-text {
  font-size: clamp(0.77rem, 1.26vw, 1.12rem); /* 30% kleiner */
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 42px; /* 30% kleiner */
}

.empty-btn {
  background: linear-gradient(135deg, #06b6d4, #0891b2);
  color: white;
  border: none;
  padding: clamp(11px, 1.4vw, 18px) clamp(21px, 2.8vw, 35px); /* 30% kleiner */
  border-radius: 21px; /* 30% kleiner */
  font-size: clamp(0.77rem, 1.26vw, 1.12rem); /* 30% kleiner */
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 20px 50px rgba(6, 182, 212, 0.4);
}

.empty-btn:hover {
  transform: translateY(-8px) scale(1.08);
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(clamp(168px, 14vw, 238px), 1fr)); /* 30% kleiner */
  gap: clamp(14px, 1.75vw, 25px); /* 30% kleiner */
  padding: 21px 0; /* 30% kleiner */
  width: 100%;
}

/* Spezielle Desktop-Anpassungen */
@media (min-width: 1200px) {
  .control-grid {
    grid-template-columns: repeat(4, 1fr);
  }

  .cards-grid {
    grid-template-columns: repeat(auto-fit, minmax(315px, 1fr)); /* 30% kleiner */
  }
}

@media (min-width: 1600px) {
  .cards-grid {
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr)); /* 30% kleiner */
    gap: 70px; /* 30% kleiner */
  }

  .control-panel {
    padding: 0 3vw;
  }

  .cards-section {
    padding: 0 3vw 70px 3vw; /* 30% kleiner */
  }
}

@media (min-width: 2000px) {
  .cards-grid {
    grid-template-columns: repeat(auto-fit, minmax(420px, 1fr)); /* 30% kleiner */
    gap: 84px; /* 30% kleiner */
  }

  .control-panel {
    padding: 0 2vw;
  }

  .cards-section {
    padding: 0 2vw 70px 2vw; /* 30% kleiner */
  }
}

/* Mobile Anpassungen */
@media (max-width: 768px) {
  .app-container {
    padding: 0;
  }

  .control-grid {
    grid-template-columns: 1fr;
    padding: 21px; /* 30% kleiner */
  }

  .stats-container {
    gap: 21px; /* 30% kleiner */
  }

  .cards-grid {
    grid-template-columns: 1fr;
    gap: 28px; /* 30% kleiner */
  }
}
</style>
