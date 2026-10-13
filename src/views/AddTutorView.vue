<script setup>
 import { ref } from 'vue';
 import { useRouter } from 'vue-router';

 const router = useRouter(); //chamando meu router
 const API_URL = 'http://localhost:3000'; //chamando minha API

 // criar objeto para salvar um novo tutor
 // propriedades: nome e telefone
 const novoTutor = ref({
  nome: '',
  telefone: '',
 });

 // salvar o novo tutor no sistema
 async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value)
  })

  //redirecionar para a tela de listagem de Tutores
  router.push('/tutores');

 }
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Tutores</h1>
      <p class="text-body-secundary mb-0">
        Cadastro de tutores no sistema.
      </p>
    </header>

    <form class="row" @submit.prevent="salvarTutor">
      <!-- nome do tutor-->
      <div class="col-md-6">
        <label for="nome" class="form-label">
          Nome do Tutor:
          <input type="text" class="form-control" id="nome" v-model="novoTutor.nome" required>
        </label>
      </div>

      <div class="col-md-6">
        <label for="telefone" class="form-label">
          Telefone:
          <input type="tel" class="form-control" id="telefone" v-model="novoTutor.telefone" required>
        </label>
      </div>
    

      <div class="col-12-d-flex gap-2">
        <button class="btn btn-success" type="submit">Salvar Tutor</button>
      </div>

    </form>
  </div>
</template>
