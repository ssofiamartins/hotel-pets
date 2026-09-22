<template>
    <header class="mb-4">
        <h1 class="text-2x1 font-bold">Listagem de Pets</h1>
        <p class="texte-body-secondary mb-0">
            Listagem dos Pets cadastrados no sistema.
        </p>
    </header>

    <RouterLink class="btn btn-primary" :to="{  name: 'novo-pet'  }">
        Adicionar Pet
    </RouterLink>
</template>