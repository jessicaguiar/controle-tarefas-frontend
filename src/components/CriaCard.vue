<template>
  <div v-if="show" class="fixed inset-0 bg-gray-600 bg-opacity-50 flex justify-center items-center">
    <div class="bg-white p-8 rounded-lg shadow-lg max-w-md w-full">
      <h2 class="text-2xl font-bold mb-4">Cadastrar Tarefa</h2>
      <form @submit.prevent="submitForm">
        <!-- Campo Título -->
        <div class="mb-4">
          <label for="title" class="block text-gray-700">Título</label>
          <input
            v-model="task.title"
            type="text"
            id="title"
            class="w-full mt-2 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
            required
          />
        </div>

        <!-- Campo Descrição -->
        <div class="mb-4">
          <label for="description" class="block text-gray-700">Descrição</label>
          <textarea
            v-model="task.description"
            id="description"
            class="w-full mt-2 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
            required
          ></textarea>
        </div>

        <!-- Campo Data de Entrega
        <div class="mb-4">
          <label for="dueDate" class="block text-gray-700">Data de Entrega</label>
          <input
            v-model="task.dueDate"
            type="date"
            id="dueDate"
            class="w-full mt-2 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
            required
          />
        </div> -->

        <!-- Campo Anexar Arquivo -->
        <!-- <div class="mb-4">
          <label for="file" class="block text-gray-700">Anexar Arquivo</label>
          <input
            type="file"
            id="file"
            class="w-full mt-2 p-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
            @change="handleFileUpload"
          />
          <p v-if="task.file" class="text-sm text-gray-500 mt-2">Arquivo: {{ task.file.name }}</p>
        </div> -->

        <!-- Ações -->
        <div class="flex gap-2 justify-end">
          <button type="button" @click="closeModal" class="bg-gray-500 text-white px-4 py-2 rounded-lg hover:bg-gray-600">
            Cancelar
          </button>
          <button type="submit" class="bg-indigo-500 text-white px-4 py-2 rounded-lg hover:bg-indigo-600">
            Salvar
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue';

const props = defineProps({
  show: Boolean,
});

const emit = defineEmits(['close', 'save-task']);

const initTask = { title: '', description: '', dueDate: '', status: "todo", file: null };

const task = ref(initTask);

const closeModal = () => {
  task.value = initTask;
  emit('close');
};

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  task.value.file = file;
};

const submitForm = async () => {
  const taskData = {
    title: task.value.title,
    description: task.value.description
  };

  try {
    const response = await fetch('http://localhost:8002/tickets', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(taskData), 
    });

    if (!response.ok) {
      const errorData = await response.json(); 
      throw new Error(`Erro ao cadastrar a tarefa: ${errorData.message || response.statusText}`);
    }

    const result = await response.json(); 
    emit('save-task', result.ticket); 
    closeModal(); 
  } catch (error) {
    console.error('Erro:', error.message); 
  }
};

</script>
