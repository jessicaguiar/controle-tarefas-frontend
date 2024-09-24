<script setup>
import { ref, onMounted } from 'vue';
import Navbar from './components/Navbar.vue';
import Raia from "./components/Raia.vue";
import CriaCard from "./components/CriaCard.vue";
import EditaCard from './components/EditaCard.vue';

const isModalOpen = ref(false);
const openModal = () => {
  isModalOpen.value = true;
};

const addTask = (task) => {
  tasks.value.push(task);
};

const tasks = ref([]);

// Função para buscar as tarefas da API
const fetchTasks = async () => {
  try {
    const response = await fetch('http://localhost:8002/tickets');
    if (!response.ok) {
      throw new Error('Erro ao buscar tarefas');
    }
    const data = await response.json();
    console.log(data)
    tasks.value = data.tickets; // Atualiza o array tasks com os dados da API
  } catch (error) {
    console.error('Erro:', error);
  }
};

const fetchTaskStatus = async (id, novoStatus) => {
  try {
    const response = await fetch(`http://localhost:8002/tickets/${id}/status`, {
      method: 'PUT', // Método PUT para atualização
      headers: {
        'Content-Type': 'application/json', // Especifica o tipo de conteúdo
      },
      body: JSON.stringify({ status: novoStatus }), // Dados a serem enviados
    });

    if (!response.ok) {
      throw new Error(`Erro ao buscar status da tarefa ${id}: ${response.status} ${response.statusText}`);
    }
    const status = await response.json();
    console.log(`Status da tarefa ${id}:`, status);
    return status; // Retorna o status da tarefa
  } catch (error) {
    console.error('Erro durante a requisição do status:', error.message);
  }
};

// Chama a função fetchTasks quando o componente for montado
onMounted(async () => {
  await fetchTasks();
});

const isEditModalOpen = ref(false);
const selectedTask = ref(null);

// Função para abrir o modal de edição com uma tarefa selecionada
const editTask = (task) => {
  selectedTask.value = task;
  isEditModalOpen.value = true;
};

// Função para atualizar uma tarefa após edição
const updateTask = (updatedTask) => {
  fetchTaskStatus(updatedTask.id, updatedTask.status)
  const taskIndex = tasks.value.findIndex(task => task.title === selectedTask.value.title);
  if (taskIndex !== -1) {
    tasks.value[taskIndex] = updatedTask;
  }
  isEditModalOpen.value = false;
};


</script>

<template>
  <Navbar @muda-status-card="val => openModal()"/>

  <div class="grid grid-cols-3 w-screen h-[calc(100vh-60px)]">
    <Raia 
        titulo="A Fazer" 
        status="todo"
        :tasks="tasks"
        @edit-task="editTask"
        @delete-task="deleteTask"
    />
    <Raia 
        titulo="Fazendo" 
        status="doing"
        :tasks="tasks"
        @edit-task="editTask"
        @delete-task="deleteTask"
    />
    <Raia 
        titulo="Feito" 
        status="done"
        :tasks="tasks"
        @edit-task="editTask"
        @delete-task="deleteTask"
      />
  </div>

  <CriaCard v-if="isModalOpen" :show="isModalOpen" @close="isModalOpen = false" @save-task="addTask" />
  <EditaCard v-if="isEditModalOpen" :show="isEditModalOpen" :task="selectedTask" @close="isEditModalOpen = false" @update-task="updateTask"
    />
</template>
