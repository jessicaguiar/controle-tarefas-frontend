<template>
    <div class="px-2 py-2">
      <div class="border-solid border-2 rounded-lg h-full w-full border-gray-200 bg-white">
        <h3 class="py-2 px-5 text-lg font-semibold">{{ tituloTratado }}</h3>
        <div class="p-2 flex flex-col gap-2">
          <Card
            v-for="(task, index) in filteredTasks"
            :key="index"
            :task="task"
            @edit="editTask"
          />
        </div>
      </div>
    </div>
</template>

<script setup>
    import Card from './Card.vue';
    import { ref, defineProps, defineEmits, computed } from 'vue';

    const props = defineProps({
      titulo: String,
      tasks: Array,
      status: String
    });

    const emit = defineEmits(['edit-task', 'delete-task']);

    const tituloTratado = ref(props.titulo.toUpperCase())

    // Filtra as tarefas com base no status da raia
    const filteredTasks = computed(() => {
      return props.tasks.filter(task => task.status === props.status);
    });

    const editTask = (task) => {
      emit('edit-task', task);
    };
    
</script>

<style lang="scss" scoped>

</style>

