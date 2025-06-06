<template>
  <div
      class="flashcard w-full h-96 perspective-1000 cursor-pointer"
      @click="flipCard"
  >
    <div
        class="relative w-full h-full transition-transform duration-700 transform-style-preserve-3d"
        :class="{ 'rotate-y-180': isFlipped }"
    >
      <!-- Vorderseite -->
      <div
          v-if="!isEditing"
          class="absolute w-full h-full backface-hidden bg-gradient-to-br from-teal-600 to-purple-700 rounded-xl shadow-2xl p-6 flex flex-col justify-center items-center text-white"
      >
        <h3 class="text-2xl font-semibold mb-4">{{ card.topic }}</h3>
        <p class="text-lg">{{ card.question }}</p>
        <div class="absolute top-4 right-4 flex space-x-2">
          <button @click.stop="editCard" class="px-3 py-1 bg-blue-600 hover:bg-blue-700 rounded cursor-pointer">Bearbeiten</button>
          <button @click.stop="deleteCard" class="px-3 py-1 bg-red-600 hover:bg-red-700 rounded cursor-pointer">Löschen</button>
        </div>
      </div>

      <!-- Rückseite -->
      <div
          v-if="!isEditing"
          class="absolute w-full h-full backface-hidden bg-gradient-to-br from-purple-700 to-teal-600 rounded-xl shadow-2xl p-6 flex flex-col justify-center items-center text-white rotate-y-180"
      >
        <h3 class="text-2xl font-semibold mb-4">{{ card.topic }}</h3>
        <p class="text-lg">{{ card.answer }}</p>
      </div>

      <!-- Bearbeitungsmodus -->
      <div v-if="isEditing" class="absolute w-full h-full backface-hidden bg-gray-800 rounded-xl shadow-2xl p-6 flex flex-col justify-center items-center text-white">
        <input v-model="editedCard.topic" class="w-full p-2 mb-2 bg-gray-700 rounded text-lg" placeholder="Thema" />
        <input v-model="editedCard.question" class="w-full p-2 mb-2 bg-gray-700 rounded text-lg" placeholder="Frage" />
        <input v-model="editedCard.answer" class="w-full p-2 mb-2 bg-gray-700 rounded text-lg" placeholder="Antwort" />
        <div class="flex space-x-2">
          <button @click="saveEdit" class="px-4 py-2 bg-green-600 hover:bg-green-700 rounded cursor-pointer">Speichern</button>
          <button @click="cancelEdit" class="px-4 py-2 bg-red-600 hover:bg-red-700 rounded cursor-pointer">Abbrechen</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref } from 'vue';

const props = defineProps({
  card: { type: Object, required: true },
  isFlipped: { type: Boolean, default: false },
  isEditing: { type: Boolean, default: false },
});

const emit = defineEmits(['click', 'edit', 'delete', 'save-edit']);

const editedCard = ref({ ...props.card });

const flipCard = () => {
  if (!props.isEditing) emit('click', props.card.id);
};

const editCard = () => emit('edit', props.card.id);
const deleteCard = () => emit('delete', props.card.id);
const saveEdit = () => emit('save-edit', props.card.id, { ...editedCard.value });
const cancelEdit = () => emit('edit', null);
</script>

<style scoped>
.flashcard {
  transform-style: preserve-3d;
}

.backface-hidden {
  backface-visibility: hidden;
}

.rotate-y-180 {
  transform: rotateY(180deg);
}

.flashcard:hover .backface-hidden {
  box-shadow: 0 0 20px rgba(255, 105, 180, 0.5);
  transition: box-shadow 0.3s ease;
}

input {
  transition: all 0.3s ease;
}

input:focus {
  outline: none;
  box-shadow: 0 0 10px rgba(0, 255, 255, 0.5);
}

button {
  transition: all 0.3s ease;
}
</style>