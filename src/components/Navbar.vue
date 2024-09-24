<template>
    <div class="w-screen h-[60px] bg-violet-700 flex flex-row gap-10 p-3">
        <button 
            type="button" 
            data-modal-target="modal-cria-card" 
            data-modal-toggle="modal-cria-card"
            class="text-gray-900 bg-fuchsia-100 border border-fuchsia-300 focus:outline-none hover:bg-fuchsia-100 focus:ring-4 focus:ring-fuchsia-100 font-medium rounded-full text-sm px-4 py-2"
            @click="$emit('mudaStatusCard', true)"    
        > 
            Adicionar Card

        </button>
    </div>

</template>

<script setup>
    defineEmits(['mudaStatusCard'])

</script>