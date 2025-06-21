<template>
  <div class="flashcard-wrapper">
    <div class="flashcard" @click="flipCard" :class="{ flipped: isFlipped }">
      <div class="card-inner">
        <!-- Vorderseite -->
        <div v-if="!isEditing" class="card-front">
          <div class="card-content">
            <div class="card-decoration"></div>
            <h3 class="card-topic">{{ card.topic }}</h3>
            <p class="card-question">{{ card.question }}</p>
          </div>
          <div class="card-actions">
            <button @click.stop="editCard" class="action-btn edit-btn" title="Bearbeiten">
              ✏️
            </button>
            <button @click.stop="deleteCard" class="action-btn delete-btn" title="Löschen">
              🗑️
            </button>
          </div>
        </div>

        <!-- Rückseite -->
        <div v-if="!isEditing" class="card-back">
          <div class="card-content">
            <div class="card-decoration"></div>
            <h3 class="card-topic">{{ card.topic }}</h3>
            <p class="card-answer">{{ card.answer }}</p>
          </div>
        </div>

        <!-- Bearbeitungsmodus -->
        <div v-if="isEditing" class="card-edit">
          <h3 class="edit-title">✏️ Karte bearbeiten</h3>

          <div class="edit-form">
            <div class="form-group">
              <label class="form-label">📚 Thema</label>
              <input
                  v-model="editedCard.topic"
                  class="form-input"
                  placeholder="Thema eingeben..."
                  @keydown.enter="saveEdit"
              />
            </div>

            <div class="form-group">
              <label class="form-label">❓ Frage</label>
              <textarea
                  v-model="editedCard.question"
                  class="form-textarea"
                  rows="3"
                  placeholder="Frage eingeben..."
              ></textarea>
            </div>

            <div class="form-group">
              <label class="form-label">💡 Antwort</label>
              <textarea
                  v-model="editedCard.answer"
                  class="form-textarea"
                  rows="3"
                  placeholder="Antwort eingeben..."
              ></textarea>
            </div>
          </div>

          <div class="edit-actions">
            <button @click="saveEdit" class="edit-btn save-btn">
              ✅ Speichern
            </button>
            <button @click="cancelEdit" class="edit-btn cancel-btn">
              ❌ Abbrechen
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref, watch } from 'vue'

const props = defineProps({
  card: { type: Object, required: true },
  isFlipped: { type: Boolean, default: false },
  isEditing: { type: Boolean, default: false },
})

const emit = defineEmits(['click', 'edit', 'delete', 'save-edit', 'cancel-edit'])

const editedCard = ref({ ...props.card })

watch(() => props.card, (newCard) => {
  editedCard.value = { ...newCard }
}, { deep: true })

const flipCard = () => {
  if (!props.isEditing) {
    emit('click', props.card.id)
  }
}

const editCard = () => {
  editedCard.value = { ...props.card }
  emit('edit', props.card.id)
}

const deleteCard = () => {
  emit('delete', props.card.id)
}

const saveEdit = () => {
  if (editedCard.value.topic.trim() && editedCard.value.question.trim() && editedCard.value.answer.trim()) {
    emit('save-edit', props.card.id, { ...editedCard.value })
  }
}

const cancelEdit = () => {
  editedCard.value = { ...props.card }
  emit('cancel-edit')
}
</script>

<style scoped>
.flashcard-wrapper {
  perspective: 1000px;
  height: clamp(175px, 20vh, 245px); /* 30% kleiner */
  width: 100%;
}

.flashcard {
  position: relative;
  width: 100%;
  height: 100%;
  cursor: pointer;
  transition: all 0.6s ease;
  transform-style: preserve-3d;
}

.flashcard:hover {
  transform: translateY(-20px) scale(1.05);
}

.flashcard.flipped {
  transform: rotateY(180deg);
}

.flashcard.flipped:hover {
  transform: rotateY(180deg) translateY(-20px) scale(1.05);
}

.card-inner {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
}

