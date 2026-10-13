<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const API_URL = 'http://localhost:3000';
const tutor = ref(null);

async function carregarTutor() {
    const idTutor = route.params.id;

    const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`);
    console.log('respostaTutor', respostaTutor);
    tutor.value = await respostaTutor.json();
    console.log('tutor', tutor.value);
}

onMounted(carregarTutor);
</script>

<template>
 <section v-if="tutor == null">
    <p>Carregando...</p>

 </section>
 <section v-else>
    <h1>Nome do Tutor: {{ tutor.nome }}</h1>
    <p>Id: {{ tutor.id }}</p>
    <h5>Telefone: {{ tutor.telefone }}</h5>
 </section>
    
</template>
