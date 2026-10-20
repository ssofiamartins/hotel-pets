<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter(); //chamando meu router
const API_URL = 'http://localhost:3000'; //chamando minha API

const novoTutor = ref({
  nome: '',
  telefone: '',
  endereco: {
    rua: '',
    numero: '',
    bairro: '',
    cidade: '',
    estado: '',
    cep: '',
  }
});

const cepApi = ref({});

async function buscarCep(cep) {
  const API_CEP = 'https://viacep.com.br/ws/';
  const respostaCep = await fetch(`${API_CEP}/${cep}/json/`);

  cepApi.value = await respostaCep.json();
  
  novoTutor.value.endereco.rua = cepApi.value.logradouro;
  novoTutor.value.endereco.bairro = cepApi.value.bairro;
  novoTutor.value.endereco.cidade = cepApi.value.localidade;
  novoTutor.value.endereco.estado = cepApi.value.uf;

}

// salvar o novo pet no sistema
async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });

  //redirecionar para a tela de listagem de Pets
  router.push('/tutores');
}

</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Tutores</h1>
      <p class="text-body-secundary mb-0">Cadastro de tutores no sistema.</p>
    </header>

    <form class="row" @submit.prevent="salvarTutor">
      <h3>Dados do tutor</h3>
      <div class="col-md-6">
        <label for="nome" class="form-label">
          Nome do Tutor:
          <input
            type="text"
            class="form-control"
            id="nome"
            v-model="novoTutor.nome"
            placeholder="Insira o nome do tutor"
            required
          />
        </label>
      </div>
      <div class="col-md-6">
        <label for="telefone" class="form-label">
            Telefone:
            <input 
            type="tel"
            class="form-control"
            id="telefone"
            v-model="novoTutor.telefone"
            placeholder="(00) 00000-0000"
            required>
        </label>
      </div>
      <h3>Endereço</h3>
      <div class="col-md-6">
        <label for="cep" class="form-label">
          CEP
          <input 
          type="cep"
          class="form-control"
          id="cep"
          v-model="novoTutor.endereco.cep"
          placeholder="00000-000"
          @blur="buscarCep(novoTutor.endereco.cep)">

        </label>
      </div>
      <div class="col-md-6">
        <label for="rua" class="form-label">
          Rua
          <input 
          type="text"
          class="form-control"
          id="rua"
          v-model="novoTutor.endereco.rua"
          placeholder="Digite o nome da rua">
        </label>
      </div>
      <div class="col-md-6">
        <label for="numero" class="form-label">
          Número
          <input 
          type="text"
          class="form-control"
          id="numero"
          v-model="novoTutor.endereco.numero"
          placeholder="Número">
        </label>
      </div>
      <div class="col-md-6">
        <label for="bairro" class="form-label">
          Bairro
          <input 
          type="text"
          class="form-control"
          id="bairro"
          v-model="novoTutor.endereco.bairro"
          placeholder="Digite o bairro">
        </label>
      </div>
      <div class="col-md-6">
        <label for="cidade" class="form-label">
          Cidade
          <input 
          type="text"
          class="form-control"
          id="cidade"
          v-model="novoTutor.endereco.cidade"
          placeholder="Digite a cidade">
        </label>
      </div>
      <div class="col-md-6">
        <label for="estado" class="form-label">
          Estado
          <input 
          type="text"
          class="form-control"
          id="estado"
          v-model="novoTutor.endereco.estado"
          placeholder="Digite o estado">
        </label>
      </div>
      <div class="col-12-d-flex gap-2">
        <button class="btn btn-success" type="submit">
          Salvar Tutor
        </button>
      </div>
    </form>
  </div>
</template>