<template>
  <div class="bg-gray-100 p-4 rounded-lg shadow">
    <h3 class="text-lg font-bold text-gray-800">{{ task.title }}</h3>
    <p class="text-gray-600">{{ task.description }}</p>
    <!-- <small class="text-gray-500 block mb-2">Entrega: {{ task.dueDate }}</small> -->

    <!-- Ações -->
    <div class="flex justify-between mt-4">
      <button @click="editTask" class="text-indigo-600 hover:underline">Ver Tarefa</button>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  task: Object, // Objeto da tarefa individual
});

const emit = defineEmits(['edit', 'delete']);

const editTask = () => {
  emit('edit', props.task);
};

</script>
