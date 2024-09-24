<template>
    <div v-if="show" class="fixed inset-0 bg-gray-600 bg-opacity-50 flex justify-center items-center">
      <div class="bg-white rounded-lg shadow-lg w-1/3 p-6">
        <h2 class="text-xl font-bold mb-4">Dados da Tarefa</h2>
  
        <!-- Formulário de Edição -->
        <form @submit.prevent="submitEditTask">
          <!-- Título -->
          <div class="mb-4">
            <span for="title" class="block text-gray-700 font-bold">Título</span>
            <!-- <input
              v-model="taskData.title"
              type="text"
              id="title"
              class="mt-1 block w-full p-2 border rounded-md shadow-sm"
              required
            /> -->
            <span>
              {{ taskData.description }}
            </span>
          </div>
  
          <!-- Descrição -->
          <div class="mb-4">
            <span for="description" class="block text-gray-700 font-bold">Descrição</span>
            <!-- <textarea
              v-model="taskData.description"
              id="description"
              rows="4"
              class="mt-1 block w-full p-2 border rounded-md shadow-sm"
              required
            ></textarea> -->
            <span>
              {{ taskData.description }}
            </span>
          </div>
  
          <!-- Data de Vencimento -->
          <!-- <div class="mb-4">
            <label for="dueDate" class="block text-gray-700">Data de Vencimento</label>
            <input
              v-model="taskData.dueDate"
              type="date"
              id="dueDate"
              class="mt-1 block w-full p-2 border rounded-md shadow-sm"
              required
            />
          </div> -->
  
          <!-- Status -->
          <div class="mb-4">
            <label for="status" class="block text-gray-700">Status</label>
            <select
              v-model="taskData.status"
              id="status"
              class="mt-1 block w-full p-2 border rounded-md shadow-sm"
              required
            >
              <option value="todo">A Fazer</option>
              <option value="doing">Fazendo</option>
              <option value="done">Feito</option>
            </select>
          </div>
  
          <!-- Botões -->
          <div class="flex justify-end space-x-4">
            <button
              type="button"
              @click="closeModal"
              class="px-4 py-2 bg-gray-300 text-gray-700 rounded hover:bg-gray-400"
            >
              Cancelar
            </button>
            <button
              type="submit"
              class="px-4 py-2 bg-indigo-600 text-white rounded hover:bg-indigo-700"
            >
              Salvar
            </button>
          </div>
        </form>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits, reactive, watch } from 'vue';
  
  const props = defineProps({
    show: Boolean,      // Controla se o modal está visível
    task: Object        // A tarefa que está sendo editada
  });
  
  const emit = defineEmits(['close', 'update-task']);
  
  const taskData = reactive({
    title: '',
    description: '',
    status: 'todo'
  });
  
  // Preenche os dados da tarefa quando ela for aberta
  watch(
    () => props.task,
    (newTask) => {
      if (newTask) {
        taskData.id = newTask.id;
        taskData.title = newTask.title;
        taskData.description = newTask.description;
        taskData.status = newTask.status;
      }
    },
    { immediate: true }
  );
  
  // Função para fechar o modal
  const closeModal = () => {
    emit('close');
  };
  
  // Função para submeter as alterações da tarefa
  const submitEditTask = () => {
    emit('update-task', { ...taskData });
    closeModal();
  };
  </script>
  