.card-front,
.card-back,
.card-edit {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: clamp(8px, 1.05vw, 14px); /* 30% kleiner */
  padding: clamp(11px, 1.75vw, 21px); /* 30% kleiner */
  display: flex;
  flex-direction: column;
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.4);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.card-front {
  background: linear-gradient(135deg, #1f2937, #111827);
}

.card-back {
  background: linear-gradient(135deg, #7c3aed, #5b21b6);
  transform: rotateY(180deg);
}

.card-edit {
  background: linear-gradient(135deg, #1f2937, #111827);
}

.card-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.card-decoration {
  width: clamp(28px, 3.5vw, 42px); /* 30% kleiner */
  height: clamp(1.4px, 0.21vw, 2.8px); /* 30% kleiner */
  background: linear-gradient(135deg, #06b6d4, #8b5cf6);
  border-radius: 4px;
  margin-bottom: clamp(8px, 1.05vw, 14px); /* 30% kleiner */
}

.card-topic {
  font-size: clamp(0.7rem, 1.12vw, 1.26rem); /* 30% kleiner */
  font-weight: 700;
  margin-bottom: clamp(8px, 1.05vw, 13px); /* 30% kleiner */
  color: #06b6d4;
  line-height: 1.2;
}

.card-question,
.card-answer {
  font-size: clamp(0.63rem, 0.91vw, 0.91rem); /* 30% kleiner */
  line-height: 1.6;
  color: white;
  font-weight: 500;
}

.card-actions {
  display: flex;
  justify-content: flex-end;
  gap: clamp(6px, 0.84vw, 11px); /* 30% kleiner */
  margin-top: clamp(8px, 1.05vw, 13px); /* 30% kleiner */
  opacity: 0;
  transition: opacity 0.3s ease;
}

.flashcard-wrapper:hover .card-actions {
  opacity: 1;
}

.action-btn {
  width: clamp(21px, 1.75vw, 28px); /* 30% kleiner */
  height: clamp(21px, 1.75vw, 28px); /* 30% kleiner */
  border: none;
  border-radius: clamp(4px, 0.7vw, 7px); /* 30% kleiner */
  font-size: clamp(0.56rem, 0.7vw, 0.77rem); /* 30% kleiner */
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-btn:hover {
  transform: scale(1.2);
}

.edit-btn {
  background: rgba(59, 130, 246, 0.2);
  color: #60a5fa;
  border: 1px solid rgba(59, 130, 246, 0.3);
}

.edit-btn:hover {
  background: rgba(59, 130, 246, 0.3);
}

.delete-btn {
  background: rgba(239, 68, 68, 0.2);
  color: #f87171;
  border: 1px solid rgba(239, 68, 68, 0.3);
}

.delete-btn:hover {
  background: rgba(239, 68, 68, 0.3);
}

.edit-title {
  font-size: clamp(0.77rem, 1.12vw, 1.12rem); /* 30% kleiner */
  font-weight: 700;
  text-align: center;
  margin-bottom: clamp(11px, 1.05vw, 15px); /* 30% kleiner */
  color: white;
}

.edit-form {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: clamp(8px, 1.05vw, 13px); /* 30% kleiner */
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-label {
  font-size: clamp(0.56rem, 0.7vw, 0.7rem); /* 30% kleiner */
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: clamp(4px, 0.42vw, 6px); /* 30% kleiner */
}

.form-input,
.form-textarea {
  padding: clamp(6px, 0.84vw, 8px); /* 30% kleiner */
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-radius: clamp(4px, 0.7vw, 7px); /* 30% kleiner */
  background: rgba(255, 255, 255, 0.05);
  color: white;
  font-size: clamp(0.56rem, 0.7vw, 0.7rem); /* 30% kleiner */
  transition: all 0.3s ease;
  resize: none;
}

.form-input:focus,
.form-textarea:focus {
  outline: none;
  border-color: #06b6d4;
  background: rgba(255, 255, 255, 0.1);
  box-shadow: 0 0 30px rgba(6, 182, 212, 0.3);
}

.form-input::placeholder,
.form-textarea::placeholder {
  color: rgba(255, 255, 255, 0.5);
}

.edit-actions {
  display: flex;
  gap: clamp(7px, 0.84vw, 11px); /* 30% kleiner */
  margin-top: clamp(11px, 1.05vw, 15px); /* 30% kleiner */
}

.edit-btn {
  flex: 1;
  padding: clamp(7px, 0.84vw, 10px); /* 30% kleiner */
  border: none;
  border-radius: clamp(4px, 0.7vw, 7px); /* 30% kleiner */
  font-size: clamp(0.56rem, 0.7vw, 0.77rem); /* 30% kleiner */
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
}

.save-btn {
  background: linear-gradient(135deg, #10b981, #059669);
  color: white;
  box-shadow: 0 15px 35px rgba(16, 185, 129, 0.4);
}

.save-btn:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 45px rgba(16, 185, 129, 0.5);
}

.cancel-btn {
  background: linear-gradient(135deg, #6b7280, #4b5563);
  color: white;
  box-shadow: 0 15px 35px rgba(107, 114, 128, 0.4);
}

.cancel-btn:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 45px rgba(107, 114, 128, 0.5);
}
</style>
