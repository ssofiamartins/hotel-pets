<script setup>
import { onMounted, ref } from 'vue';
// import { RouterLink } from 'vue-router';

const tutores = ref([]);


// chama a minha API geral:
const API_URL = 'http://localhost:3000';


// chamar a minha API para listar todos os tutores:
async function carregarDados() {

const respostaTutores = await fetch(`${API_URL}/tutores`);
tutores.value = await respostaTutores.json();

console.log('TUTORES -', tutores.value);
}


async function excluirTutor(id) {
 const confirmarExclusao = window.confirm('Deseja realmente excluir o tutor?');


 if(confirmarExclusao){
   await fetch(`${API_URL}/tutores/${id}`,{
     method: 'DELETE',
   });
   await carregarDados();
 }
}

onMounted(carregarDados);
</script>


<template>
 <div>
   <header class="mb-4">
     <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
     <p class="text-body-secondary mb-0">
       Consulte os tutores cadastrados no sistema.
     </p>
   </header>


   <table class="table table-striped table-hover">
     <thead>
       <th>id</th>
       <th>Nome</th>
       <th>Telefone</th>
       <th>Ações</th>
     </thead>


     <tbody>
       <tr v-for="tutor in tutores" :key="tutor.id" >
         <td>{{ tutor.id }}</td>
         <td>{{ tutor.nome }}</td>
         <td>{{ tutor.telefone  }}</td>
         <td>
           <RouterLink :to="`/tutores/${tutor.id}`">Visualizar</RouterLink>
           <button class="btn btn-danger" @click="excluirTutor(tutor.id)">Excluir</button>
         </td>
       </tr>
     </tbody>
   </table>
 </div>
</template>
