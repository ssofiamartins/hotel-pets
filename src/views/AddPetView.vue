<script setup>
 import { onMounted, ref } from 'vue';
 import { RouterLink, useRouter } from 'vue-router';

 const router = useRouter(); //chamando meu router
 const API_URL = 'http://localhost:3000'; //chamando minha API
 const tutores = ref([]);

 // criar objeto para salvar um novo pet
 // propriedades: nome, espécie e tutor
 const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: ''
 });

 // buscar todos os tutores que estão salvos na aplicação
 async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores `);

  //converter os dados da minha API que estão em JSON para JS
  tutores.value = await resposta.json();
  console.table (tutores.value);
 }

 // salvar o novo pet no sistema
 async function salvarPet() {
  await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet.value)
  })

  //redirecionar para a tela de listagem de Pets
  router.push('/pets');

 }

 onMounted(carregarTutores)
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Pets</h1>
      <p class="text-body-secundary mb-0">
        Cadastro de pets no sistema.
      </p>
    </header>

    <RouterLink class="btn btn-primary" :to="{ name: 'novo-pet' }">
      Adicionar Pet
    </RouterLink>

    <form class="row" @submit.prevent="salvarPet">
      <!-- nome do pet-->
      <div class="col-md-6">
        <label for="nome" class="form-label">
          Nome do Pet:
          <input type="text" class="form-control" id="nome" v-model="novoPet.nome" required>
        </label>
      </div>

      <!-- espécie do pet-->
      <div class="col-md-6">
        <label for="especie" class="form-label">Espécie</label>

        <select name="especie" id="especie" class="form-select" required v-model="novoPet.especie">
          <option value="" disabled>Selecione a espécie</option>
          <option value="Cachorro" >Cachorro</option>
          <option value="Gato" >Gato</option>
          <option value="Coelho" >Coelho</option>
          <option value="Tartaruga" >Tartaruga</option>
          <option value="Peixe" >Peixe</option>
          <option value="Mini-Vaca" >Mini-Vaca</option>
          <option value="Cavalo" >Cavalo</option>
          <option value="Pato" >Pato</option>
          <option value="Unicornio" >Unicórnio</option>

        </select>
      </div>

      <!-- nome tutor responsável pelo pet-->
      <div class="col-md-6">
        <label for="tutor" class="form-label">Tutor</label>
        <select name="tutor" id="tutor" class="form-select" v-model="novoPet.tutorId">
          <option value="" disabled>Selecione o Tutor</option>
          <option v-for="tutor in tutores" :key="tutor.id" :value="tutor.id">{{ tutor.nome }}</option>
        </select>
      </div>

      <div class="col-12-d-flex gap-2">
        <button class="btn btn-success" type="submit">Salvar Pet</button>
      </div>

    </form>
  </div>
</template>
