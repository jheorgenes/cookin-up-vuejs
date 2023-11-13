<template>
  <main class="conteudo-principal">
    <SuaLista :ingredientes="ingredientes" />
    <!-- O KeepAlive mantem vivo o componente (ou seja, preserva o estado do componente), mesmo após usar o v-if ou o v-for -->
    <KeepAlive include="SelecionarIngredientes">
      <SelecionarIngredientes v-if="conteudo === 'SelecionarIngredientes'"
        @adicionar-ingrediente="adicionarIngrediente"
        @remover-ingrediente="removerIngrediente"
        @buscar-receitas="navegar('MostrarReceitas')"
      />
      <MostrarReceitas v-else-if="conteudo === 'MostrarReceitas'" 
        @editar-receitas="navegar('SelecionarIngredientes')" 
        :ingredientes="ingredientes"
      />
    </KeepAlive>
  </main>
</template>

<script lang="ts">
import MostrarReceitas from './MostrarReceitas.vue';
import SelecionarIngredientes from './SelecionarIngredientes.vue';
import SuaLista from './SuaLista.vue';

/* Criando um tipo personalizado */
type Pagina = 'SelecionarIngredientes' | 'MostrarReceitas';

export default {
  data: () => ({
    ingredientes: [] as string[],
    conteudo: 'SelecionarIngredientes' as Pagina
  }),
  components: {
    SelecionarIngredientes,
    SuaLista,
    MostrarReceitas
  },
  methods: {
    adicionarIngrediente(ingrediente: string) { //O parametro ingrediente recebe o $event da emissão realizada no componente 
      this.ingredientes.push(ingrediente);
    },
    removerIngrediente(ingrediente: string) {
      this.ingredientes = this.ingredientes.filter(item => ingrediente !== item);
    },
    navegar(pagina: Pagina) {
      this.conteudo = pagina
    }
  }
};
</script>

<style scoped>
.conteudo-principal {
  padding: 6.5rem 7.5rem;
  border-radius: 3.75rem 3.75rem 0rem 0rem;
  background: var(--creme, #fffaf3);
  color: var(--cinza, #444);

  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5rem;
}

@media only screen and (max-width: 1300px) {
  .conteudo-principal {
    padding: 5rem 3.75rem;
    gap: 3.5rem;
  }
}

@media only screen and (max-width: 767px) {
  .conteudo-principal {
    padding: 4rem 1.5rem;
    gap: 4rem;
  }
}
</style>